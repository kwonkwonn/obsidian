

네트워크를 새로운 스위치의 구성없이 분리 할 수 있게 함.
vlan을 구성하면 네트워크가 아예 분리된 스위치 처럼 동작
서로에게 통신 할 경우 라우터를 경유 함.


##### 설정 
1. vlan 생성
2. 인터페이스에 vlan 설정
3. trunk 포트 설정(여러 vlan 이 통과 하는 포트 설정)



![](https://i.imgur.com/sKQs71c.png)
`` show vlan
vlan 확인 
``vlan n 
n 가중치로 vlan 생성
그 후, vlan 으로 설정하려는 인터페이스로 이동
`` switch port mode access
인터페이스 모드를 access로 수정
`` switchport access vlan n
연결하려는 vlan에 할당


``switchport mode trunk
모드를 trunk 로 변경 
``switch port trunk allowed vlan n, n1, n2
허용할 vlan 을 설정 할 수 있음 

`` switchport trunk encapsulation dot1q
각각의 인터페이스를 분리하기 위해 암호화를 하여줌 

`` #show interface xx switchport
인터페이스 설정을 확인 

#### sub interface 
라우터의 한 물리적 인터페이스에서 여러개의 로컬 네트워크 트래픽을 처리할 때 설정 

``int gigabitEthernet 0/0/0.(vlan id)
`` encapsulation dot1Q (vlan id)
``ip addr (ip) (ip mask)
ip 를 설정 


