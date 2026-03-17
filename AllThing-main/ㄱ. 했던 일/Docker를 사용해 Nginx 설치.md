#### 1. Nginx 이미지 다운로드
```
docker pull nginx
```


#### 2. Nginx 서버 가동
```
docker run -it --name nginx_webserver -d -p 80:80 nginx
```
- docker image로 nginx 웹서버 실행
- -i : 사용자 입출력 가능
- -t : 가상 터미널 환경 조성
- -d : detach mode로 실행 ( = 백그라운드 실행 )
- -p : \[외부 포트]:\[내부 포트] 외부 포트로 접속 요청하면 내부 포트로 맵핑

#### 3. 정상 작동 확인
![[Pasted image 20240612115247.png]]
- 위와 같은 화면이 정상적으로 떠야 한다.