

### Administrative Distance
![](https://i.imgur.com/TnhKwFz.png)
여러 정보를 가지고 있을 때, ad 를 참고하여 라우팅 전략을 설정함.


## OSPF(Open Shortest Path First)

Link-state protocol
- 인접한 라우터의 연결 정보를 의미
- LSA(link-state advertisement)를 이웃한 라우터끼리 교환하고 LSDB(link-state database)를 각자 구축


`ip ospf 1 area n`
ospf를 설정함
`show ip ospf neighbor`
인근 ospf를 보여줌 

ospf에서 area 별로 라우팅 정보를 관리함.
라우팅 업데이트는 area안에서 유지되기 때문에, 테이블 크기가 줄고 안정성이 높음

#### backbone area 
ospf에는 area 0이 항상 존재 해야하고, 모든 area는 이 backbone area 에 연결 되어야 함.
ospf네트워크 내의 라우팅 정보를 교환하는 중심 역할을 함.


- internal router 내부 라우터
- area border router 영역 경계 라우터(둘 이상의 ospf 연결)
- autonomous system boundary router(다른 프로토콜을 가지는 area 와도 연결)

router ID
동적 라우팅 프로토콜에서 각 라우터를 고유하게 식별하기 위해 사용되는 id
32비트 IPv4주소 형식을 따름

`interface loopback n`
`ip addr (ip) (ip mask)`
`ip ospf 1 area 0`
ospf를 할당하기 위한 루프백 ip를 설정함


`router ospf n`
`router-id (id)`
ospf내의 id를 할당함. 

``