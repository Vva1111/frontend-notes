- Document Object Model

- HTML 문서의 계층적 구조와 정보를 표현하며 이를 제어할 수 있는 API, 즉 프로퍼티와 메서드를 제공하는 트리 자료

## node

### 1. HTML 요소와 노드 객체

- HTML 요소는 렌더링 엔진에 의해 파싱되어 DOM을 구성하는 요소 노드 객체로 변환된다.

- 이 때, HTML 요소의 어트리뷰트는 어트리뷰트 노드로, HTML 요소의 텍스트 콘텐츠는 텍스트 노드로 변환된다.

- HTML 요소는 중첩관계를 가진다.

- 트리 자료구조
    
    - 노드들의 계층 구조로 이뤄진다.
    
    - 노드 객체들로 구성된 트리 자료구조를 DOM이라고 한다.
    
    ```jsx
    <!DOCTYPE html>
    <html>
      <head>
        <meta charset="UTF-8">
        <link rel="stylesheet" href="style.css">
      </head>
      <body>
        <ul>
          <li id="apple">Apple</li>
          <li id="banana">Banana</li>
          <li id="orange">Orange</li>
        </ul>
        <script src="app.js"></script>
      </body>
    </html>
    ```
    
    [![](https://velog.velcdn.com/images/hustlekang/post/8fa6a095-9316-4dc7-9a6a-4fb448771b63/image.png)](https://velog.velcdn.com/images/hustlekang/post/8fa6a095-9316-4dc7-9a6a-4fb448771b63/image.png)
    
      
    

- 노드의 종류
    
    - 문서 노드 : DOM트리 최상위에 존재하는 루트 노드, documnet 객체를 가리킨다. document 객체는 브라우저가 렌더링한 HTML문서 전체를 가리키는 객체로서 전역 객체 window의 document 프로퍼티에 바인딩되어있다. 따라서 문서 노드는 window.document 또는 document로 참조할 수 있다.
        
        - 브라우저 환경의 모든 자바스크립트 코드는 script 태그로 분리되어 있어도 하나의 전역객체 window를 공유한다. 따라서 모든 자바스크립트 코드는 전역 객체 window의 document 프로퍼티에 바인딩 되어 있는 하나의 document 객체를 바라본다. 즉 HTML 문서 하나 당 document객체는 유일하다.
        
        - document객체는 DOM트리의 루트 노드이므로 DOM 트리의 노드들에 접근하기 위한 진입점 역할을 한다. 즉, 요소, 어트리뷰트, 텍스트 노드에 접근하려면 문서 노드를 통해야 한다.
        
    
    - 요소 노드 : HTML 엘리먼트를 가리키는 객체이다. 요소 노드는 HTML 요소 사이 중첩에 의한 부자관계를 가지며, 이 부자 관계를 통해 정보를 구조화한다. 요소 노드는 문서의 구조를 표현한다고 할 수 있다.
    
    - 어트리뷰트 노드 : HTML 요소의 어트리뷰트를 가리키는 객체.
        
        - 어트리뷰트가 지정된 HTML 요소의 요소 노드와 연결되어 있다. 단, 요소 노드는 부모 노드와 연결되어 있지만 어트리뷰트 노드는 부모 노드와 연결되어 있지 않고 요소 노드에만 연결되어 있다. 즉, 어트리뷰트 노드는 부모 노드가 없으므로 요소 노드의 형제 노드는 아니다. 따라서 어트리뷰트 노드에 접근해 어트리뷰트를 참고하거나 변경하려면 먼저 요소 노드에 접근해야 한다.
        
    
    - 텍스트 노드 : HTML 요소의 텍스트를 가리키는 객체. 요소 노드가 문서의 구조를 표현한다면, 텍스트 노드는 문서의 정보를 표현한다고 할 수 있다.
        
        - 요소 노드의 자식노드이며, 자식 노드를 가질 수 없는 leaf node이다. 즉, 텍스트 노드는 DOM 트리의 최종단이다. 따라서 텍스트 노드에 접근하기 위해서는 먼저 부모 노드인 요소 노드에 접근해야 한다.
        
    
    - 위의 노드 외에도 comment(주석), DOCTYPE(document type), DocumentFragment 노드 등 총 12개의 노드 타입이 있다.
    

- 노드 객체의 상속 구조
    
    - DOM을 구성하는 노드 객체는 자신의 구조와 정보를 제어할 수 있는 DOM API를 사용할 수 있다. 이를 통해 토드 객체는 자신의 부모, 형제, 자식을 탐색할 수 있으며, 자신의 어트리뷰트와 텍스트를 조작할 수도 있다.
    
    - DOM을 구성하는 노드 객체는 ECMAScript 사양에 정의된 표준 빌트인 객체가 아니라 브라우저 환경에서 추가적으로 제공하는 호스트 객체이다. 하지만 노드 객체도 자바스크립트 객체이므로, 프로토타입에 의한 상속 구조를 가진다.
    
    [![](https://velog.velcdn.com/images/hustlekang/post/35b9ff6c-3843-4343-aa93-ad1504319af8/image.png)](https://velog.velcdn.com/images/hustlekang/post/35b9ff6c-3843-4343-aa93-ad1504319af8/image.png)
    
    - 위와 같이, 모든 노드 객체는 Object, EventTarget, Node 인터페이스를 상속받는다.
    
    - 추가적으로 문서 노드는 Document, HTMLDocument 인터페이스를 상속받고, 어트리뷰트 노드는 Attr, 텍스트 노드는 CharacterData 인터페이스를 상속받는다.
    
    - 요소 노드는 Element 인터페이스를 상속받는다. 또한 요소 노드는 추가적으로 HTMLElement와 태그의 종류별로 세분화된 HTMLhtmlElement, HTMLElement, Node, EventTarget 등의 인터페이스를 상속받는다.
    
    - 프로토타입 체인 관점에서 보자면, input 요소가 생성되었을 때, input 요소 노드 객체는 프로토타입 체인에 있는 모든 프로토타입의 프로퍼티나 메서드를 상속받아 사용할 수 있다.
    
    |input 요소 노드 객체의 특성|프로토타입을 제공하는 객체|
    |---|---|
    |객체|Object|
    |이벤트를 발생시키는 객체|EventTarget|
    |트리 자료구조의 노드 객체|Node|
    |브라우저가 렌더링할 수 있는 웹 문서의 요소(HTML, XML, SVG)를 표현하는 객체|Element|
    |웹 문서의 요소 중에서 HTML 요소를 표현하는 객체|HTMLElement|
    |HTML 요소 중에서 input 요소를 표현하는 객체|HTMLInputElement|
    
    - 모든 노드 객체는 공통적으로 이벤트를 발생시킬 수 있다.
    
    - 이벤트에 관련된 기능(EventTarget.addEventListner 등)은 EventTarget 인터페이스가 제공한다.
    
    - 또한 모든 노드 객체는 트리 자료구조의 노드로서 공통적으로 트리 탐색 기능(Node.parentNode, Node.childNodes 등)이나 노드 정보 기능(Node.nodeType, Node.nodeName 등)이 필요하다. 이 같은 노드 관련 기능은 Node 인터페이스가 제공한다.
    
    - 노드 객체는 공통된 기능일수록 프로토타입 체인 상위에, 개별적인 고유 기능일수록 프로토타입 체인의 하위에 프로토타입 체인을 구축해 노드 객체에 필요한 기능, 즉 프로퍼티와 메서드를 제공하는 상속 구조를 가진다.
    
    - DOM은 HTML 문서의 계층적 구조와 정보를 표현하는 것은 물론 노드 객체의 종류, 즉 노드 타입에 따라 필요한 기능을 프로퍼티와 메서드의 집합인 DOM API로 제공한다. DOM API를 통해 HTML의 구조나 내용 또는 스타일 등을 동적으로 조작할 수 있다.
    
      
    
    ## 요소 노드 취득
    
    - getElementById 메서드
        
        - Document.prototype의 프로퍼티이다. 따라서 반드시 문서 노드인 document를 통해 호출해야 한다.
        
        - HTML 문서 내에 중복된 id 값을 가지는 HTML 요소가 여러 개 존재하더라도 에러는 발생하지 않으며, 이러한 경우 getElementById 메서드는 인수로 전달된 id 값을 가지는 첫 번째 요소 노드만 반환한다. getElementById 메서드는 언제나 단 하나의 요소 노드를 반환한다.
        
        - HTML 요소에 id 어트리뷰트를 부여하면 id 값과 동일한 이름의 전역변수가 암묵적으로 선언되고 해당 노드 객체가 할당되는 부수 효과가 있다.
        
    
    - getElementsByTagName 메서드
        
        - 인수로 전달한 태그 이름을 가지는 모든 요소 노드들을 탐색해 반환한다.
        
        - 함수는 하나의 값만 반환할 수 있으므로 여러 개의 값을 반환하려면 배열이나 객체와 같은 자료구조에 담아 반환해야 한다.
        
        - getElementsByTagName 메서드가 반환하는 DOM 컬렉션 객체인 HTMLCollection 객체는 유사 배열 객체이며 이터러블이다.
        
        - HTML 문서의 모든 요소 노드를 취득하려면 메서드의 인수로 ‘*’를 전달한다.
        
        - Document.prototype.getElementsByTagName 메서드는 DOM의 루트 노드인 문서 노드, 즉 document를 통해 호출하며 DOM 전체에서 요소 노드를 탐색해 반환한다.
        
        - Element.prototype.getElementsByTagName 메서드는 특정 요소 노드를 통해 호출하며, 특정 요소 노드의 자손 노드 중에서 요소 노드를 탐색해 반환한다.
        
    
    - getElementsByClassName 메서드
        
        - 인수로 전달한 class 어트리뷰트 값을 갖는 모든 요소 노드들을 탐색해 반환
        
        - Document.prototype에 정의된 메서드와 Element.prototype에 정의된 메서드가 있다.
        
        - Document.prototype.getElementsByClassName 메서드는 DOM의 루트 노드인 문서 노드 즉, document를 통해 호출하며 DOM 전체에서 요소 노드를 탐색해 반환
        
        - Element.prototype.getElementsByClassName 메서드는 특정 요소노드를 통해 호출, 특정 요소 노드의 자손 노드 중에서 요소 노드를 탐색해 반환한다.
        
        ```jsx
        ... <HTML> .... 
        
        // DOM 전체에서 class 값이 'banana'인 요소 노드를 모두 탐색해 반환한다.
        const $bananaFromDocument = document.getElementsByClassName('banana');
        console.log($bananaFromDocument)
        // 결과값 : HTMLCollention(2) [li.banana, div.banana]
        
        // \#fruit 요소의 자손 노드 중에서 class 값이 'banana' 인 요소 노드를 모두 탐색, 반환
        const $fruits = document.getElementById('fruits');
        const $bananaFromFruits = $fruits.getElementsByClassName('banana');
        
        console.log($bananasFromFruits);
        // 결과값 : HTMLCollection [li.banana]
        ```
        
    
      
    
      
    
    - CSS Selector를 통한 요소 획득
        
        - Document.prototype./Element.prototype.querySelector 메서드는 인수로 전달할 CSS 선택자를 만족시키는 하나의 요소 노드를 탐색, 반환한다.
        
        - *, p { … }, \#foo { … }, div p { … }, p::before { … }, a:hover { … } 등 관련 내용은 검색
        
        - HTML 문서의 모든 요소 노드를 취득하려면 querySelectorAll 메서드의 인수로 전체 선택자 ‘*’ 를 전달한다.
        
        - ‘ul > li’ 를 인자로 전달 할 경우, ul 요소의 자식 요소인 li 요소를 모두 탐색, 반환한다.
        
        - 취득한 모든 요소 노드들은 NodeList 객체에 담겨 반환되며, 이 NodeList 객체는 유사 배열이며, 이터러블하다
        
        - 이 메서드 또한 Document, Element에 정의된 메서드가 있다.
        
    
    > [!important] * 요소노드객체.matches(’\#fruits > li.apple); 와 같이 matches 메서드를 사용하면 해당하는 CSS 선택자를 통해 특정 요소 노드를 취득할 수 있는지 확인할 수 있다.
    
      
    
    - HTMLCollection과 NodeList
        
        - DOM API가 여러 개의 결과값을 반환하기 위한 DOM 컬렉션 객체이다.
        
        - 위 두 객체는 모두 유사 배열이며, 이터러블 하다.
        
        - 위 두 객체의 중요한 특징은 노드 객체의 상태 변화를 실시간으로 반영하는 ‘살아 있는 객체’라는 것이다.
        
        - HTMLCollection은 언제나 live 상태로 동작하지만, NodeList는 대부분의 경우 노드 객체의 상태 변화를 실시간으로 반영하지 않고 과거의 정적 상태를 유지하는 non-live 객체로 동작하지만 경우에 따라 live 객체로도 동작할 때가 있다.
        
        - HTMLCollection
            
            - HTMLCollection 객체는 실시간으로 노드 객체의 상태 변경을 반영해 요소를 제거할 수 있기 때문에 HTMLCollection 객체를 for문으로 순회하면서 노드 객체의 상태를 변경하는 경우, 가장 먼저 변경된 요소 객체는 실시간으로 상위 노드에서 제거되기 때문에 원하는 결과를 얻을 수 없게 된다.(해결을 위해서는 고차함수-filter, map forEach 등등 을 사용할 수 있다.)
            
        
        - NodeList
            
            - 위 HTMLCollection 객체의 부작용을 해결하기 위해서 getElements~ 메서드 대신 querySelectorAll 메서드를 사용할 수 있는데, querySelectorAll 메서드는 DOM 컬렉션 객체인 NodeList 객체를 반환한다. NodeList 객체는 실시간으로 노드 객체의 상태 변경을 반영하지 않는 non-live 객체이다.
            
            - querySelectorAll이 반환하는 NodeList 객체는 NodeList.prototype.forEach 메서드를 상속받아 사용할 수 있다.
            
            - NodeList.prototype은 forEach 외에도 item, entries, keys, values 메서드를 제공한다.
            
            - NodeList 객체는 기본적으로 객체의 변경을 실시간으로 반영하지 않는 non-live 객체로 동작하지만, childsNodes 프로퍼티가 반환하는 NodeList 객체는 HTMLCollection 객체와 마찬가지로 live 객체로 동작하므로 주의가 필요하다.
            
        
        > [!important] * HTMLCollection, NodeList 객체는예상과 다르게 동작하는 경우가 많아 안전하게 DOM 컬렉션을 사용하려면 HTMLCollection 혹은 NodeList 객체를 배열로 변환해 사용하는 것이 권장된다.
        > 
        >   
        > * 위 두 객체의 경우, 메서드가 제공되기는 하지만 배열의 고차 함수만큼 다양한 기능을 제공하지는 않는다.
        
          
        
    
    - 공백 텍스트 노드
        
        - space, tab, 줄 바꿈 등의 공백 문자는 텍스트 노드를 생성한다.
        
    
    - 자식 노드 탐색
        
        - 자식 노드를 탐색하기 위해서는 노드 탐색 프로퍼티를 사용할 수 있다(Node.prototype.childNodes 등)
        
        - 자식 노드 존재 확인을 위해서는 Node.prototype.hasChildNodes 메서드를 사용한다.
        
    

## 노드 정보 취득

- 노드 객체에 대한 정보를 취득하려면 다음과 같은 노드 정보 프로퍼티를 사용한다.
    
    - Node.prototype.nodeType / .nodeName
    

## 요소 노드의 텍스트 조작

- Node.prototype.nodeValue 프로퍼티
    
    - getter / setter 모두 존재하는 접근자 프로퍼티
    
    - 참조와 할당 모두 가능하다.
    
    - 노드 객체의 nodeValue 프로퍼티를 참조하면 노드 객체의 값을 반환한다. 노드 객체의 값이란 텍스트 노드의 텍스트이다.
    
    - 텍스트 노드의 nodeValue 프로퍼티에 값을 할당하면 텍스트 노드의 값, 즉 텍스트를 변경할 수 있다.
    

- Node.prototype.textContent 프로퍼티
    
    - setter와 getter 모두 존재하는 접근자 프로퍼티이다.
    
    - 요소 노드의 텍스트와 모든 자손 노드의 텍스트를 모두 취득하거나 변경한다.
    
    - 요소 노드의 textContent 프로퍼티를 참조하면 요소 노드의 콘텐츠 영역(시작 태그와 종료 태그 사이) 내의 텍스트를 모두 반환한다.
    
    - 이 때, HTML 마크업은 무시된다.
    
    - 요소 노드의 textContent 프로퍼티에 문자열을 할당하면 문자 노드의 모든 자식 노드가 제거되고 할당한 문자열이 텍스트로 추가된다. 이 때, 할당한 문자열에 HTML 마크업이 포함되어 있더라도 문자열 그대로 인식되어 텍스트로 취급된다. 즉, HTML 마크업이 파싱되지 않는다.
    

- innerText 프로퍼티
    
    - textContent 프로퍼티와 유사한 역할, 동작을 하지만 사용이 권장되지는 않는데, 그 이유로
        
        - innerText 프로퍼티는 CSS에 순종적이다. 예를 들어, innerText 프로퍼티는 CSS에 의해 비표시 ( visibility: hidden; )로 지정된 요소 노드의 텍스트를 반환하지 않는다.
        
        - innerText 프로퍼티는 CSS를 고려해야 하므로 textContent 프로퍼티보다 느리다.
        
    

  

## DOM 조작

- DOM 조작에 의해 DOM에 새로운 노드가 추가 혹은 삭제되면 리플로우와 리페인트가 발생하는 원인이 되므로 성능에 영향을 준다.

- Element.prototype.innerHTML
    
    - HTML 마크업은 무시하고 텍스트만 반환하는 textContent와는 달리 innerHTML 프로퍼티는 HTML 마크업이 포함된 문자열을 그대로 반환한다.
    
    - innerHTML을 사용해 문자열을 할당하면 요소 노드의 모든 자식 노드가 제거되고 할당한 문자열에 포함되어 있는 HTML 마크업이 파싱되어 요소 노드의 자식 노드로 DOM에 반영된다.
    
    - 요소 노드의 innerHTML 프로퍼티에 할당한 HTML 마크업 문자열은 렌더링 엔진에 의해 파싱되어 요소 노드의 자식으로 DOM에 반영된다.
    
    - 사용자의 입력을 innerHTML로 그대로 받아 사용하는 것은 XSS에 취약
    
    - HTML5는 innerHTML 프로퍼티로 삽입된 script 요소 내의 자바스크립트 코드를 실행하지 않는다.
    
    - innerHTML의 또 다른 단점으로는, 요소 노드의 innerHTML 프로퍼티에 HTML 마크업 문자열을 할당하는 경우, 요소 노드의 모든 자식 노드를 제거하고 할당한 HTML 마크업 문자열을 파싱해 DOM을 변경한다는 것이다. 이는 기존에 존재하는 요소 노드와 할당하려는 요소 노드가 동일하다 하여도 모두 제거하고 새롭게 요소 노드를 DOM에 반영하기 때문에 효율적이지 않다.
    
    - 또한, 기존 요소 사이에 새로운 요소를 할당하려 해도 위치를 직접 지정할 순 없다.
    

- Element.prototype.insertAdjacentHTML(position, DOMString)
    
    - 기존 요소를 제거하지 않으며 특정한 위치를 지정해 새로운 요소를 삽입할 수 있다.
    
    - 첫 번째 인자인 position의 경우, beforebegin, afterbegin, beforeend, afterend이며, 위치는 아래와 같다.
    
    [![](https://ko.javascript.info/article/modifying-document/insert-adjacent.svg)](https://ko.javascript.info/article/modifying-document/insert-adjacent.svg)
    
    - 기존 요소를 제거하지 않고 새로운 요소를 삽입할 수 있어 textContent 프로퍼티보다 효율적이긴 하지만, 여전히 XSS에는 취약하다.
    
      
    

- 노드 생성, 추가
    
    - 노드 생성
        
        - Document.prototype.createElement(tagName)
        
        - 위 메서드로 생성한 요소는 최초에 기존 DOM에 추가되지 않고 홀로 존재하는 상태가 된다.
        
        - 위 메서드는 요소를 생성만 할 뿐, DOM에 추가하지는 않으며, 생성된 요소를 DOM에 추가하는 작업이 필요하다.
        
    
    - 텍스트 노드 생성
        
        - Document.prototype.createTextNode(’문자열’)
        
        - creatElement 메서드와 같이 텍스트 노드가 생성만 되어 있으며, 어떠한 상위 노드에 추가되어 있지는 않은 상태이다.
        
    
    - 텍스트 노드를 요소 노드의 자식 노드로 추가
        
        - Node.prototype.appendChild(childNode)
        
        - childNode에게 인수로 전달한 노드를 appendChild 메서드를 호출한 노드의 마지막 자식 노드로 추가한다.
        
    
    - 요소 노드를 DOM에 추가
        
        - 위와 동일하게 appendChild 메서드를 사용한다.
        
    
    > [!important] 위 과정이 기존의 DOM에 새로운 요소를 추가하는 한번의 과정이며, 기존의 DOM에 단 한번 추가되므로 DOM은 한 번 변경되며, 이 때 리플로우와 리페인트가 실행된다.
    
      
    

- DocumentFragment 노드
    
    - 문서, 요소, 어트리뷰트, 텍스트 노드와 같은 노드 객체의 일종으로, 부모 노드가 없어서 기존 DOM과는 별도로 존재한다는 특징이 있다.
    
    - 위 노드는 자식 노드들의 부모 노드로서 별도의 서브 DOM을 구성해 기존 DOM에 추가하기 위한 용도로 사용한다.
    
    - 이 노드는 기존 DOM과는 별도로 존재하므로 DocumentFragment 노드에 자식 노드를 추가 해도 기존 DOM에는 어떠한 변경도 발생하지 않는다.
    
    - 또한, DocumentFragment 노드를 기존 DOM에 추가하면 자신은 제거되고 자신의 자식 노드만 DOM에 추가된다.
    
    ```jsx
    // DocumentFragment 노드 생성
    
    const $fragment = document.createDocumentFregment();
    ```
    
    - 이 방식을 사용해 DOM에 요소 노드를 추가했을 때는 DocumentFragment 노드를 기존 DOM에 추가했을 당시 한 번만 리플로우와 리페인팅이 일어난다.
    
      
    

- 노드 삽입
    
    - 마지막 노드로 추가
        
        - Node.prototype.appendChild
        
        - 인수로 전달 받은 노드를 자신이 호출한 노드의 마지막 자식 노드로 DOM에 추가한다.
        
        - 추가할 위치는 정할 수 없으며, 항상 마지막에 추가된다.
        
    
    - 위치를 지정해 삽입
        
        - Node.prototype.insertBefore(newNode, childNode)
        
        - 첫 번째 인수로 전달 받은 노드를 두 번째 인수로 전달 받은 노드 앞에 삽입한다.
        
        - 두 번째 인수로 전달받은 노드는 반드시 insertBefore 메서드를 호출한 노드의 자식 노드이어야 한다.  
            
        
    

- 노드 이동
    
    - DOM에 이미 존재하는 노드를 appendChild 또는 insertBefore 메서드를 사용해 DOM에 다시 추가하면 현재 위치에서 노드를 제거하고 새로운 위치에 해당 노드를 추가한다.
    

- 노드 복사
    
    - Node.prototype.cloneNode ( [ deep: true | false ] )
    
    - 노드의 사본을 생성, 반환
    
    - 매개변수 deep에 true를 인수로 전달하면 노드를 깊은 복사해 모든 자손 노드가 포함된 사본을 생성하고,
    
    - false를 전달하거나 생략하면 얕은 복사를 해 노드 자신만의 사본을 생성한다. 얕은 복사된 노드는 자손 노드를 복사하지 않으므로 텍스트 노드도 없다.
    

- 노드 교체
    
    - Node.prototype.replaceChild ( newChild, oldChild )
    
    - 자신을 호출한 노드의 자식 노드를 다른 노드로 교체한다.
    
    - 첫 번째 매개변수 newChild에는 교체할 노드, 두 번째 매개변수 oldChild는 이미 존재하는 교체될 노드를 인수로 전달한다.
    
    - 교체된 oldNode는 DOM에서 삭제된다.
    

- 노드 삭제
    
    - Node.prototype.removeChild ( child )
    
    - 매개변수에 인수로 전달한 노드를 DOM에서 삭제한다.