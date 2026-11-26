repository-root/ (Turborepo를 설치한 예시 저장소 루트)
- turbo.json 
	- build: **"dependsOn": ["^build"]**, "outputs": ["dist/**"]
	
		- **apps/web-app/**
			- package.json    **<-- root 에서 참조**
				- dev: "vite --host",
				- build : "run-p type-check \"build-only {@}\" --" 
				- ....

		- **apps/admin-app/**
			- package.json    **<-- root 에서 참조**
				- dev: "vite --host",
				- build : "run-p type-check \"build-only {@}\" --"
				- ...




### Turborepo - CI 연계
- CI 파이프라인 안에서 Turbo 가 영향받은 workspace의 task 만 골라서 실행한다.
- build / lint / test 등 명령어를 실행하면 Git 변경 내역과 workspace 의존성 그래프를 기준으로 영향받은 패키지만 대상으로 잡는다.
