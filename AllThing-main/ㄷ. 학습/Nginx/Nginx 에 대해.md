- 비동기 처리 방식의 웹 서버 프로그램
- Apache 보다 동작이 단순하며, 전달자 역할만 하기 때문에 동시 접속 처리에 특화되어 있다.
- 스레드/프로세스 기반 구조를 가지는 Apache는 클라이언트 하나당 스레드 하나를 사용하기 때문에 클라이언트가 많아질수록 계속해서 스레드가 생성된다. 즉, 서버에 많은 부하가 가게 된다는 뜻이다.
	- 그에 반해 Nginx는 비동기식 구조이기 때문에 모든 클라이언트 요청을 쓰레드가 아닌 이벤트 핸들러가 처리한다. 따라서 스레드의 사용이 적고, 서버 자원 활용 능력이 더 뛰어나다.
	- ![[Pasted image 20240304112642.png]]**Nginx 구조**
	- ![[Pasted image 20240304112718.png]]**Apache 구조**

#### HTTP 서버로서의 역할
- HTML, CSS, JavaScript, 이미지와 같은 정보를 웹 브라우저에 전송하는 웹 서버 역할을 한다.
- 이미지, css 등 정적 리소스에 대한 요청을 Nginx가 처리하도록 하고, 동적으로 계산하거나 전달해야 하는 것들은 WAS에 맡기고, DB에 대한 요청은 DB 서버가 처리하도록 함으로써 서비스를 담당하는 서버를 분산해 효율적인 서버 관리를 할 수 있다.

#### 리버스 프록시 역할
- 리버스 프록시는 인터넷 내부에서 서버의 앞쪽에 위치한다. 즉, 클라이언트와 서버의 사이에 존재한다.
	- 리버스 프록시는 어떤 서버에 요청(request)을 보낼지 결정하고 효율적이고 안전하게 요청/응답을 관리하기 위한 목적을 가진다.
- 클라이언트가 가짜 서버에 요청(request)을 하면, 프록시 서버인 Nginx가 배후 서버(reverse server)인 응용 프로그램 서버로부터 데이터를 가져오는 역할을 한다.
- 웹 응용 프로그램 서버에 리버스 프록시(Nginx)를 두는 이유는 요청(request)에 대한 버퍼링이 있기 때문이다.
- 클라이언트가 직접 App 서버에 요청하는 경우, 프로세스 1개가 응답 대기 상태가 되어야 하지만, 프록시 서버를 두면 요청을 배분할 수 있다.
- 


###### * 포워드 프록시
- 서버가 존재하는 인터넷에 접속하기 전에 사용자의 요청(request)을 받아 서버로 전달하고, 요청에 대한 서버의 응답을 다시 사용자에게 전달하는 중개인 역할
- 한 예로, 특정 기관에서 방화벽을 세워 인터넷 접속에 제한을 두어 일부 인터넷만 사용하는 것이 있다. 즉, 사용자들이 인터넷에 접속하기 전 하나의 중개



#### 80(http), 443(https) Port
- 인터넷 제공자에 의해 닫혀있는 포트가 있을 수 있기 때문에 아무 포트나 사용해 Nginx를 배포해서는 안된다.
- 일반적으로 http가 80, https가 443이고, 표준 규약이지만, 실제로는 어떤 포트가 열려 있고, 그 포트를 적절히 처리할 수 있는 서버 설정이 있다면 https에도 80번 포트를 사용할 순 있긴 하다.
	- 하지만, 서로 다른 포트를 사용할 경우, 몇 가지 문제가 발생할 수 있는데,
	1. 대부분의 웹 브라우저와 시스템은 80번 포트를 통해 http 트래픽을 기대하고 있기 때문에 https 트래픽이 80번 포트로 들어오게 되면 예상치 못한 결과나 혼란을 초래할 수 있다.
	2. 80번 포트는 일반적으로 암호화 되지 않은 http 트래픽을 위해 사용하기 때문에 이 포트를 통해 https 트래픽을 보내려고 하면, 중간자 공격 등의 보안 위험에 노출될 수 있다.
	3. 일부 기기나 소프트웨어는 https 트래픽을 443번 포트로만 받아들일 수 있다. 이 경우, 80번 포트를 사용하면 호환성 문제가 발생할 수 있다.


#### SSL/TLS 지원
- 사용자의 정보를 보호하기 위해 데이터에 대한 암호화 작업이 필요한데, 주로 SSL 혹은 TLS의 보안 프로토콜을 적용한다. 
- Nginx는 http에 SSL, https에 TLS 프로토콜 기반 인증서를 적용해 https 프로토콜을 지원한다.


