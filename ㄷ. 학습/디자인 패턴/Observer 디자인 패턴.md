- 한 객체의 상태 변경을 여러 객체에게 자동으로 통지하고, 각 Observer가 그 변경에 반응하도록 만드는 디자인 패턴
- 상태(또는 이벤트) 발생지와 그 반응자들을 느슨하게 결합하는 패턴

### 구성 요소
- **Subject** ( 또는 Observable, Publisher ) - 상태를 가지고 있고, 상태가 바뀔 때 등록된 모든 Observer에게 알림을 보냄
- **Observer** ( 또는 Subscriber ) - Subject의 변경을 구독하고, 알림을 받으면 자신의 로직을 실행
- **ConcreteSubject / ConcreteObserver**
	구체 구현체들. Subject는 attach(observer), detatch(observer), notifu() 같은 메서들를 가진다.

### 동작 흐름
1. Observer가 Subject에 등록(구독)한다.
2. Subject의 상태가 변경된다.
3. Subject는 notify()를 호출해 모든 등록된 Observer에게 알림을 보낸다.
4. 각 Observer는 알림을 받아 필요한 업데이트를 수행한다.

### Push vs Pull
- **Push 방식** - Subject가 변경 데이터(또는 이벤트 객체)를 알림 시 함께 보낸다. Observer는 전달 받은 데이터만으로 처리
	- 장점 : Observer가 필요한 데이터 모두 즉시 받음
	- 단점 : 불필요한 데이터가 전달될 수 있음
- **Pull 방식** - Subject는 단순히 "변경 발생"만 알리고, Observer가 필요한 데이터를 Subject에서 직접 조회 한다.
	- 장점 : Observer가 원하는 데이터만 가져옴
	- 단점 : Observer가 Subject에 의존해 데이터를 조회하므로 약간 더 결합될 수 있음

### 프론트엔드에서의 실제 사용 예시
- **UI 상태 관리** : 전역 스토어 변화 발생 시 여러 컴포넌트가 구독하고 업데이트
- **DOM 이벤트** : element.addEventListener 이벤트(suubject) 발생 -> 리스너(observer)들 호출
- **폼 유효성 검사 플러그인** : 입력 변화에 따라 여러 검증/경고 로직 구동
- **웹소켓/서버 푸시** : 서버에서 들어오는 메시지를 여러 UI 컴포넌트에 전파


### 장/단점
- **장점**
	- 느슨한 결합 : Subject와 Observer는 서로 구체 구현을 몰라도 됨
	- 확장성 : Observer 추가/제거가 쉬움
	- 분리된 관심사 : 데이터 공급과 반응 로직 분리
- **단점**
	- 메모리 누수 : 구독을 해제하지 않으면(특히 컴포넌트 언마운트 시) 누수가 발생한다. 항상 unscribe를 관리해야 함
	- 복잡도 증가 : 많은 옵저버와 복잡한 알림 흐름은 디버깅을 어렵게 한다
	- 순서/동시성 문제 : 동기/비동기 알림에서 타이밍 이슈가 발생할 수 있음
	- 잘못된 설계로 인해 성능 저하(과도한 재 렌더링 등)가 발생할 수 있음

### 실무 적용
- 구독 해제를 반드시 할 것
- 하나의 에러가 다른 구독에 영향을 주지 않도록 try/catch 처리
- 만은 변화가 짧은 시간에 발생하면 Batching 해서 한 번만 알리기 (React batchUpdates / Vue nextTick)
- 필요한 데이터만 전달
- 스레드 / 비동기 고려
- 많은 컴포넌트가 서로 직접 구독/발행하는 대신 Mediator/Store를 두는 것이 관리에 유리하다.


### React에서 Observer 패턴을 이해하는 포인트
1. React의 렌더링 트리거는 state 변경 -> 'Observer 통지'와 동일
2. useEffect cleanup = 구독 해제
3. 전역 상태 관리(store)는 Observable 데이터
4. 커스텀 훅은 OObservable <-> Component 간 브릿지
5. RxJS, SWR, React Query 등도 모두 Observer 기반