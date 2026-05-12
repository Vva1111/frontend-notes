[www.naver.com](http://www.naver.com) 을 입력하면

1. [naver.com](http://naver.com) 도메인에 속해있는, 이름이 www인 컴퓨터와 인터넷 통신을 하기 위해서는 해당 컴퓨터의 IP주소를 파악해야 한다

1. 목적으로 하는 곳의 IP주소를 얻기 위해서 DNS에 질의( 공유기 혹은 그 외의 방법 )
    
    - 정보를 요청하려고 하는 IP 주소는 달라질 수 있다
    

1. 요청하는 pc에서 얻은 IP에 TCP 연결
    
    1. HTTP 연결은 TCP를 기반으로 하기 때문에
    

1. TCP연결이 된다면, 그 이후에 HTTP requset

1. request에 대한 응답(response)을 받는다

  

- GSLB
    
    → DNS
    
    → System(DNS)
    
    ⇒ Health Check : 여러 개의 서버 중에서 하나가 문제가 났을 경우, 우회 서버를 찾고,
    
    우회하기 위해서
    
    ( 부하/분산, 장애 대응, fail over(?), 재해 대응 )
    

- CDN ( Contents Delivery Network Service )
    
    → 접속자의 IP를 가지고 DB에 저장 된 주소를 파악한다
    
    → CDN이 해당 위치의 접속자가 가장 빠르게 사용할 수 있는(반응성이 좋은) 웹 서버의 IP를 제공한다
    
    ( 예를 들어 naver라면 naver가 가진 웹 서버는 한 개가 아닌 여러 개를 가지고 있다가 접속자의 위치에 따른 최적화 된 웹 서버의 IP 주소를 내어준다 )
    
    → CDN을 사용하는 이유