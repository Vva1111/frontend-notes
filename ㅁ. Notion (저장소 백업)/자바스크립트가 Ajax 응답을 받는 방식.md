> [!info] 코딩교육 티씨피스쿨  
> 4차산업혁명, 코딩교육, 소프트웨어교육, 코딩기초, SW코딩, 기초코딩부터 자바 파이썬 등  
> [http://tcpschool.com/ajax/ajax_intro_works](http://tcpschool.com/ajax/ajax_intro_works)  

> [!info] 🌐 Fetch API 으로 AJAX 요청하기  
> 자바스크립트 AJAX 요청 방식 정통적으로 XMLHttpRequest() 객체를 생성하여 요청하는 방법이 있지만 문법이 난해하고 가독성도 좋지 않다.  
> [https://inpa.tistory.com/entry/JS-📚-AJAX-서버-요청-및-응답-fetch-api-방식#xml_http_request_방식](https://inpa.tistory.com/entry/JS-📚-AJAX-서버-요청-및-응답-fetch-api-방식#xml_http_request_방식)  

![[Untitled 349.png]]

- Ajax를 사용한 데이터 전송 순서를 살펴보자면,
    
    1. 사용자에 의한 요청 이벤트 발생
    
    1. 요청 이벤트가 발생하면 이벤트 핸들러에 의해 자바스크립트가 호출
    
    1. 자바스크립트는 XMLHttpRequest 객체를 사용해 서버로 요청을 보냄
    
    1. 서버는 전달 받은 XMLHtpRequset 객체를 가지고 요청을 처리
    
    1. 서버는 처리한 결과를 HTML, XML 또는 JSON 형태의 응답 데이터를 생성, 웹 브라우저에 전달
    
    1. 이때 전달되는 응답은 새로운 페이지를 전부 보내는 것이 아니라 필요한 데이터만을 전달
    
    1. 서버로부터 전달 받은 데이터를 가지고 웹 페이지의 일부분만을 갱신하는 자바스크립트를 호출한다.
    
    1. 결과적으로 웹 페이지의 일부분만이 다시 로딩 되어 표시된다.
    
      
    

## XMLHttpRequest 객체

- 웹 브라우저가 서버와 데이터를 교환할 때 사용하는 객체

  

### readyState

- XMLHttpRequest 객체 내부의 readyState 프로퍼티는 Ajax에서 서버로부터의 응답을 확인하기 위해 사용하는 프로퍼티이다.
    
    - 이 프로퍼티는 XMLHttpRequest 객체의 현재 상태를 나타내며, 객체의 현재 상태에 따라 아래와 같은 주기로 순서대로 변화한다.
        
        1. UNSENT ( 0 ) : XMLHttpRequest 객체가 생성됨
        
        1. OPENED ( 1 ) : open() 메소드가 성공적으로 실행됨
        
        1. HEADERS_RECEIVED ( 2 ) : 모든 요청에 대한 응답이 도착함
        
        1. LOADING ( 3 ) : 요청한 데이터를 처리 중임
        
        1. DONE ( 4 ) : 요청한 데이터의 처리가 완료되어 응답할 준비가 완료됨
        
    

  

### status

- status 프로퍼티는 서버의 문서 상태를 나타낸다.

  

### 응답 데이터 프로퍼티

- 서버로부터 응답 반은 데이터는 아래 프로퍼티에 들어 있다. 클라이언트는 이를 이용해 받은 데이터를 적절히 처리할 수 있다.
    
    1. responseText : 서버에 요청해 응답으로 받은 데이터를 문자열로 반환
    
    1. responseXML : 서버에 요청해 응답으로 받은 데이터를 XML DOM 객체로 반환
    
      
    

> [!important] Fetch 방식
> 
> - Fetch를 통해 데이터 요청을 하고, 서버로부터 응답 값을 받으면 .then을 통해 함수의 인자에 값이 담기게 되는데, 이 값은 Response 객체로서 여러 정보를 담고 있다.
>     
>     ![[Untitled 350.png]]
>     
>     - reponse.status : HTTP 상태 코드
>     
>     - response.ok : HTTP 상태 코드가 200과 299 사이일 경우 true 값 반환
>     
>     - response.body : 응답 값의 내용
>     
>     - response.? :
>         
>         응답을 읽고 ? 부분에 해당하는 값으로 반환한다.
>         
>         ?에 들어갈 수 있는 값은
>         
>         text(), json(), fromData(), blob(), arrayBuffer() 가 있다.
>         
>     
>     - 위 응답 자료 형태 반환 메서드는 한 번만 사용할 수 있다. 예를 들어, response.text()를 사용해 응답을 얻었다면 본문의 콘텐츠는 모두 처리 된 상태이기 때문에 같은 요청 내에서 다른 반환 메서드는 사용할 수 없다.
>