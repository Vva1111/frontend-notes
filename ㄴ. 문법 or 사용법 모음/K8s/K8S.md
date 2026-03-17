1. 초기 구성
	 - winget 을 사용한 kubectl 설치
		 - **winget install -e --id Kubernetes.kubectl**
	- home 디렉토리 내부에 /.kube 디렉토리 생성
	- /.kube 에 config(확장자 없이 또는 확장자 환경변수 지정 후) 파일 생성
	- kubectl ns 로 namespace 목록 확인
	- namespace 목록 떠야 정상 적용



#### 프로젝트 패키지 실제 설치 방식
pod(jade-web.yaml), sevice(jade-web-service.yaml) 파일을 
고객사에 가져가서
shell 명령을 통해 ( 해당 파일 실행하고 적용할 수 있는 명령어, 그에 필요한 사전 명령어 순서대로 )
pod, service 를 k8s 서버에 설치
1. 정상 설치 확인
2. nodePort 접근 확인, 화면 정상적으로 뜨는지 확인

### alias 설정
- window 의 경우, **$PROFILE** 내부에 alias 설정을 통해 alias 를 사용할 수 있는데,
- notepad $PROFILE 로 해당 파일에 설정이 적용되어 있는지 확인할 수 있다.
- 만약 해당 파일이 없다면,
	- New-Item -ItemType File -Path $PROFILE -Force
- 중간 파일이 없을 수도 있기 때문에 디렉토리도 먼저 만들어주는게 더 안전할 수 있다.
- 만약, 이 시스템에서 스크립트를 실행할 수 없으므로 ...ps1 파일을 로드할 수 없습니다. 에러가 발생하는 경우,
	- 파워쉘의 실행 정책 때문에 스크립트가 실행되지 않는 것이며
	- Set-ExecutionPolicy RemoteSigned -Scope CurrentUser
	- 을 통해 현재 접속자에 대해서만 실행 정책을 완화하는 명령을 사용할 수 있다.
		- - `RemoteSigned`: 로컬에서 만든 스크립트는 실행 가능, 인터넷에서 받은 건 서명 필요
		- `-Scope CurrentUser`: 현재 사용자에게만 적용되므로 시스템 전체에 영향 없음
- 모든 설정이 완료 되어 $PROFILE 을 열어볼 수 있다면,
	- 해당 파일 내에
		- function k { kubectl $args }
		- Set-Alias kgp 'kubectl get pods'
		- Set-Alias kaf 'kubectl apply -f'
	- 위와 같은 명령을 저장해 alias 설정을 추가할 수 있다.

### Helm
- **그룹 러너 enable 버튼 눌러서 enable 상태로 해놔야 함**
- K8s 패키지 매니저 ( K8s 앱 설치, 관리 도구 )
- ex) 리눅스 apt, yum, brew 등과 비슷
- K8s 리소스를 직접 만들려면 .yaml 파일을 여러 개(deploy, service, configmap 등등..) 작성해야 하지만, 이들을 한 번에 템플릿화 하고 재사용 가능하게 만드는 역할
- 핵심 개념
	- **Chart** : Helm 패키지, K8s 리소스 템플릿들이 담겨 있는 폴더 구조
	- **Release** : 특정 Chart를 클러스에 설치한 실행 인스턴스
	- **Repository** : 여러 Chart가 저장된 저장소 ( 예: `https://charts.gitlab.io/` )
	- **Values**(.yaml) : Chart 템플릿에 주입하는 변수 설정 파일
- 명령어
	- **helm repo add docker-repository 주소**
		- docker-repository 라는 이름으로 특정 주소에 있는 애플리케이션 Chart가 들어있는  Helm Chart 저장소를 추가한다.
	- **helm install nginx bitnami/nginx**
		- bitnami/nginx Chart를 설치하고, 이 설치 인스턴스의 이름을 nginx 로 설정한다.
		- 위 명령 실행 시, K8s 에 Nginx deploy, service 등이 자동으로 생성된다.

- #### values.yaml
	- **options**
		- **replicaCount** : 생성할 파드 수 ( deployment의 replicas )
		- **image.repositoy** : 사용할 도커 이미지의 이름
		- **image.tag** : 사용할 이미지 버전
		- **image.pullPolicy** : 이미지를 가져올 정책 ( Always, IfNotPresent, Never )
		- **service.type** : K8s service 타입 ( ClusterIP, NodePort, LoadBalancer )
		- **ingress.port** : 외부에 노출할 포트
		- **ingress.enable** : Ingress 리소스를 만들지 여부
		- **ingress.className** : 사용항 Ingress 클래스 이름 ( nginx. alb, trafik 등)
		- **ingress.host** : 도메인과 경로 설정
		- **resources.limits/requests** : 컨테이너의 CPU/메모리 제한 및 요청 설정
		- **env** : 파드에 주입할 환경 변수 목록

### docker registry용 secret 생성 kubectl 명령어
복사
```
#kubectl create secret docker-registry <시크릿이름> \
	# --docker-server=<레지스트리주소> \
	# --docker-username=<사용자명> \
	# --docker-password=<비밀번호나토큰> \ 
	# -n <네임스페이스> 
	
kubectl create secret docker-registry bmp-repo-secret \
	--docker-server=harbor1.dev.spinel-cloud.com \
	--docker-username=user-spinel \
	--docker-password=0701dPtm \
	-n gitlab-runner-fe
	
# 생성 확인 
	kubectl get secret -n gitlab-runner-fe
```

### helm 설정 install
1. **Gitlab Helm 저장소 추가**
```
helm repo add gitlab https://charts.gitlab.io
```

2. **values.yaml 수정**
	- values.yaml은 템플릿 파일을 직접 수정하는 것이 아니라, helm이 템플릿을 렌더링할 때 값을 주입하는 방식이다.
	- 수정한 values.yaml 은
		- ./template/ 내부의 템플릿 파일들이 {{ .Values.XXX }} 처럼 정의한 부분에 values.yaml 에 작성한 값들이 자동으로 렌더링 시점에 치환되어 적용된다.

3. **설치**
- install 명령어 실행 시, 네임스페이스 항상 명시가 권장된다.
```
# values.yaml 파일을 사용한 설치
helm install -n <namespace> --create-namespace <runner name> -f values.yaml gitlab/gitlab-runner
```
	or
```
# values.yaml 파일을 사용하지 않는 최소한 설정 설치
helm install gitlab-runner gitlab/gitlab-runner \
  --namespace gitlab-runner \
  --create-namespace \
  --set gitlabUrl=https://gitlab.com/ \
  --set runnerRegistrationToken=<YOUR_REGISTRATION_TOKEN> \
  --set rbac.create=true \
  --set runners.executor=kubernetes

```


* 네임 스페이스 지정하지 않을 시,
```
kubectl config view --minify
```
로 확인되는 기본 네임스페이스에 설치된다.