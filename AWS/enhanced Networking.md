
## EC2 enhanced networking 
	큰 대역폭, 높은 pps, 낮은 대기속도를 자랑함.
	옵션 1. ENS(elastic network adaptor)- 100기가 까지 
	옵션 2. 인텔 뭐시기(레거시) 10기가 정도
	최신 ec2 모델들이 사용할 수 있음 

EFA(enhanced fabric adapter)
	improved ENA for HPC(high performance computer) linux에서만
	단단하게 결합되어 있어 노드들 사이 연결이 좋음 


```
modeinfo ena
```
라는 커맨드로 현재 ena의 상태를 확인 할 수 있다.
```
//구버전
ethtool -i eh0 
//2023 aws linux
ethtool -i ehX0
```
현재 사용하는 네트워크 드라이브의 특성을 알 수 있음