
![](https://i.imgur.com/Jeg3ekI.png)
네트워크 인터페이스를 표시하는 커맨드는 동일.
switch는 라우터와 다르게 별다른 설정이 up up 상태로 

down down 이면 케이블이 연결 안된거임 
administartive down이면  shutdown커맨드로 종료한거임


- show interfaces status
![](https://i.imgur.com/c1qcY49.png)
인터페이스 상태 검색
	name- description
	 status- 연결상태
	 vlan- virtual lan
	 duplex- 한번에 데이터를 송수신 할 수 있는지 
	 speed- fast ethernet 이라서 100megabit임
	 type- 연결 종류

아래와 같이 설정가능
![](https://i.imgur.com/6IxzAfv.png)

아래 커맨드로 여러 인터페이스를 한번에 조절할 수 있다.
![](https://i.imgur.com/7KyIRYG.png)
shutdown으로 꺼버림 


half duplex- 디바이스가 데이터를 한번에 쏠 수 없음 CSMA/CD와 같은 프로토콜의 도입
허브같은 디바이스에선 허브가 리피터이기 때문에 전송중엔 기다리는 시간이 필요해음

스위치의 발명과 기술의 발전으로 현대 기기들은 full duplex로 동작함


### 기본설정들
- 다양한 속도로 설정 될 수 있는 디바이스는 (10/100 or 10/100/1000) auto duplex auto speed 로 기본 설정 됨
-  기기는 자신의 설정값을 다른 기기에 광고함. 두 기기의 최고 속도로 자동 설정 됨 
-   만약 autonegotiate를 꺼놓는다면 속도는 감지가 안되면 최저 속도로 실행, 듀플렉스는 10,100에서 반듀플랙스 그 이상에서 full deplex로 실행 


에러를 확인하기 위한 커맨드 - show interfaces