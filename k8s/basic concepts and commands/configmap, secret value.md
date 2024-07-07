

환경변수 :

```
spec:
  container:
    - name: {이름}
      image: {이미지 이름}
      env:
        - name: {환경변수 이름}
          value: {값}
```
과 같은 식으로 환경 변수를 삽입하여 파드를 실행 할 수 있다.
이렇게 지정 된 환경변수는 한번 파드가 실행되면 바뀌지 않고, 환경변수를 수정하기 위해 새로 pod를 실행 함으로써 구현 할 수 있다.


환경 변수가 위 처럼 몇개 안된다면 저런식으로 파드에 포함시킬 수 있지만 복잡해질 경우 컨피그 맵을 보통 사용한다.

컨피그맵은 클러스터에 독립적으로 저장된 설정 값의 모임 으로 볼 수있고, 여러 파드에서 참고 할 수 있다.

```
env:
  - name: {키}
    value: {벨류}
  - name: {키}
    valueFrom:
      configMapKeyRef:
        name: {컨피그맵 이름}
        key: {컨피그맵에서 가져올 항목 이름}
```

이렇게 사용될 수 있다
```
kubectl create configmap {config map 이름} --from-{파일 형식}={가져올 곳}

kubectl create configmap {config map 이름} --from-env-file={가져올 곳}
// .env 파일에서 가져온다.

kubectl create configmap {config map 이름} --from-literal={key}={value}
//설정값이 많지 않은 경우 literal로 생성해도 문제가 없음


kubectl get cm {config map 이름}

Kubectl describe {config map 이름}
```

컨피그 맵에서 모든 값들을 읽어오는 경우에는, 
spec.container.env와 같은 레이어에서 
```
env:
  {...config}
envFrom:
  - configMapRef:
    name: {configmap 이름}
```
와 같이 yaml파일을 작성한다면 해당 컨피그맵에서 모든 키-벨류를 가져올 수 있다.
환경변수의 이름이 중복되는 경우 env에서 정의 된 항목들이 envFrom 보다 우선시 되어 정의된다.

