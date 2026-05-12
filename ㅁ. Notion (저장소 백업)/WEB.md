## 내가 보려고 만드는 개발 노션

[[※ 틀린 정보가 있을 수 있음( 지속적으로 수정 중…)]]

  

### front

  

  

[[HTML]]

[[JavaScript]]

[[Vue.js]]

[[Quasar Framework]]

[[Effective TypeScript]]

[[CSS]]

[[Jquery]]

[[React.js]]

[[Vuex(수정 필요)]]

---

### Back

  

[[Python]]

[[Node.js]]

  

---

### DataBase

  

[[여러 개념]]

[[ㅁ. Notion (저장소 백업)/SQLite]]

---

### 네트워크

  

[[REST API]]

[[GATEWAY]]

[[PROXY]]

[[URL 입력 시 일어나는 일]]

[[TCP-IP]]

[[OSI 7계층 모델]]

[[ROUTING]]

[[CORS]]

  

---

### 협업 / CICD

[[Git]]

[[완벽한 IT 인프라 구축을 위한 Docker]]

[[Docker]]

  

---

  

### 기본 개념

- npm run build
    
    → npm run build는 서비스 최종 배포 전 배포 버전 코드를 새로 만드는 명령어
    
    → 메모리를 최대한 절약하기 위해 공백 등 필요 없는 사항들을 제거한 코드를 제공
    
    → build 하게 되면 build 디렉토리가 생성되며, 내부의 index파일에 배포 코드가 생성된다.
    
    - 명령어
        
        - npm run build : 배포 버전의 코드를 생성
        
        - serve -s build : 사용자가 어떤 경로로 들어오던 시작은 빌드 된 index파일로 안내한다.
        
        - npx serve -s build : 배포 버전 코드를 바탕으로 어플리케이션을 실행한다.