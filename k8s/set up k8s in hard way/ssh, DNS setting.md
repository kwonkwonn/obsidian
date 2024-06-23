
administrator 노드에 1차적으로 ssh를 연결 한 후, 그 노드에서 다른 노드들에 ssh연결을 시켜줘야 한다.

첫번째로는 debian에서 기본적으로 막혀있는 root 계정 ssh 연결을 뚫어준다.
/etc/ssh/sshd_config 에 
\#PermitRootLogin 을 yes로 바꿔 주면 된다.

모든 연결 노드해 실행한다.

그 후
``ssh-keygen
로 ssh키를 생성해 주고,
`` ssh-copy-id root@{ip}
를 통해서 키를 전달한다.


그 후, /etc/hosts파일을 수정하여 개개의 컴퓨터에 DNS를 설정할 수 있다.
각각의 노드의 /etc/hosts 파일의 127.0.1.1 주소(추가 루프백 주소)에 각 노드의 이름과 

``hostnamectl 커맨드를 이용하여 노드이 이름을 자신의 DNS에 사용할 이름이로 맞춰준다.

또, 각각의 노드에 모든 노드와 컨트롤 노드의
ip   address hostname 이 포함 된 파일을 업데이트 할 수 있다.
이런 DNS는 어떤 주소에 접근할 때, 매번 ip주소를 입력할 필요를 없앨 수 있다.

