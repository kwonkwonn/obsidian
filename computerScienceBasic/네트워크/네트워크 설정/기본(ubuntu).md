
네트워크에 관한 기본 설정들은 etc/network/~에 있었지만,
신버전에서는 netplan이라는 파일로 관리함.
만약 없다면 netplan.io라는 패키지를 설치해야 함

net-tools 라는 패키지는 ifconfig, netstat, hostname 과 같은 명령어들이 포함되어 있음

ifconfig로 네트워크 설정을 알수 있지만, ip 명령어로도 알아낼 수 있음

ip address는 다양한 네트워크 정보를 알려주고,
ip -s link 명령어는 인터페이스로 송수신되는 패킷정보를 확인할 수 있음.
인터페이스별로 정상적으로 입력(rx), 출력(tx), 에러 패킷 수 등을 확인할 수 있음.

우분투에서 네트워크 서비스를 제어하기 위해선 
```
/etc/init.d/networking start
/etc/init.d/networking stop
/etc/init.d/networking reload
/etc/init.d/networking restart
```
과 같은 명령어를 사용함