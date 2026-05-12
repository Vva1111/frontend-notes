> [!info] [JavaScript] Virtual DOM 만들기  
> React 공식 홈페이지에서는 Virtual DOM을 'UI로 표현될 객체를 가상 메모리에 저장하여 라이브러리에 의해 실제 DOM으로 동기화 하는 개념'으로 표현하고 있습니다.  
> [https://jungpaeng.tistory.com/91](https://jungpaeng.tistory.com/91)  

  

## Virtual DOM

- ‘UI로 표현될 객체를 가상 메모리에 저장해 라이브러리에 의해 실제 DOM으로 동기화 하는 개념’

- React에 의해 개척된 개념

- UI로 표현 될 객체는 DOM을 의미하며, 라이브러리는 Virtual DOM을 렌더링 해주는 라이브러리를 의미

- 리액트는 두 가지 전략을 사용해 diff 알고리즘을 기존의 O(n^3) 에서 O(n)으로 성능 개선

  

## Virtual DOM의 이점

- Virtual DOM은 DOM이 변경될 때 전체 DOM을 Reflow 하는 것이 아닌, 가상의 DOM을 이용해 한 번만 Reflow를 수행할 수 있도록 해 DOM의 부하를 줄여주는 이점이 있다.

  

## React의 diff 알고리즘

- diff 알고리즘의 주요 작업은 어떤 한 상태에서 다른 상태로 변경하는 가장 합리적인 방법을 찾아내는 것이다.

- A 상태에서 B 상태로 변경하는 최소한의 변경이 필요하지만 일부 응용 프로그램은 각각 영향을 미치는 기타 요인에 따라 작업에 무게를 두어야 할 수 있다.

  

## diff 알고리즘의 필요 이유

- React 컴포넌트를 처음 렌더링할 때 모든 엘리먼트의 트리가 만들어진다.

- state 또는 props의 업데이트에 의해 render() 함수가 다시 호출된다면 React Element를 업데이트 한다.

- 여기에서 원래 상태와 변경된 상태에 대한 리소스의 가장 높은 효율을 식별하는 것인데, 이 문제에 대한 일반적인 해결책으로는 O(n^3)의 복잡성을 가지는 알고리즘이 있다. (n : 트리 엘리먼트의 수)

  

## React의 관점

- 위의 접근 방식을 사용한다면 1000개의 element를 업데이트 하기 위해서는 10억번의 비교가 필요할 것이다.

- 이것은 비효율적인 방법으로, React는 다음의 두 가정을 기반으로 O(n) 알고리즘을 구현해냈다.
    
    1. 서로 다른 타입을 가진 두 element는 서로 다른 트리를 만들어낸다.
    
    1. 개발자가 key prop을 사용해 하위 element의 변경 여부를 표시할 수 있다.
    

  

## Different root element

- Virtual DOM 트리의 Root element 타입이 달라진다면, DOM 트리를 버린 다음, 새로운 트리를 구축한다.
    
    - **Point A 에서**
        
        ```jsx
        <p> <MyComp /> </p>
        ```
        
    
    - **Point B 로 변경**
        
        ```jsx
        <span> <MyComp /> </span>
        ```
        
        위의 경우, **Point A**를 마운트 해제(`componentWillUnmount` : React LifeCycle Hook) 한 뒤,
        
        **Point B**를 마운트(`componentWillMount` -> `componentDidMount` : React LifeCycle Hook) 한다.
        
          
        
        element가 제거된다면 해당 element와 하위 element의 state 값 역시 사라진다.
        
    

  

  

## Same root element

- 루트 element가 동일하다면, 모든 props를 비교해 동일한 값은 유지하고, 새롭게 추가되거나 변경된 props만 적용한다.

- 스타일의 갱신 역시 변경된 사항만 갱신한다.
    
    - **Point A 에서**
        
        ```jsx
        <div className="comp1" style={{color: 'red', backgroundColor: 'blue'}}>
        Test
        </div>
        ```
        
    
    - **Point B 로 변경**
        
        ```jsx
        <div className="comp2" style={{color: 'red', backgroundColor: 'black'}}>
        Test
        </div>
        ```
        
    

  

## All the Elements are Of The Same Type

- 컴포넌트가 갱신되면 인스턴스는 동일하게 유지되며, 렌더링 사이 state 값이 유지된다.

- React는 새로운 엘리먼트와 일치하도록 컴포넌트의 props를 업데이트 한 뒤, `componentWillReceiveProps` 및 `componentWillUpdate` 를 호출한다.

- 위 과정을 거친 뒤, render() 메서드가 호출되어 이전 결과와 비교하기 위해 diff 알고리즘을 재귀 호출한다.

  

## Recursion on Child element

- React는 리스트를 비교할 때 순차적으로 비교해 가며 차이점이 있다면 변경한다.

- 순회 비교를 하므로, element 리스트의 앞에 추가하는 것보다는 뒤에 추가하는 것이 성능상 좋다.

- 예시 1)
    
    - **Point A**
        
        ```jsx
        <ul>
          <li>a</li>
          <li>b</li>
        </ul>
        ```
        
    
    - **Point B**
        
        ```jsx
        <ul>
          <li>a</li>
          <li>b</li>
          <li>c</li>
        </ul>
        ```
        
        위의 두 코드는 <li> 목록 a와 b가 동일하다는 것을 파악한 뒤 <li> 목록 c 를 추가하므로 성능상의 문제가 없다.
        
          
        
    

