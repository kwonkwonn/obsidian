![[Pasted image 20231004165645.png]]
프로세스의 단계들
	new : 새로 태어난 아이
	ready: 대기 상태 (바로 실행되는건 아님, 바로 cpu에 올리면 구동 가능한 상태), 라운드 로빈 스케줄링에서는 running 상태에서 끝나면 ready 상태로 이전 됨
	running: cpu에서 할당 받아서 돌아가고 있음
	waiting: 대기하고 있는상태(ex.입력을 기다리고 있는상태, 계속 running 상태면 cpu의 낭비임), 특정 조건이 만족되기 전까진 cpu를 사용하는게 의미없는 상태



time slicer- 프로세스별 할당 된 cpu 가용시간 

라운드 로빈 스케줄링 : 모든 프로세스가 공평하게 줄을 서서 cpu에 접근,사용하는 방식
매우 빠르게 진행되기 때문에 사용자는 여러개의 프로세스가 '동시에' 돌아가는듯한 착각을 줌


**PROCESS STATE**
- D .  uninterruptible sleep (usually IO) -값을 기다리는 상태
- R.  runnable (on run queue)- 큐에 있는 상태 
- S.  sleeping
- T. traced or stopped. -디버거 생각하면 됨, 타임 스또프
- Z. a defunct("zombie") process


