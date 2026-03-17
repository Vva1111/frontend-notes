#### .call() - GPT, MDN
- 함수의 호출 방식을 제어하는데 사용되는 메서드
- 이미 할당되어있는 다른 객체의 함수/메서드를 호툴하는 해당 객체에 재할당할때 사용
- this는 현재 객체(호출하는 객체)를 참조한다. 메서드를 한번 작성하면 새 객체를 위한 메서드를 재작성할 필요 없이 call()을 이용해 다른 객체에 상속할 수 있다.
- 특정 객체를 this 값으로 설정해 함수를 호출할 수 있게 해준다. 즉, 해당 메서드를 사용하면 함수가 호출될 때 this가 어떤 객체를 참조할지 명시적으로 지정할 수 있다.
```ts
function greet(){
	console.log(`Hello, My Name is ${this.name}`)
}

const person = {
	name: 'Alice'
}

// call()을 사용해 person 객체를 this로 설정
greet.call(person);
```

#### Object.prototype.toString.call(value) - GPT
- 객체의 타입을 확인할 수 있는 메서드
- 객체의 내부 \[[Class]] 속성을 반환해 객체가 어떤 종류인지 판별 가능하게 한다.
- value: 객체 타입을 확인할 대상
- Object.prototype.toString.call([]) - \[object Array] 를 반환
	Object.prototype.toString.call({}) - \[object Object] 를 반환
	Object.prototype.toString.call(null) - \[object Null] 을 반환
	Object.prototype.toString.call(undefined) - \[object Undefined] 를 반환
	Object.prototype.toString.call(new Date()) - \[object Date] 를 반환
	Object.prototype.toString.call(/regex/) - \[object RegExp] 를 반환
- 해당 방식은 typeof 혹은 instanceof 연산자보다 더 정확한 타입을 반환한다.
- 기타 방식으로는 
	- typeof: 기본 데이터 타입만 확인, null과 같은 특정 객체 판별 불가
	- instanceof: 해당 객체가 특정 생성자(Array, Object, Date 등)의 인스턴스인지 확인하는데 사용되는 연산자
	- Array.isArray(value): 해당 value가 배열인지 확인할 떄 사용
	- object.getPrototypeOf(): 객체의 프로토타입을 확인해 유추할 수 있지만 타입을 직접적으로 반환하지는 않는다.


#### Object.entries(obj) - GPT
- 객체의 속성과 그 값을 배열 형태로 반환하는 메서드
- 객체의 각 속성을 \[key, value] 쌍의 배열로 변환해 2차원 배열을 생성한다.
- 객체의 속성을 반복하거나, 다른 데이터 구조로 변환할 때 유용하며, for ... of 루프를 사용해 객체의 속성을 순회할 수 있다.
- 해당 메서드는 객체의 열거 가능한 속성만 반환한다. 즉, 상속된 속성이나 비 열거 속성은 포함되지 않는다.

#### .startsWith()
- 문자열이 특정 문자열로 시작하는지 확인하고, Boolean 값 반환
```ts
str.startsWith(searchString[, position])
```
- searchString: 확인할 문자열
- position: 옵션, 검색을 시작할 위치를 지정
