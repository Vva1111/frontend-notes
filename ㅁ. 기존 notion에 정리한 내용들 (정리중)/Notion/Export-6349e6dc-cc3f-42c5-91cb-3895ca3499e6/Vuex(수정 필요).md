※ ( [https://doozi0316.tistory.com/entry/Vuex-개념과-예제-이해하기](https://doozi0316.tistory.com/entry/Vuex-%EA%B0%9C%EB%85%90%EA%B3%BC-%EC%98%88%EC%A0%9C-%EC%9D%B4%ED%95%B4%ED%95%98%EA%B8%B0) 참고 )

Vuex  
- state : 상태(state)의 집합  
- 단일 상태 트리(single state tree) : 하나의 어플리케이션은 하나의 state만 가진다는 의미  
하나의 어플리케이션은 하나의 store만 가지기 때문에 현재 state의 상태를 쉽게 찾을 수 있다.  
* state의 상태가 하나의 어플리케이션 내에서 컴포넌트에 상관 없이 유일한 값을 가진다.  
- state : store 내의 state에 접근해 현재 컴포넌트로 해당 state를 가져온다  
* this.$store.state.state이름 방식으로 접근한다.  
- mapState : computed에 많은 state가 선언되면 코드가 복잡(this.$store.state.~~ 이 다수 생성)해질 수 있기 때문에  
Vuex에서 제공하는 Helper  
computed 내부에 사용하며, ...mapState(['storestate1', 'storestate2']) 와 같은 방식으로 state를 가져온다  
- getter : state를 계산한 값을 사용해야 하는 경우 사용한다  
각각의 컴포넌트에서 state의 변경이 잦으면 state를 반복해서 호출하고, 호출이 잦아지면 효율이 좋지 않다.  
> store 내부에서 getter를 이용해 계산 된 값을 컴포넌트에서 가져오는 방식이며,  
store 내부의 getter 메소드는 반환값(getter / state)을 가져야 한다.  
- mapGetters : mapState와 같이 getter 코드의 반복을 막기 위해 사용한다. (전개 구문 사용)  
- mutations : getters와 비슷하게 state의 값을 변환할 때 사용한다.  
- getters와의 차이점은, 전달 인자를 받을 수 있으며, computed가 아닌 methods에 등록해 사용한다는 점이다.  
- 동기적 로직(순차적 변경 진행)을 정의한다는 특징을 가진다.  
- 여러 컴포넌트에서 state를 변경한다면 state의 변경을 추적하기 어렵기 때문에 mutations에 정의한 commit 함수를 이용해  
명시적으로 state를 호출하면 상태를 추적할 수 있다.  
- mapMutations : 다른 map~과 같이 코드를 간결하게 해주며, methods에 정의한다.( 전개구문 사용 )  
- ...mapMutations(['storestate1','storestate2']) 와 같이 사용  
- mapMutations를 사용함으로 commit문법응 작성하지 않고도 자동으로 사용할 수 있게 해준다.  
- actions : 동기적 처리인 mutations와 달리, 비동기 처리를 다루는 actions  
- store 내부의 actions에 setTimeout() 혹은 http 통신과 같은 비동기 처리 로직을 선언한다.  
- actions 역시 비동기적 상태를 추적해야 하기 때문에 commit을 사용한다  
- actions 의 경우, 컴포넌트 methods에서 commit 대신 dispatch를 사용한다.  
- store actions 메소드 내에서 비동기적으로 commit을 호출하기 때문에 actions가 비동기적 로직을 다룬다고 한다  
- 비동기적 처리를 다루기 때문에 promise, then / async / await를 함께 사용한다  
- mapActions : 다른 map~과 같이 코드의 반복을 막고, dispatch 를 명시하기 위해 사용한다.

```
- store 인스턴스에서 modules 속성을 사용해 모듈화 할 수 있다.
- 모듈화 한 이후에 namespace를 지정하지 않는다면, 여러 모듈들은 여전히 전역 namespace 내에 존재하기 때문에
	다른 모듈에 적용된 변경 사항도 적용되게 된다. 그렇기 때문에 모듈화를 해서 분리된 state는 namespace를 사용해
	구분해야한다.

- createNamespaceHelpers를 생성해 헬퍼에서 namespace를 간단하게 사용할 수 있다.
```