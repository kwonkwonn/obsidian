라우터들이 ip를 통해 인터넷에서 통신하는 것을 돕는다 
![](https://i.imgur.com/UP1jCN2.png)

- version: ip패킷의 종류를 나타낸다
  0100= ipv4, 0110= ipv6
- IHL(internet header length):
  헤더의 총 길이를 나타냄, 1은 4바이트를 의미함
  최소길이 는 5(20바이트)
  최대길이는 15(60바이트)
- DSCP(differentiated services code point):
  QOS(quality of service)를 위해 존재, 패킷의 중요도를 설정하기 위해 존재(딜레이에 민감한 통신, 전화, 비디오 콜) 
- ECN(explicit congestion notification) field:
  네트워크가 과열됐을때 패킷을 드랍에 신호를 전달함
- total length: 
  L3 header과 L4 segment 를 더한값임, 최소 20(바이트), 최대 63555(2^16 바이트).
- identifiaction field: 
  패킷이 너무 크면 분해되는데, 이때 조각이 포함된 패킷을 나타냄. MTU(maximum transmit unit, 1500바이트)를 초과하면 분해됨 
- flag fields: 
  fragments를 컨트롤/식별하기 위해 존재. (3비트로 구성)
  비트 0: 항상 0,
  비트 1: 해당 패킷이 분해 되었다는것을 알림 1.
  비트 2: 분해된 패킷이 다음에도 연속적으로 존재하는 지 알림. 1이면 뒤에 패킷 더 있음
- fragment offset: 
  조각의 순서를 표시. 프래크먼트의 도착이 순차적이지 않더라도 조합을 가능하게 함
- time to live field: 
  TTL이 0이되면 패킷을 드랍함. 무한루프 방지용
- protocol field:
  L4의 프로토콜을 지칭: 
  6= tcp 
  17= udp 
  1=icmp 
  89= ospf(dynamic routing protocol)
- header checksum : 
  체크섬을 검사해 오류가 있는지 확인함
- source ip/ destination ip: ip주소임 
- options:
  잘 안씀 걱정 ㄴ

![](https://i.imgur.com/gwTNQTm.png)
버전 4: ipv4
0101: 5 x 4=20 bytes
총 길이:100바이트
identification: 5, 분해되었다면 다른 분해 된 패킷도 5를 가졌을 것
플래그: 
패킷은 분해되지 않았다는 것을 알 수 있고 분해됐다해도 마지막 세크먼트임 
프로토콜: 1, icmp


#### 패킷이 너무 커요(분해 해야함)
![](https://i.imgur.com/x1emUvu.png)
![](https://i.imgur.com/PZLCSVa.png)
total length: 1500, 1500바이트들로 분해됨 
identification: 같은 패킷 번호 



![](https://i.imgur.com/eThMJ4M.png)
df-bit(dont fragment bit):
패킷 나누지 마라
하지만 mtu를 넘지 못해서 실패함 