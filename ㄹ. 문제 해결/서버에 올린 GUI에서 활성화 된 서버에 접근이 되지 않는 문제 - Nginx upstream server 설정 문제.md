#### 문제
- nginx 를 통한 웹 서비스를 올린 후 server 연결 후 실제 존재하는 페이지가 있음에도 404 error, api 요청 시도도 하지 않음 문제 발생


#### 원인
- nginx upstream 의 server 설정에 도메인을 사용해 server 를 설정해놨었는데,
  이렇게 하게 되면 도메인은 nginx 를 통해 80번 포트로 이미 포트 포워딩을 하고 있기 때문에 해당 서버의 80 포트로 연결이 된다.
- 이렇게 server target 을 잡게 되면, nginx에서 server의 어떤 포트로 접근 할지 알 수 없게 된다.

#### 해결
- nginx upstream server 설정을 서버 ip 주소와 포트를 명시해 정확한 요청이 갈 수 있도록 수정한다.


![[Pasted image 20250313171814.png]]