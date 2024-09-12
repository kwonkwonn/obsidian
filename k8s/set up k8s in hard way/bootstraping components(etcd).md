
쿠버네티스 컴포넌트들은 기본적으로 stateless한 상태이고, 이러 한 상태들을 etcd라는 모듈에 저장한다. 
etcd는 분산되어 저장하고, 분산된 상태에서 적절히 데이터를 유지하기 위해서 컨샌서스 과정을 거쳐 하나의 etcd 노드가 다운 되더라도 리더 선정을 거쳐 쿠버네티스가 원활히 작동하도록 돕는다.


```
scp \
  downloads/etcd-v3.4.27-linux-arm64.tar.gz \
  units/etcd.service \
  root@server:~/
```
다운로드 된 etcd tar 파일을 서버 노드의 units/etcd.service로 복사한다

```
{
  tar -xvf etcd-v3.4.27-linux-arm64.tar.gz
  mv etcd-v3.4.27-linux-arm64/etcd* /usr/local/bin/
}
```
그 후, tar 명령어를 통해 복사 된 파일을 해제한다.
/usr/local/bin 파일에 있는 etcd, etcdctl 의 바이너리 파일을 /usr/local/bin에 복사하여 등록한다.
```
{
  mkdir -p /etc/etcd /var/lib/etcd
  chmod 700 /var/lib/etcd
  cp ca.crt kube-api-server.key kube-api-server.crt \
    /etc/etcd/
}
```
설정에 사용되는 /etc/etcd 와 데이터를 저장하는 /var/lib/etcd 폴더를 생성합니다.
권한을 변경한 후, 이전에 생성한 kube-api-server와의 통신을 위한 인증서를 설정 폴더에 저장합니다.
etcd는 kubelet과 같은 모듈과 직접적인 통신을 하기 보단, kube-api-server서버를 통해서 접근한다고 합니다.

```
mv etcd.service /etc/systemd/system/
```
etcd.service를 등록하여, systemctl로 관리 할 수 있도록 합니다.


```
{
  systemctl daemon-reload
  systemctl enable etcd
  systemctl start etcd
}
```

