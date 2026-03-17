#### 패키지 빌드 이미지가 설치될 서버 정보는 runner에서 파악

#### 분리 구성 단계
- 기존 Main Branch 제거
- BT Branch를 Main으로
- Main(기존 BT Branch) branch에서 분리된 두 개의 Branch 생성
	1. 1.0 - Phase 2 ( 2차 개발 )
	2. 1.1 - Phase 3 ( 3차 개발 - 추후 개발 기준이 될 Branch )

#### vite.config.ts
- **target URL 변경?** 
	각자 개별 도메인을 가지는 경우 변경. 인데 2, 3차 분리에서는 도메인도 분리되기 때문에 설정 변경 필요

#### Default.Conf 설정 변경
- Vite가 바라보는 nginx의 설정을 구성
- upstream app의 Back End Service Name, Port 설정 변경 필요

#### Docker-compose.yml
- networks는 docker 이미지들이 속해있는 기본 네트워크를 지정할 수 있다.
- services 설정은 현재 프로젝트의 서비스 이름과 포트를 지정할 수 있다.
	 서비스 이름에 이미지와 포트 번호를 지정하면 해당 서비스에 대한 정의를 할 수 있다.

#### Dockerfile
- 현재 프로젝트의 Dockerfile 설정은 nginx image 설치에 대한 설정만 구성.
- 추가로 디렉토리 트리 내부의 Dockerfile-node 명령어를 통해 node:16.19.alpine 이미지가 도커 내부에 설치되어 있는 상태

#### gitlab-ci.yml
- tagname, build에 사용 할 image 선언
- build 된 image에 생성 될 label 정보 변경
- CICD가 일어날 rules 수정