##### [Vite를 사용해 빌드할 때, 빌드 결과 파일들을 분류하는 방법](https://light9639.tistory.com/entry/Viteconfigts-%EC%84%A4%EC%A0%95)

### Vite.config.js 
##### (https://ko.vitejs.dev/config/)

- 터미널에서 Vite를 실행시킬 때, Vite는 자동으로 프로젝트 루트의 vite.config.js 파일 확인을 시도한다.
- Vite는 프로젝트가 네이티브 Node ESM을 사용하지 않는 경우에도 설정 파일에서 ES 모듈 구문을 사용할 수 있도록 지원한다. ( ex. package.json의 type:"module" )
- --config CLI 옵션을 사용해 명시적으로 특정 설정 파일을 지정할 수 있다.
	```
	vite --config my-config.js
	```

	**공용 옵션**
	- **root** : 프로젝트 루트 디렉토리를 설정 ( 절대 / 상대 경로 가능 )
	- **base** : 개발 또는 프로덕션 모드에서 사용되는 Public Base Path
		( 절대 / 전체 경로명 가능, 빈 문자열 또는 ./ 가능 )
		- 만약 배포하고자 하는 디렉터리가 루트 디렉터리가 아니라면, base 설정을 이용해 프로젝트의 루트가 될 디렉토리를 명시해 줄 수 있다.
		- JS ( import ), CSS ( url() ), .html 파일에서 참조되는 에셋 파일의 URL들은 빌드 시 이 Base Path를 기준으로 가져올 수 있도록 자동으로 맞춰지게 된다.
		- 만약 동적으로 에셋의 URL을 생성해야 하는 경우라면, import.meta.env.BASE_URL을 이용해야 한다. 이 상수는 빌드 시 Public Base Path로 변환되어 이를 이용해 동적으로 가져오려는 에셋에 대한 URL을 생성할 수 있다. 다만, 정확히 import.meta.env.BASE_URL과 동일한 문자열에 대해서만 지환하는 방식이다.
	- **mode** : 설정에서 mode를 지정하면 serve와 build 모두에서 기본 모드를 오버라이드 한다.
	- **define** : 전역 상수로 대체되는 값을 정의한다. 정의된 내용들은 개발 중에는 전역으로 정의되나, 빌드 중에는 정적으로 대체된다.
	- **plugins** : 사용할 플러그인의 배열. 
	- **publicDir** : 정적 에셋들을 제공하는 디렉토리. 이 디렉토리의 파일들은 개발 중에는 / 에서 제공되고, 빌드 시에는 outDir의 루트로 복사되며, 변형 없이 언제나 있는 그대로 제공되거나 복사된다. ( 절대 / 상대 경로 가능 )
	- **cacheDir** : 캐시 파일을 저장할 디렉토리. 이 디렉토리의 파일들은 미리 번들된 의존 파일이거나 Vite에 의해 생성된 어떤 다른 캐시 파일로, 성능을 향상시킬 수 있다.