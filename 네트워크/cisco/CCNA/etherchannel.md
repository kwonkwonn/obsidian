여러 인터페이스를 하나로 묶어서 논리적으로 하나의 인터페이스 처럼 사용하는 기술 


**프로토콜 종류:** LACP(Link Aggregation Control Protocol), PAgP(Port Aggregation Protocol)

etherchannel 을 활용하지 않고 여러 인터페이스를 동시에 연결하면, stp 때문에 하나의 인터페이스에서만 통신이 활성화 됨

생성 과정
`interface port-channel n`
포트 채널 생성
`interface range gig~~ 0/1-4`
여러개의 인터페이스를 한번에 잡음
`channel-group n mode <mode>`
![](https://i.imgur.com/GPbHAxq.png)

`show etherchannel summary`
이더채널 구성 확인 

L3에서 만드는 법
`int port-channel n`
`ip address (ip) (ip mask)`
`no shutdown`

나머지는 동일


