쿠버네티스 운영환경에서 내부의 어플리케이션의 충돌로 실행이 종료될 경우, 컨테이너 내부에서 저장 되던 데이터는 모두 유실된다.
이는 컨테이너 특성상, os와 어플케이션의 상태로 저장 된 이미지 이후의 데이터는 이미지에 따로업데이트 되지 않기 때문인데, 만약 중요한 데이터를 처리하던 중 컨테이너가 종료 된다면, 새로운 컨테이너를 시작하여 컨테이너에 다시 연결 할 수 있지만, 정작 중요한 데이터는 유실 될 가능성이 크다.

이를 위해 컨테이너의 생애주기와 상관없이 이미지의 상단 계층에 따로 추가되어 관리할 수 있는 스토리지를 사용할 필요가 있다.

```
spec:
  containers:
    - name: sleep
      image: {image name}
      volumeMounts:
       - name: data
         mountPath: /data # 볼륨을 /data에 마운트
volumes:
  - name: data
    emptyDir: {} # empty Dir을 마운트 함
```
위와 같이 빈 디렉터리를 마운트 할 수 있고, 여기의 데이터는 파드가 재시작 되더라도 유지된다.
