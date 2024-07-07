
노드들 사이의 통신의 보안성을 위해서 TLS(Transport Layer security) certificate 설정을 한다.
TLS 인증은 kub-apiserver, controller, scheduler, kublet, kubeproxy등에 설정 된다.

openssl을 통해 설정하는 것은 시간이 많이 소요되는 작업이기 때문에, 홈랩을 구성하는 경우 사전에 ca.conf에 설정한 파일을 이용하고, 직접 생성한 프라이빗 키와 루트 인증서를 사용하여 설정할 예정이다.
이는 실제 프로젝트에서는 좀 더 견고한 방법으로 진행되어야 함.

```
{
  openssl genrsa -out ca.key 4096
  openssl req -x509 -new -sha512 -noenc \
    -key ca.key -days 3653 \
    -config ca.conf \
    -out ca.crt
}
```
위와 같은 코드로 진행 할 수 있다.
openssl 로 rsa 키를 만드는 것을 확인 할 수 있는데 이는 ssh-keygen에서 하는 방식과 유사한 것 같다. 같은 rsa를 쓰는 듯.
그 아래줄은 x.509 인증서를 생성하는 것 같다. 볼 수 있듯이 ca.key 와 ca.conf 를 참고하여 ca.crt의 파일을 만드는 모습이고, -day를 통해 유효기간(3653일)을 설정할 수 있다.

이 과정을 통해 ca.crt(클라이언트 용), ca.key(서버 용)을 생성할 수 있는데 전형적인 비밀키/공개키 모델을 생각하면 좋을 것 같다.




```
certs=(
  "admin" "node-0" "node-1"
  "kube-proxy" "kube-scheduler"
  "kube-controller-manager"
  "kube-api-server"
  "service-accounts"
)

for i in ${certs[*]}; do
  openssl genrsa -out "${i}.key" 4096

  openssl req -new -key "${i}.key" -sha256 \
    -config "ca.conf" -section ${i} \
    -out "${i}.csr"
  
  openssl x509 -req -days 3653 -in "${i}.csr" \
    -copy_extensions copyall \
    -sha256 -CA "ca.crt" \
    -CAkey "ca.key" \
    -CAcreateserial \
    -out "${i}.crt"
done
```
각각의 컴포넌트들에게 키와 인증서를 생성하게 하기 위한 자동화 코드이다. 
앞선 코드를 셸 스크립트를 통해 구현 한 것 같다. 
각각의 이름.key 와 이름.csr을 제작함.

```
for host in node-0 node-1; do
  ssh root@$host mkdir /var/lib/kubelet/
  
  scp ca.crt root@$host:/var/lib/kubelet/
    
  scp $host.crt \
    root@$host:/var/lib/kubelet/kubelet.crt
    
  scp $host.key \
    root@$host:/var/lib/kubelet/kubelet.key
done
//각각의 노드에게 적절한 인증서와 키를 인증서를 복사한다. 노드들의 관리와 노드들관의 상호작용을 하는 kubelet에 설치한 모습.

scp \
  ca.key ca.crt \
  kube-api-server.key kube-api-server.crt \
  service-accounts.key service-accounts.crt \
  root@server:~/
//서버 노드에 설정파일들을 복사한다
```


그 후 kubernetes의 클러스터를 설정하는 kubernetes config file 을 생성해, 올바른 위치에서 인증파일과 키 파일을 참고 할 수 있게 하자.

기본적으로  마스터노드(서버/컨트롤 플레인)와 워커노드의 관계를 설명하면,
워커노드에는 
- Kubelet
- kube-proxy
- container runtime 등이 있고,
마스터 노드에는 
- kubernetes api-server
- etcd
- Kubelet
- controlloer-manager
- scheduler
- kube-proxy
등이 있어, 마스터 노드에서 워커노드의 다양한 파드와 컨트롤러등을 관리한다고 할 수 있다.

이를 통해 왜 대다수의 컴포넌트의 비밀키와 인증서를 서버 노드에 복사하고, 워커 노드에는 Kubelet에만 복사했는지 알 수 있다.
####  worker node conf
```
for host in node-0 node-1; do
  kubectl config set-cluster kubernetes-the-hard-way \
    --certificate-authority=ca.crt \
    --embed-certs=true \
    --server=https://server.kubernetes.local:6443 \
    --kubeconfig=${host}.kubeconfig

  kubectl config set-credentials system:node:${host} \
    --client-certificate=${host}.crt \
    --client-key=${host}.key \
    --embed-certs=true \
    --kubeconfig=${host}.kubeconfig

  kubectl config set-context default \
    --cluster=kubernetes-the-hard-way \
    --user=system:node:${host} \
    --kubeconfig=${host}.kubeconfig

  kubectl config use-context default \
    --kubeconfig=${host}.kubeconfig
done
```
각각의 워커 노드 클러스터를 설정하는 코드이다.
첫번째 단락: 클러스터의 이름 등 클러스터의 정보를 설정
두번째: 사용자의 자격증명을 설정한다.
세번째: 컨택스트 설정
네번째: 현재 컨택스트를 설정한다.

