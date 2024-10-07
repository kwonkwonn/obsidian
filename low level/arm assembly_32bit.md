sp: stack pointer(램상의 특정 스택을 참고하고 있을때 주소값을 저장함)
lr: return(함수를 종료할 때 return 값을 저장하는 레지스터)
pc: program counter(프로그램상에서 참고 하고 있는 부분의 주소값을 지정함)
cpsr, spsr...: 특정 오퍼레이션 이후 그 오퍼레이션의 결과값을 나타내는 플레그들( 음수인지, 캐리가 있었는지 등등..)

reg7 : 호출하고 싶은 시스템콜의 주소를 저장하는 레지스터
시스템콜을 실행하는 방법<--- interrupt를 일으킨다.
r7의 주소로 이동함.

```
.global _start //_start 라벨을 글로벌로 지정함 
_start: /// <--- 특정 코드들을 나누는 라벨임, 프로그램이 시작되는 장소를 지정함

```

## Intructions

MOV  **des reg**,**src reg**
//src의 데이터를 des reg로 옮긴다.
MOV **des reg**, \#int 
//int값을 des로 옮김
ex.) 
```
MOV R7, #1   R7에 1(terminate program)의 주소를 삽입
SWI 0 <--- software interrupt를 일으킴.
//자동으로 R7의 주소값을 읽고 소프트웨어를 종료시킴
```
ADD  **RD**,**RN**,**#const**
ADD  **RD**,**RN**,**RM**
두번째, 세번째 값을 더한 후, RD에 옮긴다

SUB **RD**,**RN**,**#const**
SUB **RD**,**RN**,**RM**
두번째, 세번째 값을 뺀후, RD에 옮김.

CPSR(current program status register)
![](https://i.imgur.com/lcOhC61.png)

  assembly 는 매 인스트럭션 마다 상태(cpsr)이 리프레시가 되지 않고, 특정 인스트럭션 이후 그 상태를 업데이트하고 참고 한다.
  NZCV를 봐도 add 나 sub등의 인스트럭션 이후 어떻게 특정 상태의 플래그를 설정하는 지 알 수 있다.


