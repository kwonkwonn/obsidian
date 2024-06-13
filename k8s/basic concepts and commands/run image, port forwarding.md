
kubectl 은 쿠버네티스 클러스터에 접속하여 쿠브 api를 이용하여 클러스터를 관리한다.

minikube를 사용하여 mac os에 설치 후 kubectl 과 연결을 한 상태.
minikube start --device=docker <-- minikube docker에 실행하기 

```
kubectl get nodes
//클러스터에 포함 된 모든 노드 출력 Ready 면 클러스터를 정상적으로 사용가능

kubectl run {pod 이름} --image={사용자 이름}/{이미지 이름}
// dockerd에서 실행시 도커 허브의 사용자 이름의 레포를 찾아가 이미지를 로드에 {이름}으로 실행시키는 커맨드

kubectl wait --for=condition=Ready pod {이름}
//pod 가 준비 될때까지 기다린다 

kubectl get pods 
//모든 파드의 목록을 출력한다

kubectl describe pod {파드 이름}
```
![](https://i.imgur.com/9KWivdG.png)
describe시 나오는 내용 

이렇게 생성 된 파드는 한 노드에 배정되고 이러한 파드를 관리하고 파드에 포함 된 컨테이너를 실행하는 책임도 이 노드가 맡는다.
이러한 과정은 컨테이너 런타임 인터페이스(CRI)라는 공통 된 api를 이용하여 컨테이너 런타임과 연동되는 형태로 진행 됨. 


minikube ssh 를 통해 minikube 컨테이너 상으로 접속 후 컨테이너 리스트를 확인 후 컨테이너를 삭제해 보았다.
![](https://i.imgur.com/UZHYdwx.png)

여전히 파드는 살아있었고
![](https://i.imgur.com/6rOys3W.png)
직후에 컨테이너를 확인해 보니 컨테이너가 부활 한 것을 확인할 수 있었음
![](https://i.imgur.com/vjetcR7.png)
컨테이너를 감싸고 있던 파드가 자체척으로 이미지가 삭제 된 것을 확인하고 다시 실행 한 것 같다.


### 트래픽 조절
```
kubectl port-forward pod/{pod이름} {로컬 포트}:{파트 포트}
// 로컬 컴퓨터(노드)의 특정 포트로 들어오는 트래픽을 파드 포트로 포워딩 하는 역할을 수행한다.
```
