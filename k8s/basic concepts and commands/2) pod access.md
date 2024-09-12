
```
kubectl get pod {pod 이름} -o custom-columns=NAME:metadata.name,POD_IP:status.podIP
// 특정 파드의 ip받아오기

kubectl execd -it hello-kiamol -- sh
//파드 내부와 연결할 대화형 셸 실행

kubectl logs --tail={n} {pod 이름}
//로그 확인

kubectl exec -it  deploy/{디플로이 이름} -- sh
//디플로이로 배포한 파드도 이런식으로 열람 가능

Kubectl logs --tail={n} -l (라벨로 확인 한다는 뜻) app={디플로이 이름}
//라벨을 참고해 배포 된 파드를 가져와서 로그출력
```

파드 내부의 파일을 접근하는 방법은 아래와 같다

```
mkdir -p /tmp/{디랙토리}
//그냥 임시 파일을 만드는 과정

kubectl cp hello-kiamol:{파드 내 file path} {로컬 file path}
//파드 내부의 파일을 로컬로 복사하는 과정, docker 커맨드와 비슷하다 

```

파드 삭제 

```
kubectl delete pods -all 
//모든 파드를 삭제 ##deployment로 배포 된 파드는 삭제가 되어도 다시 생성 된다

kubectl get deploy
//디플로이 조회

kubectl delete deploy --all
//모든 디플로이 삭제, deploy가 관리하던 파드도 모두 삭제된다

kubectl delete -f {dir Name}/
# 해당 디렉토리 내부의 모든 yaml로 구성 된 리소스를 제거한다.

```
![](https://i.imgur.com/0cINVz8.png)
