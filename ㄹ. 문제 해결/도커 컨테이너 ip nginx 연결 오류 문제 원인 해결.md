docker logs -f {container id}

docker network inspect {network name}


** nginx에서 network 요청을 보냈는데,

2024/01/04 01:08:32 [error] 29#29: *1191 connect() failed (111: Connection refused) while connecting to upstream, client: 200.100.1.135, server: , 
	request: "GET /v1/auth/session-check HTTP/1.1", upstrea  m: "http://172.19.0.3:8000/v1/auth/session-check", host: "200.100.1.139:3000", referrer:
	 "http://200.100.1.139:3000/"
와 같이 서버에서 연결이 거절되는 문제 발생,

원인 : 서버에서 이미지를 내렸다가 올리는 과정에서 기존에 nginx가 바라보던 ip 주소와는 다른 ip 주소로 변경이 되면서 nginx가 바라보는 ip를 잃게 된다.

해결 : docker-compose 옵션에서 해당 네트워크 이미지의 ip 주소를 static하게 고정시킬 수 있다.

** 서버 리부트 시, 네트워크 이미지의 ip주소가  변경될 수 있다.
** 가끔 docker 이미지를 내리고, 올리는 과정에서 문제가 생겨 down 했고, 로그도 down으로 생성이 됐지만, os 상에서는 down이 되지 않아 
	이미지는 그대로 남아있고 또 다시 동일한 이미지를 up 하면 기존의 down 되지 않은 이미지가 존재하기 때문에 다른 ip로 이미지가 생성이 되어 ip가 변하는 경우가 있다.
	이럴 때, docker-compose의 always 옵션을 사용해 비정상적인 up/down이 발생했을 때 정상적인 up/down이 될 수 있도록 
	계속 실행해주는 옵션을 사용할 수 있다.
	