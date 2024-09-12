

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
또한, 여기서 중요한 점은 컨테이너의 env값을 설정한다고 하더라도 이미지는 바뀌지 않고 동일한 바이너리 파일을 사용한다는 점이다. 
컨피그맵은 파드의 외부에 존재한다.


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

이를 위해선 컨피그 맵을 사전에 설정할 필요가 있는데, 아래와 같이 설정할 수 있다.

```
kubectl create configmap {config map 이름} --from-{파일 형식}={가져올 곳}

EX: 
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

### 컨테이너 속 파일로 설정값 주입
![](https://i.imgur.com/i8NO5cL.png)
아래와 같이 생각할 수 있다. 
외부에서 생성 된 컨피그맵들을 컨테이너의 파일시스템의 특정 디렉토리에 삽입할 수 있다.
```
spec:
  containers:
  - name: web
    image: {이미지}
    volumeMounts:   #컨테이너에 볼륨을 마운트 한다
    - name: config   # 마운트할 볼륨 이름
      mountPath: "app/config" # 볼륨이 마운트 될 경로
      readOnly: true
  Volums:
  - name: config # 볼륨 마운트 이름과 일치하여야 함
    configMap:
      name: {컨피그맵 이름}
```
여기서 컨피그맵은 마치 디렉토리 처럼 취급된다.
컨피그맵속 각각의 항목은 컨테이너 파일 시스템속 파일이 된다.
파일 시스템에 없는 /app/config 파일을 쿠버네티스가 직접 생성하여 그 안에 설정값을 적용시킨다.
파일들은 디렉토리와 txt파일 형태로 저장된다.
여러 단계로 구성된 
```
data:
  config.json: |
    {
	    "dir1":{
	      "dir2":{
		     "key":"value"
	      }
	    }
    }
```
와 같은 파일이 있다면, 
/app/config/dir1/dir1/key.txt란 파일에 'value'와 같이 저장되어 있을 것이다.

어플리케이션에 실행 중 컨피그 파일을 변경한다면, 그 후로는 어플리케이션의 설정에 따라 행동의 양상이 바뀐다.
설정파일의 변경을 감지하고, 행동의 변화를 주는 어플리케이션은 이는 특정상황에 대처할 수 있는 유연한 어플리케이션을 만드는것에 도움을 줄 것이다.
그러나 이러한 수정은 예기치 않은 오류를 줄 수 있으니 조심하는 것이 좋다.
특히 volumMounts와 같은 경우 이미지 바이너리에 병합되는 것이 아니라 그대로 덮어쓴다.



## secret value 

비밀값은 민감한 정보를 다루기 위해 클러스터 내부에서 별도로 처리되고, 노출을 최소화한다는 점을 제외하곤 컨피그맵과 거의 유사하다.

```
kubectl create secret generic {이름} --from-{형식}={key}={value}
#컨피그 맵과 거의 유사함

kubectl get secret sleep-secret-literal -o jsonpath='{.data.secret}'
# base64로 난독화 된 값 출력                                              kubectl get secret sleep-secret-literal -o jsonpath='{.data.secret}' | base64 -d
#값을 확인 할 수 있다
```
아래와 같이 비밀값을 설정할 수 있다
```
spec:
  - name: sleep
    image: {image}
env:
- name: {name}
  valueFrom:
    secretKeyRef:
      name: {비밀값 이름}
      key: {key}
```
이렇게 설정 된 애플리케이션은 비밀값을 평문의 형태로 받아올 수 있다.
그러나 이렇게 환경변수 형태로 전달하는 것은 여전히 위험이 있으므로 비밀값의 파일 형태로 전달하는 대안이 있을 수 있다. 

```
apiVersion: v1
kind: Secret
metadata:
  name: {name}
type: opaque
stringData: 
  {Key}
```
이러한 데이터는 어플리케이션에서
환경변수로 등록사여 사용하게나 애플리케이션 manifest에서 애초에 value값을 값의 경로로 설정하는 법으로 접근할 수 있다.
