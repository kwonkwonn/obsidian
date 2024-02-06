instance를 만들면 keyPair 를 만들 수 있음(ssh 에서 쓰는 방식)
ip주소를 하나 할당 해주는데, 이를 이용해서 터미널에서 ssh 연결을 할 수 있음
- key의 관리 권한을 0400으로 주는것을 잊지말것 
```
ssh -i (key_pair) ec2-user@(ip-address) 
```
하면 연결 됨.


인스턴스 종료 후 다른 크기의 인스턴스로 교체할 수 있음 
만약, EBS(인스턴스 규격)이 호환되는 인스턴스일 경우 인스턴스를 교체 한 후에도 이전에 남아있던 데이터를 계속 사용할 수 있음.

# 종료 시 행동
인스턴스의 설정 중 종료 시  stop 할 지, terminate 할 지 정할 수 있는데,
stop시 말 그대로 인스턴스가 멈추고, terminate 할 시 인스턴스가 삭제된다.
만약 terminate로 설정 후, terminate protection 을 사용 시, 웹에서 인스턴스를 죽이고 terminate하는 것을 방지 할 수 있지만, 터미널에서 죽일 시 terminate를 멈출 수 없다.


# troubleshooting

**instanceLimitExceeded** 
	region당 사용자에게 할당 된 vcpu를 모두 소모했다는 뜻 
	on-demand 나 spot에서만 일어나고, 다른 region에서 실행하거나 aws에 문의 하는 식으로 해결 할 수 있다.

**insufficientInstanceCapacity**
	해당 AZ에 충분한 on-Demand 수용량이 없을때. 
	 요구 인스턴스의 양을 줄이 거나 , 급하면 다른 인스턴스로 교체하는 것도 고려, 다른 AZ에서 실행하는 것도 방법중 하나. 

**ssh오류**
	해당 키의 권한이 0400으로 설정 되어 있는지 확인.
	옳은 사용자 이름, 포트가 22로 설정되어 있는지 확인

## ssh 연결 방식
	instance에 ssh에 연결하기 위한 inbound rule이 있는데, 만약 local에서 연결하려는 사용자의 ip가 rule에 있을 시 연결이 가능하지만, 그렇지 않다면 웹에서 ip주소를 등록하여 연결을 다시 시도할 수 있다.
	하지만 만약 웹(aws 사이트)에서 연결을 하려고 한다면, 자신의 Ip가 아니라 지역aws의 ip에서 연결을 시도 하기 때문에 이럴경우 aws ip pool을 확인 후 해당 ip를 등록하여 연결한다.
	