
![](https://i.imgur.com/Dx1uj6y.png)
layer 2 프레임의 구성도 


# header
## preamble
프레임의 시작부분에 위치, 7바이트로 10101010의 반복으로 구성
기기가 데이터를 읽을 준비를 하는데 사용

## SFD
10101011로 구성.
preamble이 이제 끝나고 다음 프레임 데이터가 있을것을 알려줌
1바이트

## destination source
출발지와 목적지의 MAC주소를 넣어서 보냄
각 6바이트

## Type or Length
값이 1500이하면 보통 길이를 의미함
1536보다 크거나 같을경우 패킷의 종류를 의미 함(ipv4 or ipv6)
2바이트


# Trailer
## FCS 
frame check sequence 
4바이트
CRC(Cyclic Redundancy Check)알고리즘을 통해서 오류를 검출함


MAC 주소

6바이트로 구성
첫 3바이트는 제조사에서 할당 됨
12글자의 16진수로 구성


How switch work
![](https://i.imgur.com/Xu3c9M5.png)

스위치는 자신의 인터페이스에 할당 된 테이블을 가지고 있고, 특정 인터페이스로 프레임이 들어와서 읽을 때, 해당 프레임의 source 부분을 읽고 동적으로 매핑하여 학습한다.
동적 매핑은 5분후 테이블에서 삭제된다

만약 테이블을 찾아보더라도 데이터를 찾을 수 없을경우(unknown unicast frame) flood를 실행해 모든 인터페이스에 패킷을 전송한다

## 이더넷 프레임

프레임의 최소 크기는 64바이트 이다.
보통 preamble+sfd는 이더넷 헤더에 포함이 안됨
그럼 총 6+6+ 4+2 = 18바이트 정도이고, 
덕분에 최소 payload는 64-18= 46바이트 정도임 
만약 패킷데이터가 46보다 작다면 0으로 구성된 더미데이터가 추가됨 


만약 IP주소는 알지만 MAC 주소를 모른다면?

스위치는 layer2 기기 이기때문에 MAC주소는 필수적으로 알아야함 그럴때 사용하는게 
ARP(address resolution protocol)이다

arp는 arp request 와 arp reply로 구성되는데, arp request 와 같은경우는 브로드캐스트로, 그에 대한 응답은 유니캐스트 임 


![](https://i.imgur.com/Z01J4zB.png)

arp req 패킷은 이렇게 생겼다

![](https://i.imgur.com/wz7BCC7.png)
arp reply 패킷예시


![](https://i.imgur.com/Vi5XGfS.png)
arp -a커맨드 시 등록된 arp table을 열람가능하다

in IOS
show arp
arp열람
show mac address-table 
맥 주소록 열람 
clear mac address-table dynamic 
주소록에 다이나믹 테이블 지우기 
clear mac address-table dynamic  (address 주소 or interface 포트)도 가능
![](https://i.imgur.com/vbTGGr1.png)

