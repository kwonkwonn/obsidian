

![](https://i.imgur.com/wwRINkm.png)

show ip interface brief

	-interface 연결된 인터페이스
	-ip 주소 표시
	-OK 레거시임 
	-method ip주소가 할당된 방식을 표현
	-status layer1연결의 상태로 봐도 무방함, 양쪽에 케이블이 잘 연결돼 있으면 up이라고 뜸, shutdown커맨드로 껏거나 라우터의 경우 디폴트로 administratively down이라고 표현됨. 스위치와 같은경우 디폴트로 up임(연결이 잘 되어있는경우) 
	-protocol status와 달리 layer2의 연결 상태임
	

# 설정하는법 
	configure terminal 를 통해 글로벌 설정보드로 진입
	 interface gigabitethernet 0/0 커맨드를 통해 인터페이스 조작가능
	 ip address (ip 주소 : 10.255.255.254) (subnet : 255.0.0.0)
	 no shutdown을 해준다 
![](https://i.imgur.com/fMLqR6u.png)

결과
![](https://i.imgur.com/tdNeHRR.png)



![](https://i.imgur.com/NABgm3c.png)

show interfaces g0/0 
![](https://i.imgur.com/TNuufjk.png)

description을 표시하는 명령어(왜 설치했는지 알 수 있음 )
![](https://i.imgur.com/dsJ3kfn.png)



![](https://i.imgur.com/8qoGdDw.png)
