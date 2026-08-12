### 모노레포 상황에서 pacakges/ 의 공통 모듈 사용 방안

- **방안 1. apps/ 내부의 각 app이 모듈에 렌더링 될 모든 정보를 가지고 있고, 공통 모듈은 주입만 받는 방식 - 기존 방식**
	- apps/ 내부의 앱들이 모든 설정, store, api 요청 권한을 가지고 있고
	- packages/ 의 특정 모듈에 app 에 종속되는 정보를 주입해서 packages/ 내부의 공통 모듈들은 app 에 대한 정보를 몰라도 되게 하는 것이 목표
	- 그러기 위해서는 props, adapter, provider, inject, store state/action gettter/setter 를 사용해 공통 모듈이 app에 종속되는 정보들을 받기만 할 수 있도록 만들어야 했음
	- 위 방법은 복잡도가 높아지지만 공통 모듈이 app의 정보를 몰라도 된다는 점에서 완전한 공통 모듈화에 가까운 방식
	- 위 방식은 store state/action, api 등 공통으로 사용할 UI 값들 외에는 모두 app이 가지고 있어야 한다.

- **방안 2. 현재 구조 그대로 공통 모듈이 store state/actions, api를 관리하는 방식**
	- 이 방식은 UI, store state/actions, api 요청 방식 등 모든 공통의 요소가 앱별로 변화가 거의 없다고 할 때 오히려 비용은 적게 들고 개발도 쉬운 방향으로 진행할 수 있다.
	- 공통 모듈 별로 config 파일을 사용해서 어떤 app 인지에 따라 그에 맞는 config를 공통 모듈에 적용시키는 방식으로 적용한다.
	- config에 앱별로 구별해서 적용해야 할 정보가 테이블 칼럼, api 엔드 포인트, 파라미터 정도인 경우 해당 방식이 합리적이라고 할 수 있다.