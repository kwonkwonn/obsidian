
docker container ls  -> 현재 실행중인 컨테이너에 대한 정보 알 수 있음
해시값의 앞 두글자만 있어도 컨테이너를 지정할 수 있다
docker container inspect (컨테이너 이름) -> 컨테이너의 정보가 json포맷으로 반환

docker container run --detach --publish 8088:80 diamol/ch02-hello-diamol-web 
간단한 html파일을 가지고 있는 아파치 서버임.

--detach ->컨테이너를 백그라운드에서 실행하며 컨테이너 id를 출력한다
--publish -> 컨테이너의 포트를 호스트 컴퓨터에 공개한다.

docker container ststs -> 실행중인 컨테이너의 상태를 알 수 있다. 

docker container rm (컨테이너 이름)-> 컨테이너를 삭제함

docker container rm --force $(docker container ls -all quiet)
-> 모든 도커 컨테이너를 삭제함 
$()안에 있는 문장들은 출력을 다른 커맨드에 전달하는 역할을 함
linux | 문법이랑 비슷한듯

docker image pull (이미지 이름) - 이미지 받아오기

도커는 컨테이너별로 환경변수를 가지고 이를 외부에서 부여할 수  있다.
docker container run --env TARGET=google.com (이미지 이름)
환경변수 지정하고 실행하기