- 예시 2)
    
    - **Point A**
        
        ```jsx
        <ul>
          <li>a</li>
          <li>b</li>
        </ul>
        ```
        
    
    - **Point B**
        
        ```jsx
        <ul>
        	<li>c</li>
          <li>a</li>
          <li>b</li>
        </ul>
        ```
        
        위의 코드와 같이 변경 사항이 맨 앞에 삽입되는 경우, React는 a(old)와 c(new)를 비교하면서 하위 트리를 유지할 수 없다고 판단 후 모든 하위 개체를 변경한다. 이럴 경우, 성능상 문제 발생 가능성이 높다.
        
          
        
    

## Keys

- Point A
    
    ```jsx
    <ul>
      <li key='1'>a</li>
      <li key='2'>b</li>
    </ul>
    ```
    

- Point B
    
    ```jsx
    <ul>
      <li key='3'>c</li>
      <li key='1'>a</li>
      <li key='2'>b</li>
    </ul>
    ```
    
    목록의 중간 또는 맨 앞에 새로운 element가 추가되는 경우, 성능상 문제가 발생하는 것을 방지하기 위해 key props를 지원한다. React는 key props를 이용해 비교할 두 트리를 결정한다.
    
    key 값이 동일한 두 트리를 비교한 후, 차이점이 발생하면 트리를 업데이트 한다.
    
    이러한 특징 때문에 React에서 반복문을 사용해 Element를 생성할 때 일반적으로 key props를 요구한다.
    
    주의할점으로는, key props에 index 속성을 사용한다면 element 항목이 재 배열 되는 경우 key 값이 변경될 수 있어 목록이 추가 / 제거 될 가능성이 있다면 key에 index 사용을 권장하지 않는다.
    

  

## React의 createElement

```jsx
const Component = React.createClass({
  render: function() {
    if (this.props.first) {
      return (
        <div className="first">
          <span>A Span</span>
        </div>
      );
    } else {
      return (
        <div className="second">
          <p>A Paragraph</p>
        </div>
      );
    }
  }
});
```

- React의 render 결과는 실제 DOM 노드가 아닌 자바스크립트 객체로 반환되며, render를 통해 생성된 자바스크립트 객체를 Virtual DOM이라고 칭한다.

- React는 위에서 알아본 절차를 거쳐 첫 render 이후 render를 통해 업데이트 할 때 가장 효율적인 방법을 찾는다.

- 위의 예시를 통해 `<Component first={true} />` 에서 `<Component first={false} />` 로 변경하고, 컴포넌트를 unmount 시킬 때 DOM은 아래의 과정을 수행한다.
    
    1. 노드 생성
        
        ```jsx
        <div className="first"><span>A Span</span></div>
        ```
        
    
    1. attribute, 노드 교체
        
        `className="first”` 을 `className="second”` 로 교체,
        
        `<span>A Span</span>` 을 `<p>A Paragraph</p>` 으로 교체
        
    
    1. 노드 삭제
        
        `<div className="second">` , `<p>A Paragraph</p></div>` 삭제
        
          
        
          
        
    

