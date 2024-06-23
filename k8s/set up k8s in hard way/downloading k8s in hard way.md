
```
https://storage.googleapis.com/kubernetes-release/release/v1.28.3/bin/linux/arm64/kubectl
//kubectl- 쿠버네티스 클러스터와 상호작용하는 설명줄 모듈

https://storage.googleapis.com/kubernetes-release/release/v1.28.3/bin/linux/arm64/kube-apiserver
//api server- api요청들을 처리하는 모듈

https://storage.googleapis.com/kubernetes-release/release/v1.28.3/bin/linux/arm64/kube-controller-manager
//controller manager- 여러 컨트롤러들을 관리하는 모듈

https://storage.googleapis.com/kubernetes-release/release/v1.28.3/bin/linux/arm64/kube-scheduler
//스케줄러- 새로 생긴 pod를 적절한 노드에 배치하는 모듈

https://github.com/kubernetes-sigs/cri-tools/releases/download/v1.28.0/crictl-v1.28.0-linux-arm.tar.gz
//crictl- 컨테이너 런타임과 상호작용하는데 사용되는 모듈

https://github.com/opencontainers/runc/releases/download/v1.1.9/runc.arm64
//runc- 컨테이너 런타임, 컨테이너의 생성과 실행 담당

https://github.com/containernetworking/plugins/releases/download/v1.3.0/cni-plugins-linux-arm64-v1.3.0.tgz
//cni- 가상 네트워크 구성 모듈 

https://github.com/containerd/containerd/releases/download/v1.7.8/containerd-1.7.8-linux-arm64.tar.gz
//containerd- 컨테이너의 생명주기를 담당하는 프로젝트

https://storage.googleapis.com/kubernetes-release/release/v1.28.3/bin/linux/arm64/kube-proxy
//프록시- 쿠버네티스 내부에서 네트워크 규칙과 라우팅 로드밸런싱을 담당 

https://storage.googleapis.com/kubernetes-release/release/v1.28.3/bin/linux/arm64/kubelet
//kubelet - 각 노드에서 pod와 컨테이너를 관리하는 모듈 

https://github.com/etcd-io/etcd/releases/download/v3.4.27/etcd-v3.4.27-linux-arm64.tar.gz
//etcd- 쿠버네티스 내의 모든 클러스터들의 데이터를 저장하는 모듈
```

와 같은 버전과 저장 위치를 명시 해놓은 텍스트 파일을 준비한다.

```
wget -q --show-progress \
  --https-only \
  --timestamping \
  -P downloads \
  -i downloads.txt
```
와 같은 커맨드로 모두 다운로드 받는다.

다운 받은 파일은 (ex. kubectl) 
chmod +x ./kubectl 
cp ./kubectl /usr/local/bin/
과 같은 커맨드로 실행파일 디렉토리에 넣음으로 써 실행 할 수 있다.
예) `kubectl version --client`

