![](https://i.imgur.com/AP4oSyH.png)
위와 같은 yaml파일 을 생각해보자.

api 버전과 정의하려는 리소스의 유형을 나타내고 
metadata 에서 파드의 이름(필수) 또 라벨(비필수)를 정의 할 수 있따.

스펙에서 리소스에서 사용할 이미지와 이름을 정의할 수 있따.
이러한 yaml파일은 선언 적으로 실행되고,
```
kubectl apply -f pod.yaml이라는 커맨드로 실행 할 수 있다.
```

![](https://i.imgur.com/NLNLnLh.png)
이번엔 디플로이먼트를 선언해보았다.
spec.selector를 통해서 자신이 관리해야할 라벨을 선언할 수 있고,
템플릿을 통해서 생성할 컨테이너에 대한 정보를 쓸 수 있는데, 디플로이먼트로 선언할 경우 컨테이너의 이름을 선언하기 보단, label의 정보를 matchLables와 정확히 일치하게 선언하여야 한다.

