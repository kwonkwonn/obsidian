프로세스는 맨 처음 생겨나고 아래의 상태주기를 몇번이고 오갑니다.
![](https://i.imgur.com/QNVO1be.png)
프로세스의 단계들
	new : 새로 태어난 아이
	ready: 대기 상태 (바로 실행되는건 아님, 바로 cpu에 올리면 구동 가능한 상태), 라운드 로빈 스케줄링에서는 running 상태에서 끝나면 ready 상태로 이전 됨
	running: cpu에서 할당 받아서 돌아가고 있음
	waiting: 대기하고 있는상태(ex.입력을 기다리고 있는상태, 계속 running 상태면 cpu의 낭비임), 특정 조건이 만족되기 전까진 cpu를 사용하는게 의미없는 상태



**PROCESS STATE**
- D .  uninterruptible sleep (usually IO) -값을 기다리는 상태
- R.  runnable (on run queue)- 큐에 있는 상태 
- S.  sleeping(대부분의 대기상태, 입력을 기다리고 있을때도)
- T. traced or stopped. -디버거 생각하면 됨, 타임 스또프
- Z. a defunct("zombie") process