####  kube-proxy conf
```
{
  kubectl config set-cluster kubernetes-the-hard-way \
    --certificate-authority=ca.crt \
    --embed-certs=true \
    --server=https://server.kubernetes.local:6443 \
    --kubeconfig=kube-proxy.kubeconfig

  kubectl config set-credentials system:kube-proxy \
    --client-certificate=kube-proxy.crt \
    --client-key=kube-proxy.key \
    --embed-certs=true \
    --kubeconfig=kube-proxy.kubeconfig

  kubectl config set-context default \
    --cluster=kubernetes-the-hard-way \
    --user=system:kube-proxy \
    --kubeconfig=kube-proxy.kubeconfig

  kubectl config use-context default \
    --kubeconfig=kube-proxy.kubeconfig
}
```
kube-proxy를 설정하는 코드임.
위의 코드와 동일하게 클러스터의 이름과 기본 내용을 설정한 후, kube-proxy.kubeconfig 와 같은 파일을 생성한다.
그 후 생성 된 파일에 누적적으로 새로운 설정들을 덧붙이는 방식인 것 같다.
마찬 가지로 클러스터 설정, 인증방법 설정, 컨택스트 등의 설정을 추가한다.

#### kube-controller-manager conf
```
{
  kubectl config set-cluster kubernetes-the-hard-way \
    --certificate-authority=ca.crt \
    --embed-certs=true \
    --server=https://server.kubernetes.local:6443 \
    --kubeconfig=kube-controller-manager.kubeconfig

  kubectl config set-credentials system:kube-controller-manager \
    --client-certificate=kube-controller-manager.crt \
    --client-key=kube-controller-manager.key \
    --embed-certs=true \
    --kubeconfig=kube-controller-manager.kubeconfig

  kubectl config set-context default \
    --cluster=kubernetes-the-hard-way \
    --user=system:kube-controller-manager \
    --kubeconfig=kube-controller-manager.kubeconfig

  kubectl config use-context default \
    --kubeconfig=kube-controller-manager.kubeconfig
}
```

#### kube-scheduler conf
```
{
  kubectl config set-cluster kubernetes-the-hard-way \
    --certificate-authority=ca.crt \
    --embed-certs=true \
    --server=https://server.kubernetes.local:6443 \
    --kubeconfig=kube-scheduler.kubeconfig

  kubectl config set-credentials system:kube-scheduler \
    --client-certificate=kube-scheduler.crt \
    --client-key=kube-scheduler.key \
    --embed-certs=true \
    --kubeconfig=kube-scheduler.kubeconfig

  kubectl config set-context default \
    --cluster=kubernetes-the-hard-way \
    --user=system:kube-scheduler \
    --kubeconfig=kube-scheduler.kubeconfig

  kubectl config use-context default \
    --kubeconfig=kube-scheduler.kubeconfig
}
```

#### admin conf
```
{
  kubectl config set-cluster kubernetes-the-hard-way \
    --certificate-authority=ca.crt \
    --embed-certs=true \
    --server=https://127.0.0.1:6443 \
    --kubeconfig=admin.kubeconfig

  kubectl config set-credentials admin \
    --client-certificate=admin.crt \
    --client-key=admin.key \
    --embed-certs=true \
    --kubeconfig=admin.kubeconfig

  kubectl config set-context default \
    --cluster=kubernetes-the-hard-way \
    --user=admin \
    --kubeconfig=admin.kubeconfig

  kubectl config use-context default \
    --kubeconfig=admin.kubeconfig
}
```
이전 코드들과 달리 차이가 있는 것을 확인 할 수 있다.


#### copying file
```
for host in node-0 node-1; do
  ssh root@$host "mkdir /var/lib/{kube-proxy,kubelet}"
  
  scp kube-proxy.kubeconfig \
    root@$host:/var/lib/kube-proxy/kubeconfig \
  
  scp ${host}.kubeconfig \
    root@$host:/var/lib/kubelet/kubeconfig
done
```
생성한  컨피그 파일들을 각각의 워커노드에 배치한다.
각각의 컴포넌트가 가장 먼저 kubeconfig를 참고한다 하였으니 설정대로 행동할 것 같다.

```
scp admin.kubeconfig \
  kube-controller-manager.kubeconfig \
  kube-scheduler.kubeconfig \
  root@server:~/
```
서버(마스터노드)에 각각의 설정파일을 복사한다.