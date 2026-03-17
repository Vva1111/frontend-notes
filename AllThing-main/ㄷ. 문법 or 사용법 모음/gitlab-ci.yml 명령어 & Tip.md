
#### Tip.
- gitlab-cy.yml 의 전역변수는 파이프라인 실행 시 최초 한번만 읽고, 이후에는 rules의 분기 처리에 따라 변하지 않는다. 따라서 분기 처리에 따른 변수의 변경은 job 단위로 설정이 되어야 한다.

#### GitLab에서 직접 제공하는 환경 변수
- $CI_COMMIT_REF_NAME : 현재 커밋이 속한 브랜치의 이름
- $CI_COMMIT_SHA : 현재 커밋의 SHA 해시
- $CI_JOB_ID : 현재 작업의 고유 식별자
- $CI_JOB_STAGE : 현재 작업이 속한 스테이지 이름
- $CI_COMMIT_REF_NAME : 현재 커밋이 속한 브랜치의 이름
- 등등

	 위 환경 변수들은 GitLab에서 자동으로 제공되며, cicd가 시작되는 조건 등의 설정에 사용할 수 있다.