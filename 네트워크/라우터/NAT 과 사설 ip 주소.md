
만약 디바이스나 서버 별로 하나의 ip주소를 할당 받았다면, ip 주소는 진작에 포화 했을 것입니다.
또한, 네트워크 관리자로서 ip주소를 배정 받아 사용할때 모든 디바이스를 공인 ip로 연결한다면 
매 Ip주소를 관리하기 어려워 집니다.

왜냐하면 크기가 다른 ip주소를 배정받을 가능성이 있을뿐만 아니라 사용하지 않는 ip주소는 연결을 끊어 버리기 때문에 필요할때, 효과적으로 배정하기 힘듭니다.

그렇기 때문에 사설 Ip주소를 사용하는 것을 추천하는데, 사설 ip는 사실 인터넷에 연결 되어있지 않더라도 구축가능합니다.

직접 공인 ip주소를 뚫는것은 KISA와 같은 할당기관을 통해서 힘들게 뚫거나 공유기를 사용해서 할 수 있습니다.
공유기에 NAT기능을 사용하면 로컬 ip를 가지면서도 공인 ip를 통해 인터넷에 연결할 수 있습니다.
일반적으로 공유기에서 많이 사용되는 192.168.0.0/16은 할당 되어있는 네트워크 주소로 로컬 네트워크의 ip주소로 많이 사용됩니다.

192.168.0.1에 접근하는 경우 현재 사용하는 공유기에 접근하여 포트포워딩 등을 할 수 있습니다
