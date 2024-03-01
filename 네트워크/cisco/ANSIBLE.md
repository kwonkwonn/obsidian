
terraform 보다 개별 장비에 대한 세부적인 컬렉션을 제공함.


ansible

playBooK - yaml 을 통해서 내부적으로 실행해야하는 명령어를 선언하면, playbook이 실행함 .
variables - 변수를 하드코딩하는게 아니라 variables에서 저장함
inventory- 사용장비들의 식별 정보를 저장


ansible -> control node 에서 코들르 보관하고 있다가, ssh 연결을 통해서 대상 하드웨어를 제어함


192.168.13.113



```
host: 호스트
gather_facts: false

  tasks:

    - name: 이름

      cisco.nxos.nxos_facts://모듈 가져오기 

        gather_subset: all

    - name: Show Gathered Facts

      debug:

        msg: "{{ ansible_facts }}"// 변수 출력


```



```
- hosts: nexus

  gather_facts: false

  tasks:

    - name: Run show version on Remote Device

      cisco.nxos.nxos_command:

        commands: 
        - show version 
		- clear  // 실행시킬 커맨드
		
		wait_for: result[0] contains hyeyeon 
		// 특정 값이 포함되어 있는지 확인. 포함되어 있지 않을경우 에러 

      register: results  //변수 저장 

    - name: Show results

      debug:

        msg: "{{ results.stdout_lines }}" // 변수에서 꺼내옴
```
![](https://i.imgur.com/Rb9EsM2.png)
실제 스위치에서 실행했을 때의 결과