#### Nginx default.conf 설정 구성
```
user  nginx;
worker_processes  1;
error_log  /var/log/nginx/error.log warn;
pid        /var/run/nginx.pid;
events {
    worker_connections  1024;
}
  
http {
    include       /etc/nginx/mime.types;
    default_type  application/octet-stream;
    upstream docker-server {
        server server:8080;
    }
    server {
        listen 80;
        server_name localhost;
  
        location / {
            root /usr/share/nginx/html;
            index index.html index.htm;
            try_files $uri $uri/ /index.html =404;
        }
        location /api {
            proxy_pass         http://docker-server;
            proxy_redirect     off;
            proxy_set_header   Host $host;
            proxy_set_header   X-Real-IP $remote_addr;
            proxy_set_header   X-Forwarded-For $proxy_add_x_forwarded_for;
        }
        location /socket {
            proxy_http_version 1.1;
            proxy_set_header Upgrade $http_upgrade;
            proxy_set_header Connection "upgrade";
            proxy_set_header Host $host;
            proxy_pass http://docker-server;
        }
    }
    log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
                      '$status $body_bytes_sent "$http_referer" '
                      '"$http_user_agent" "$http_x_forwarded_for"';
    access_log  /var/log/nginx/access.log  main;
  
    sendfile        on;
server_tokens     off;
    keepalive_timeout  65;
    include /etc/nginx/conf.d/*.conf;
}
```
- **user** : 프로세스 실행 권한, 보안상 root는 권장되지 않는다.
-  **worker_process** : 몇 개의 워커 프로세스를 생성할 것인지 지정하는 지시어. 1이면 모든 요청을 하나의 프로세스로 실행하겠다는 뜻으로, CPU 멀티코어 시스템에서 1이면 하나의 코어만으로 요청을 처리하는 것이 된다. 보통은 auto로 설정하는 경우가 많다.
-  **error_log** : 로그 레벨을 설정하는 지시어. debug/info/notice/warn/error/crit 등의 종류가 있다.
-  **pid** : nginx의 마스터 프로세스 id 정보가 저장된다.

- ##### events block
	- **worker_connections** : 하나의 프로세스가 처리할 수 있는 커넥션의 숫자

- ##### http block
	-  **include** : 옵션 항목을 설정해둔 파일의 경로를 지정하는데 보통 파일 확장명과 MIME 타입 목록을 지정한다.
	-  **default_type** : application/octet-stream; 을 사용했을 경우, 옥텟 스트림 기반의 http를 사용한다는 지시어
		- octet-stream : 해당 컨텐츠 타입(Content-type)의 MIME 형식이다.
			 서버에서 다루는 확장자 명이 어떤 형식의 자료인지 알려주는 것으로,
			 octet-stream은 8비트로 된 데이터라는 뜻을 가진다.
		- MIME 형식 : 미디어 타입(Multipurpose Internet Mail Extensions), 문서, 파일 또는 바이트 집합의 성격과 형식을 나타낸다.
			 브라우저는 '파일 확장자' 가 아닌 MIME 타입을 사용해 URL 처리 방법을 결정한다. 따라서, 웹 서버가 응답의 Content-type 헤더에 올바른 MIME 타입을 보내는 것이 중요하다. 그렇지 않을 경우, 브라우저가 파일 내용을 잘못 해석하거나, 사이트가 제대로 작동하지 않고 다운 받은 파일이 잘못 처리될 수 있다.

- ##### upstream block
	- origin 서버 라고도 하는데, 여기서는 WAS(Web Application Server)를 의미하며, nginx는 **downstream**에 해당한다고 할 수 있다. 
		 nginx와 연결한 웹 어플리케이션 서버를 지정하는데 사용된다.
		 하위에 있는 server 지시어는 연결할 WAS의 host:port를 지정한다.

- ##### server block
	- 하나의 웹 사이트를 선언하는데 사용한다. server block이 여러 개면 한 개의 머신(호스트)에 여러 웹사이트를 서빙할 수 있다. 이러한 개념을 가상 호스트 ( 실제 호스트는 1개이지만 여러 개 인것처럼 보이게 함 ) 라고 한다.
	- **listen** : 이 웹사이트가 바라보는 port를 설정
	- **server_name** : 클라이언트가 접속하는 서버(주로 도메인 네임). 이것과 실제로 들어온 요청(request)의 header에 명시된 값이 일치하는지 확인해 server를 분기 해준다.
	- **location block** : server block 내부에서 특정 웹 사이트의 url을 처리하는데 사용한다.
		 같은 도메인 내부에서 특정 경로에 따른 접근 요청을 다르게 처리하고 싶을 때 사용한다. 내부의 **root**는 웹 사이트가 바라보는 root 폴더의 경로를 의미한다.
 
- **sendfile** : 파일을 효율적으로 전송하기 위한 커널의 시스템 호출을 이용하는 옵션이다. 활성화 될 경우, 웹 서버는 커널이 파일의 내용을 직접 네트워크 스택으로 보내도록 지시한다. 이는 파일 데이터를 사용자 모드와 커널 모드 사이에서 복사하는 작업을 줄여준다. 즉, sendfile 옵션을 on으로 설정하면 파일 전송 성능이 향상될 수 있다. 그러나 이 옵션은 정적 파일을 제공하는 경우에만 효과
- **server_token** : 헤더에 nginx 버전을 숨기는 기능을 하며, off로 숨겨 보안 상 숨기는 것을 기본으로 한다.
- **keepalive_timeout** : 접속 시 커넥션 유지 시간을 지정한다.