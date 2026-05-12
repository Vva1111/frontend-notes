[https://chat.openai.com](https://chat.openai.com)

출처. Chat GPT

  

- 자바스크립트의 Symbol type은 변경 불가능 한 값.

- 고유하고, 다른 어떤 값과도 동등하지 않은 값을 생성할 때 사용.

  

- 하지만 Symbol 값을 사용해 생성한 객체의 프로토타입은 변경 가능하다.

- 이것은 Symbol을 사용해 프로토타입을 변경하는 것이 아니라, 객체의 프로토타입 자체를 변경하는 것이다.

![[Untitled 368.png]]

- 위 코드에서는 obj2의 프로토타입을 newPrototype으로 변경했다. 이로 인해 obj2는 newPrototype의 프로퍼티에 접근할 수 있게 된다.

- 하지만 이러한 프로토타입 변경은 Symbol 값 자체를 변경하는 것이 아니라, 프로토타입 체인을 수정하는 것이다.

- 따라서 Symbol 값 자체를 변경할 수는 없고, 그 값을 가진 객체를 변경하는 것은 가능하다.

- 프로토타입을 사용한 Symbol 값 자체의 변경은 불가능하다.