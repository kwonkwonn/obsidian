
첫째, 실제 물리 컴퓨터의 핵심적으로 동일한 환경을 제공할 것
둘째, 최악의 경우라도 최소한의 성능적인 저하만 있을 것
셋째, 컴퓨터 리소스의 완벽한 제어가 가능할 것
이라는 세가지 조건을 부합하는 효율적인, 물리 컴퓨터의 복제본을 만들기 위한 이론적 환경을 제시한다.


#### 동일한 환경
동일한 환경이란, 가상화 된 컴퓨터는 호스트 컴퓨터에서 동작하는 컴퓨터와 동일한 효과(effect identical)를 내야 한다는 것이다. 
물리 컴퓨터와의 리소스 차이나 프로세서를 여러 vm들이 공유할 수 밖에 없는 점에서 발생하는 시간 지연성들은 제외 될 수 있다.

#### 효율성
효율성이란, VM을 가동하는 환경에서 최소한의 오버헤드를 일으켜야 한다는 점이다. 
조건이 다소 모호하게 느껴지기도 하지만, 해당 논문에서는 컴퓨터를 완전히 에뮬레이팅하여 실행하는 가상 머신 모델이 아니라 무해한(innocuous)한 인스럭션들은 호스트 cpu에 전달하여 vmm(가상머신 모니터)의 추가적인 오버헤드를 줄임으로써 최대한 효율적으로 만들어야 한다는 걸 의미하는 것 같다.
qemu와 같은 완전 가상화 방식의 프로젝트들은 이런 조건에 부합하지 않는 것 처럼 보이지만, kvm-qemu와 같이 하드웨어 지원 가상화와 같은 기능들이 있기 때문에 이런 전통적인 조건에 완전히 제외되 지 않을 수 있을 것 같음.

#### 리소스 제어
가상화 된 컴퓨터는 1) 가상화 된 환경에서 자신에게 부여받은 리소스가 아닌 곳에 접근 할 수 없을 것. 2) vmm이 특정 상황에서 이미 부여한 리소스를 다시 얻을 수 있을 것 이라는 조건 아래에서 VM은 자원에대한 완전한 통제를 할 수 있어야 한다.



### 3 세대 컴퓨터 모델 

3세대 컴퓨터 모델을 정의 하기위해 i/o 인스트럭션과 인터럽트가 존재하지 않는다는 가정하에 아래와 같은 컴퓨터의 상태(state)를 정의할 수 있다.
S= <E,M,P,R>
여기서 E 는 q의 사이즈를 갖는 컴퓨터 스토리지(메인 메모리라고 해석 가능), M은 user, supervisor을 갖는 인스트럭션 모드, P는 현재 프로그램 카운터, R은 (l,b)로 구성 된 가상메모리를 위한 relocation bound 의 상태이다.

여기서  E\[0]은 기존의 PSW(<M,P,R>) 을 저장하는 곳이고 E\[1]은 새로 가져올 PSW가 저장되는 곳이다.

#### trap 
여기서 instruction Trap 을 i(E1,M1,P1,R1)= (E2,M2,P2,R2)로 정의하는 데 이때, 
E2\[j] = E1\[j],
for0 <j < q, E2\[0] = (M1,P1,R1)
(M2, P2, R2) = E1\[1].
을 충족하여야 한다.

즉, trap 이 실행되면, E2\[0]에 old-PSW를 저장하는 동시에, 현재 상태를 저장하고,  E1\[1](fetch할PWS)을 현재 상태로 업데이트 하는 인스트럭션으로 이해할 수 있다.

즉, trap은 현재 인스트럭션을 블럭하는 것이 아니라, 이를 E\[0]에 저장하고, 특정 위치의 PSW(유저모드,프로그램 카운터, relocation bound register) 값을 가져와 실행하는 것으로 이해 할 수 있다. 
덕분에 특정 인스트럭션에 대해 다른 권한을 가지거나, 다른 스코프를 가지는 메모리의 인스트럭션을 실행할 수 있다.

#### instruction behavior
우리는 여러 인스트럭션들이 어떻게 행동하는지에 따라 컴퓨터의 상태를 빚대여 그룹화 할 수 있다.
##### privileged
유저모드(supervisor, user)에 따라 해당 인스트럭션이 trap을 일으키는지 아닌지가 결정 된다면 해당 인스트럭션은 privileged 한 인스트럭션이다.
여기서 privileged한 인스트럭션은 NOPing과 같은 인스트럭션으로 해소가 되지 않는다는 점이 핵심임.

##### control sensitive 
control sensitive 인스트럭션은 해당 인스트럭션의 시행 이후 <E,M,P,R>에서 M(유저모드), R<할당 레지스터>의 값을 변화 시킬 수 있는 인스트럭션을 의미한다.
여기서 control sensitive 한 인스트럭션들은 가상환경의 리소스 제어 조건에 영향을 줄 수 있다.

##### Behavior sensitive
behavior sensitive를 설명하기 위해 2가지 notation을 들 수 있다
r= (l,b)라고 할때
r@x = (l+x,b)이고 (memory allocation bound 라고 생각할 수 있다)
e | r = 가상 메모리상에서 지정하는 사용할 수 있는 메모리의 영역으로 E\[l]에서 E\[l+b]만큼의 영역이다.
S1= < e | r1, m, p, r>, S2= < e|r1@x, m, p, r1@x> 이고
각각 특정 인스트럭션 i를 시행했을 때,
e | r1 != e | r1@x 거나 p1!= p2
이면 해당 인스트럭션은 behavior sensitive한 인스트럭션이다.
즉, 어떤 인스트럭션이 레지스터가 정의 하는 크기나 위치, 혹은 유저 모드에 따라 인스트럭션의 결과가 달라진다면 해당 인스트럭션은 behavior sensitive한 인스트럭션이다.

정의에 의거하여 우리는 어떤 instruction이 control, behavior sensitive 하면 sensitive 라고 할 수 있으며, 그렇지 않다면 무해한(innocuous)한 instruction이라고 할 수 있다.

#### VMM

vmm(virtual machine monitor)은 앞서 말한 조건들을 충족하기 위해 크게 세가지의 모듈을 활용한다고 볼 수 있는데, 이는 **dispatcher**, **allocator**, **interpreter** 정도로 볼 수 있다.

###### dispatcher
dispatcher은 말 그대로 특정 상황(trap)에 따라 어떤 모듈을 불러올지 결정하는 상위의 모듈이다. 이는 hardware 가 trap을 할때 배치되어 실행된다.

###### allocator 
allocator은 vm에 자원할당을 결정하는 모듈이다. vm이 하나인 경우 vmm과 vm의 자원을 분리하는 단순한 업무를 맡지만, 여러개인 경우,  vm들 간에 중복된 자원을 할당하지 않게 관리한다.
allocator은 dispatcher가 특권모드로 자원의 할당량을 변경하는(relocation bound를 수정 한다거나) 명령을 실행할 시 호출 될 수 있다.

###### interpreter
나머지 모든 trap 들은 interpreter에 의해 관리 될 수 있다. interpreter 은 기본적으로 하나의 trap 에 한번씩 루틴을 돌며, 원본(vm 내부)의 intruction이 호스트 컴퓨터에 어떤 영향을 미칠 지 계산하고, 변환한다.

