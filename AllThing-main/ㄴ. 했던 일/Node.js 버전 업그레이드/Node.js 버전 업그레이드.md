- 로컬에서 개발용 프로젝트 패키지를 띄울 때, Node.js는 기본적으로 로컬에 설치된 Node.js 버전을 참조합니다. 즉, 다음 사항들이 적용됩니다:
	1. **로컬 Node.js 버전**:
	    - 프로젝트를 실행할 때 사용되는 Node.js는 시스템에 설치된 버전입니다. `node -v` 명령어로 확인할 수 있습니다.
	1. **`nvm` 사용 시**:
        - 만약 `nvm` (Node Version Manager)을 사용하고 있다면, 프로젝트에 설정된 Node.js 버전을 `.nvmrc` 파일로 관리할 수 있습니다. 이 경우 `nvm use` 명령어로 해당 버전으로 전환한 후 프로젝트를 실행해야 합니다.
	1. **`package.json`의 `engines` 필드**:
        - `package.json`의 `engines` 필드는 권장 Node.js 버전을 명시하지만, 실제로 참조하는 버전은 로컬에 설치된 버전입니다.
	1. **Node.js 설치 경로**:
        - Node.js가 설치된 위치는 운영체제에 따라 다르며, 일반적으로 `/usr/local/bin/node` (macOS/Linux) 또는 `C:\Program Files\nodejs\node.exe` (Windows)와 같은 경로에 있습니다.
	따라서 로컬에서 개발할 때는 로컬에 설치된 Node.js 버전이 참조되며, 다른 곳을 참조하지 않습니다.

- ㅁㅁㅁㅁ



