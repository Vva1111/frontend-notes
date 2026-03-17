#### npm run preview(배포 환경) 디버깅 시,
- **vite.config 내부에**
	export default {
	  build: {
	    sourcemap: true // 꼭 필요!
	  }
	}
	설정 필요
- **launch.json 내부에는**
	{
	  "type": "chrome",
	  "request": "launch",
	  "name": "Launch Chrome against localhost",
	  "url": "http://localhost:4173",
	  "webRoot": "${workspaceFolder}/dist",
	  "sourceMaps": true,
	  "skipFiles": []
	}
	설정 필요 ( url, webRoot, sourceMaps 설정이 개발 환경과 다름 )