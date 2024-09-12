

```
kubectl get pod -l app={deploy 이름} --output jsonpath='{.items[0].status.podIP}'
//파드의 ip 주소를 확인 할 수 있음

kubectl exec deploy/{deploy1 이름} -- ping -c $(kubectl get pod -l app={deploy2 이름} --output jsonpath='{.items[0].status.podIP}')
//특정 파드의 ip주소로 핑을 보낼 수 있음
```
그러나 파드의 ip주소는 삭제 후 재생성 될때 마다 바뀌므로 이런한 방법은 꽤 불안정하다.

이를 위해 DNS 서버 서비스를 사용할 수 있는데, 기존 DNS처럼 서비스의 이름을 pod의 ip 주소와 연동하여 사용할 수 있다.
![](https://i.imgur.com/0U4j2pL.png)
이와 같이 서비스를 짤 수 있다.
metadata.name 이 도메인 네임을 설정하는 부분이라 볼 수 있고,
spec.selector.app의 이름이 sleep-2인 파드들이 대상이다(deployment에서 matchlabel만 빠진듯.)


```
똑같히 kubectl apply 명령어로 실행 할 수 있고, 
get svc {서비스 이름}으로 검색 할 수 있다.

이제 서비스에서 지정한 도메인 이름으로 통신을 할 수 있다{ex. sleep-2}

```


## 로드밸런서 서비스

로드밸런서 서비스는 외부에서 오는 신호를 파드에 전달하는 간단하고 유연한 방법이다. 로드밸런서 서비스의 범위는 클러스터 전체로써 클러스터 내부의 어떤 노드나 파드에 신호를 전달 할 수 있다,
![](https://i.imgur.com/NxnUWL7.png)
간단한 로드밸런서 선언 코드
8080번 포트를 주시하고 있다가 해당 포트로 들어오는 트래픽을 80번으로 전달하는 역할을 한다.
// spec.port를 spec.ports로 변경



익스터널 네임

헤드리스 서비스