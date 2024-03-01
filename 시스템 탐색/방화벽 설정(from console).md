
linux 에서 방화벽 설정을 도와주는 iptables.

```
sudo iptables [option] CHAIN_rule [-j target]
// 문법 기본형

sudo iptables -L
//현재상태(list)

iptables -D INPUT -s [발신지] --sport [발신지 포트] -d [목적지] 
--dport [목적지 포트] -j [정책]



sudo iptables -A INPUT -s 192.168.0.2 -p tcp --dport 555 -j ACCEPT
//192.168.0.2 에서 오는 신호의 경우 555번 포트를 허락해줘라


sudo -s iptables-save -c
// 규칙 저장 (안하면 부팅시 초기화 됨)
```





기본 추가 문법
- -A --append: Adds a rule to a string (at the end).
- -C --check: Finds a rule that matches the requirements of the string.
- -D --delete: Removes the specified rules from a string.
- -F --flush: Deletes all rules.
- -I --insert: Adds a rule to a string at a given position.
- -L --list: Displays all rules in a string.
- -N -new chain: Creates a new string.
- -v --verbose: Displays more information when using a list option.
- -X --delete-chain: Deletes the supplied string.

# 예시들

`sudo iptables -A INPUT -i lo -j ACCEPT
-A(더해라) 인풋규칙 -i 로컬 호스트 인터페이스를 -j 받아드리기로  
// 위 규칙을 더한 이후 로컬호스트 내에서 자유자재로 통신 가능

`sudo iptables -A INPUT -p tcp -dport 80 -j ACCEPT 
-A더해라 -protocol tcp프로토콜을 -dport 80포트 -j 받아드리기로

`sudo iptables -A INPUT -s 목표 ip주소 -j DROP 
-A더해라 -source(목표 ip주소를) -j 트래픽을 막을 

``sudo iptables -A INPUT -m iprange --src-range 시작 ip-끝 ip -j REJECT
-A더해라 -match iprange라는 옵션을 --src-range ip범위를 -j 거절해라


`sudo iptables -A INPUT -J DROP
만약 방화벽을 설정한다면, 위의 커맨드를 통해  다른 포트에서의 접근을 막아야 함


## 규칙 삭제

``sudo iptables -L --line-numbers
위의 커맨드를 치면 규칙이 숫자별로 할당되어 나옴
![](https://i.imgur.com/EucPled.png)


```bash
sudo iptables -D INPUT <Number>
```

그 번호를 치면 규칙이 삭제됨

```
sudo sh -c '/sbin/iptables-save > /etc/sysconfig/iptables'
```
이 커맨드만 쳐도 현재 임시로 생성한 iptable이 영구적으로 저장 됨.
sbin/iptables-save에 임시 파일이 있고
/etc/sysconfig/iptables에 저장파일이 있는듯 ㅇㅇ.



virt-manager -c 'qemu+ssh://myuser@localhost:5022/system?keyfile=id_rsa'