## Virtual DOM 생성

![[Untitled 351.png]]

![[Untitled 352.png]]

  

- 왼 쪽의 element는 오른쪽의 JS 객체로 구현할 수 있다. 이는 React가 DOM을 저장하는 방법과 같다.

- 하지만 큰 규모의 트리를 직접 작성하기는 어렵기 때문에 createVirtualDOM 헬퍼 함수를 사용할 수 있으며, 해당 함수를 사용하면 아래와 같은 JS 객체 결과물이 생성된다.

![[Untitled 353.png]]

그림. createVirtualDOM 함수를 이용해 만든 JS 객체

## 실제 DOM 생성

- 객체로 저장한 Virtual DOM을 활용해 실제 DOM을 생성해내야 한다.

- 가상 돔 노드를 가져와 실제 돔 노드를 생성하는 createElement 함수를 사용한다.

![[Untitled 354.png]]

- 텍스트 노드로 전달하는 경우, 위와 같이 처리할 수 있다.

- 하지만 객체 형태의 엘리먼트를 처리하기 위해서는 children 배열에 대한 처리가 필요하며, 이것 또한 텍스트 노드 또는 엘리먼트이기 때문에 createElement 함수로 생성할 수 있다.

![[Untitled 355.png]]

- 엘리먼트의 children에 대해 createElement 함수를 호출하고 해당 엘리먼트에 appendChild() 하면 위와 같다.

  

## 변경 사항 처리

- Virtual DOM을 실제 DOM으로 변경한 후, Virtual DOM의 변화를 감지하는 알고리즘을 작성해야 한다.

- 해당 알고리즘은 old, new 두 개의 트리를 비교해 실제 DOM 변경에 필요한 만큼 수행된다.
    
    - 노드가 새로 추가된 경우, appendChild 를 통해 노드를 추가
    
    - old에 있던 내용이 new에서 제거되는 경우, removeChild 를 통해 해당 노드를 제거
    
    - 특정 위치의 노드가 달라지는 경우, 해당 노드를 replaceChild 를 통해 변경
    
    - children이 중첩되어 있는 경우, 하위 노드까지 비교 필요
    
    위 네 가지만 반영해 아래의 함수를 만든다.
    
    ![[Untitled 356.png]]
    
    위 함수는 parent, newNode, oldNode 세 개의 파라미터를 받는다.
    
      
    

- **노드가 새로 추가**되는 경우, appendChild 와 createElement 함수를 이용해 작성할 수 있다.

  

- **노드를 삭제**해야 하는 경우,
    
    - New Virtual Tree에 노드가 없는 경우, 실제 DOM에서 제거해야 한다.
    
    - 상위 엘리먼트를 알고 있으므로, $parent.removeChild 를 호출한 뒤, 해당 함수에 실제 DOM 엘리먼트의 reference를 전달해야 한다.
    
    - 하지만 현재 해당 reference를 가지고 있지 않으므로, $parent.childNodes[index]로 reference를 얻을 수 있다.
    
    ![[Untitled 357.png]]
    

  

- **노드가 변경** 되는 경우,
    
    - 두 노드를 비교하고, 노드가 실제로 변경되었는지 알려주는 함수를 작성해야 한다. 노드는 텍스트 노드 또는 엘리먼트 노드가 될 수 있다.
        
        ![[Untitled 358.png]]
        
    
    - 상위 노드에서 현재 노드의 index 값을 활용해 새로 생성된 노드로 쉽게 대체할 수 있다.
    
    ![[Untitled 359.png]]
    

  

- **Children 비교**
    
    - 마지막으로, 두 노드의 모든 자식을 재귀로 비교해야 한다.
    
    - 텍스트 노드는 자식을 가질 수 없으므로 엘리먼트 노드만 비교되어야 한다.
    
    ![[Untitled 360.png]]