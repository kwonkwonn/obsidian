
## stp(spanning tree protocol)


stp 는 네트워크 상에서 스위치와 같은 단말이 서로 연결 되어 있는 상황에서 발생할 수 있음.

broadcast와 같은 통신이 일어날 때, 해당 패킷이 트리 내부에서 무한히 순환 가능.

이를 해결 하기 위한것이 stp임.

![](https://i.imgur.com/g2SGymf.png)
아래와 같이 무한 순회 구조가 만들어 졌을 때, 한 스위치를 root bridge로 설정할 수 있음. 
root bridge 는 모든 포트로 configuration BPDU(bridge protocol data unit)를 송신함.
configuration 은 hello interval(2초)마다 전송
TCN BPDU는 토폴로지에 변화가 생긴경우 전송 


```
show spanning tree 커맨드로 루트 브릿지의 정보를 알 수 있음.
```

스위치가 연결 된 초기에는 모두 스스로가 root bridge 라고 여김
연결 직후 자신의 priority 와 인터페이스의 mac주소를 모두에게 전송(configuration BPDU)

전달 받은 스위치는 자신의 priority 와 패킷상의 priority 를 비교함.
priority 가 더 낮은 스위치가 root 로 설정 됨.
같은 경우 더 낮은 mac주소를 가진 스위치가 설정 됨.

`` debug spanning-tree events
커맨드를 통해 로그를 확인 할 수 있음

```
spanning-tree vlan 1 root primary
// 해당 스위치를 루트로 만듬
spanning-tree vlan 1 priority ?
// 해당 스위치의 프라이어티를 조정함
```


#### root port 설정
1. cost 를 비교하여 root bridge 와 가장 가까운 포트 1개 선정
2. priority 가 낮은 스위치와 연결 된 포르로 설정
3. system mac이 낮은 스위치와 연결 된 포트로 선정
4. 패킷을 보낸 상대방 포트의 priority 가 낮은 포트로 선정
5. 패킷을 보낸 상대의 포트의 낮은 포트 번호를 선정
![](https://i.imgur.com/6S6Bx9w.png)


네트워크 상태 
- blocking :bpdu도 차단 됨
- listening: forwarding delay 만큼 기다 린 후 learning으로 변환
- learning: mac주소를 학습하는 단계, forwarding delay 만큼 기다린 후 forwarding 상태로 이동
- forwarding: 정산 통신

만약 중간 다리 역할을 하던 스위치가 끊어진다면, 스위치는 bpdu를 기다리다가, learning, forwarding 상태를 거쳐 토폴로지를 변화시킴 

루프가 생기지 않을 것이라 확신 할 수 있는 토폴로지는 
`portfast`를 사용하여 forwarding 시간을 기다리지 않아도 됨 
`spaaning-tree portfast`




