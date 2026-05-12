---

- Vue의 Lifecycle ( 수정필요 )
    
    > Vue의 인스턴스는 생성(Create), DOM에 부착(mount), 업데이트(update), 종료/삭제(destroy) 의 4과정을 거친다.  
    > Hooks  
    - beforeCreate : Vue 인스턴스가 생성 > 초기화 된 직후,  
    DOM, data, event, methods 등 모두 아직 정의되어 있지 않기 때문에 모두 접근할 수 없다.  
    - created : data, computed. methods 등에 접근 가능하지만, DOM에는 접근할 수 없다.  
    - beforeMount : Vue가 DOM에 부착되기 직전, 가상 DOM이 생성은 되어 있으나, 부착되지 않았기 때문에 접근할 수는 없다.  
    - mounted : 가상 DOM의 내용이 실제 DOM에 부착되고 난 이후, 모든 요소에 접근이 가능하다.  
    * 일반적인 경우, 부모 컴포넌트의 mounted훅은 항상 자식 컴포넌트의 mounted훅 이후에 발생하지만,  
    자식 컴포넌트에서 비동기로 데이터를 받아오는 경우처럼  
    부모의 mounted 훅이 모든 자식 컴포넌트의 마운트 상태를 보장하지는 않는다.  
    이 때를 대비해 위의 this.$nextTick()을 사용하게 된다.  
    - beforeUpdate : 컴포넌트에서 사용되는 data의 값이 변하고, 해당하는 값을 DOM에 적용시켜야 할 때 사용  
    * beforeUpdate를 이용하면 값들을 추가적으로 변화시켜도 렌더링을 추가로 호출하지는 않는다.  
    - updated : 가상 DOM을 렌더링하고 실제 DOM이 변경된 이후에 호출  
    > 변경 된 data가 DOM에도 적용 된 상태  
    > 변경 된 값들을 DOM을 이용해 접근하고 싶다면 가장 적절  
    > 하지만 이 단계에서 data를 변경하는 경우, 무한 루프를 일으킬 수 있기 때문에 data를 직접 변경하는 것은 적절하지 않다.  
    > mounted와 같이, this.$nextTick()을 이용해 모든 화면이 업데이트 된 이후의 상태를 보장할 수 있다.  
    - beforeDestroy : 해당 인스턴스가 해체되기 직전에 호출, 해체되기 직전이므로 모든 속성에 접근 가능하다.  
    > 이 단계에서는 이벤트 리스너를 해제하는 등, 인스턴스가 사라지기 전에 해야할 일들을 처리  
    - destroyed : 인스턴스가 해체되고 난 직후에 호출
    
      
    
    ※ this.$nextTick( callback )
    
    → data 가 업데이트 되고 난 직후, UI 갱신될 때, Vue가 data를 렌더링 할 DOM을 찾지 못하는 경우가 있다.  
    > 비동기로 처리되는 jsvascript의 특성 때문에 일어나는 현상인데  
    > data 로딩과 ui 로딩의 순서가 꼬여서 발생하는 문제  
    > 이런 경우에 this.$nextTick() callback 메소드를 사용한다.  
    > this.$nextTinc() 의 callback 함수 내부에서 DOM을 조작하면  
    데이터를 갱신 후 UI 로딩 완료한 뒤에 nextTick 내부의 callback함수를 최종적으로 수행한다.