[참고 - Nginx 기본 설정](https://medium.com/@jina-dev/nginx-%EA%B8%B0%EB%B3%B8%EC%84%A4%EC%A0%95-fa06e7ef612d)
[참고 - React에서 다수 프로젝트 배포 방식](https://velog.io/@briiiiiiiick/React-%EC%97%AC%EB%9F%AC-%ED%94%84%EB%A1%9C%EC%A0%9D%ED%8A%B8%EB%A5%BC-Nginx-%EC%9B%B9-%EC%84%9C%EB%B2%84%EC%97%90-%EB%B0%B0%ED%8F%AC%ED%95%98%EA%B8%B0)
[참고 - Nginx Routing](https://velog.io/@pinot/nginx-%ED%8F%AC%ED%8A%B8%ED%8F%AC%EC%9B%8C%EB%94%A9%EC%97%90-%EB%8C%80%ED%95%98%EC%97%AC-%EC%95%8C%EC%95%84%EB%B3%B4%EC%9E%90)

#### 1. 메인 프로젝트의 Nginx를 사용해서 빌드
- A(Main) 프로젝트의 Nginx를 사용해서 이미 Static 파일이 생성되어 있는 B 프로젝트와 연결해서 하나의 서버에 서로 다른 여러 프로젝트를 띄우고, Path로 이동할 수 있도록 설정

#### 2. 프로젝트와 Nginx 분리해서 빌드
- A(Main), B 각각의 Static 파일을 생성한 후, 별개의 Nginx에서 두 프로젝트를 하나로 통합


#### 3. 위 두 방법의 경우, 방식만 조금 다를 뿐 설정은 비슷함
- 프로젝트 별 Docker Container 내부에 생성된 정적 파일 디렉토리를 Host 디렉토리와 Volume 연결해서 주 Nginx가 참조할 수 있도록 설정하는 것이 중요하다.


#### 4. 설정 과정
##### **- 프로젝트 별 설정**
##### 1) Vite.config 설정
![[Pasted image 20240723104729.png]]
- **base url 설정** ( local 에서도 현재 프로젝트의 url을 구분할 수 있게 해준다 )
	**이 속성은 Vite 애플리케이션이 배포될 때의 기본 URL 경로를 설정하는데 사용된다. (GPT)**
	1. **정적 자원 경로 설정** : Vite는 모든 정적 자원을 base 설정이 되어있는 곳을 기준으로 요청한다.
	2. **배포 환경에 따른 경로 조정** : 애플리케이션이 root(/)가 아닌 하위 경로에서 제공될 때 유용하다. 같은 호스팅 서비스 내에서 여러 경로를 가진 프로젝트를 배포할 경우 base를 하위 경로로 설정해야 한다. 이렇게 하면 사용자가 애플리케이션에 접근할 때 올바른 URL 리소스를 로드할 수 있다.
	3. **라우팅 연계** : base 설정은 라우팅 경로와 함께 작동해 내비게이션 링크가 올바른 경로로 이동할 수 있도록 도와준다.
	- 만약 base가 /aaa/로 설정되어 있다면, Vite는 모든 정적 자원을 /aaa/style.css, /aaa/main.js 등과 같이 요청한다.
	- 따라서, 배포할 서버의 구조에 맞게 base값을 설정하는 것이 중요하며, 기본적으로는 /로 설정되어 있지만, 하위 경로에서 애플리케이션을 실행할 경우 반드시 경로를 반영해야 한다.

- **Package.json의 hompage 설정** : Package.json 내부 에서도 homepage라는 base와 동일한 설정이 있지만, 이는 정적 자원의 경로 설정에 중점을 두며, 주로 React 환경에서 주로 사용한다. 
  Vite의 base 설정은 라우팅과 정적 자원 요청, 빌드 프로세스와 관련된 설정이므로, Vite를 사용하는 경우, base 설정을 쓰는 것이 권장된다.


![[Pasted image 20240723104428.png]]
- Build 후 생성될 Static 파일들을 프로젝트 별로 구분하기 위해 정적 파일이 생성될 디렉토리를 지정해준다.

##### 2) Nginx Dockerfile
![[Pasted image 20240723112043.png]]
- vite.config 에서 생성한 dist 하위의 정적파일 디렉토리를 컨테이너 내부에 복사해준다.

##### 3) docker-compose.yml
![[Pasted image 20240723112328.png]]
- Docker Host 시스템 디렉토리와 컨테이너 내부의 디렉토리를 연결하는 Volume 설정 추가 필요
- **Host 시스템 디렉토리:컨테이너 디렉토리** 연결 Volume 설정
	- 이렇게 연결된 디렉토리들은 서로를 참조하기 때문에 어느 한 쪽이 바뀌면 다른 곳에도 바로 반영이 된다.
	- 여기서 연결되는 volume은 Host 시스템에 있는 정적 파일들을 컨테이너 내부에서도 참조할 수 있도록 하기 위함이다. 

##### 4) gitlab-ci.yml
![[Pasted image 20240723112601.png]]
- **gitlab-ci.yml의 script 설정**
	- 패키지를 빌드할 때 생성된 /dist/프로젝트 정적 파일 디렉토리를 gitlab-runner 내부의 /data/ 하위 디렉토리에 복사하는 과정이 추가되어야 한다.
	- gitlab-ci.yml의 script 과정을 통한 cicd는 gitlab-runner 내부에서 실행이 되기 때문에 script에서 실행된 build의 결과물도 gitlab-runner 컨테이너 내부에 존재하게 된다.
	- gitlab-ci.yml의 script 과정에서 생성된 정적 파일들을 runner 외 다른 컨테이너도 참조할 수 있도록 runner와 host 시스템의 디렉토리 또한 volume 설정을 통해 연결해주어야 한다.

##### 5) gitlab-runner container
- **docker exec -it {컨테이너 id} bash** 
![[Pasted image 20240723113513.png]]
- gitlab-runner 컨테이너 내부의 config.toml 파일에서 volume 설정 필요
![[Pasted image 20240723113820.png]]
- Host 시스템의 data 디렉토리 전체와 gitlab-runner 실행시 생성되는 **임시 컨테이너**의 data 디렉토리 전체를 volume으로 설정
- gitlab cicd 과정의 script/build 과정에서 생성된 정적파일들을 다른 컨테이너가 읽을 수 있도록 Host 시스템의 디렉토리에도 반영
	- 여기서 잡은 volume은 gitlab-runner cicd 가 작동할 때 임시로 생성됐다 사라지는 임시 runner 컨테이너에 있는 /data 디렉토리를 볼륨으로 잡는다
- #####

##### **- Nginx 설정**
##### 1) nginx default.conf 설정
![[Pasted image 20240723181501.png]]
- upstream 설정을 이용해 요청을 전달할 서버를 명시, 현재 하나의 nginx에서 여러 클라이언트를 띄워야 하기 때문에 그에 대응하는 서버도 같이 명시해두어야 한다.
- 기본 80 포트로 설정

![[Pasted image 20240723181913.png]]
- location 설정을 사용해 nginx가 참조할 정적 파일들의 위치와 파일들을 명시해 어떤 url에서 어떤 정적 파일 디렉토리와 파일을 참조할 것인지를 설정한다.

##### 2) docker-compose.yml 설정
![[Pasted image 20240723182137.png]]
-  gitlab-runner cicd의 script 내부에서 build 후 생성된 정적 파일들이 보관되어있는 gitlab-runner 임시 컨테이너 내부와 volume으로 잡혀있던 host 시스템의 디렉토리를 nginx 컨테이너 내부의 home/dist 디렉토리와 한번 더 volume으로 잡아서 nginx 컨테이너 내부에서도 각 프로젝트 별로 빌드된 결과물 static 파일들을 참조할 수 있도록 한다.
- /data/nginx/html(host) : /home/dist(nginx)


### 이후 수정된 사항들
#### 1) 프로젝트를 새로운 서버로 이전하는 과정에서 Docker 환경의 HOST와 실행 컨테이너, front-end build 결과물 컨테이너의 Volume 설정에 문제 발생
- 문제 사항 
1) node build를 통해 front-end 패키지를 빌드한 이후 생성되는 artifacts를 저장한 후, docker-compose.yml에 설정해 놓은 HOST Volume 디렉토리에 생성된 artifacts가 반영되지 않음
2) HOST 뿐만 아니라 생성된 front-end 컨테이너의 지정된 디렉토리에도 artifacts가 생성되어 있지 않음을 발견
3) docker-compose.yml의 Volume 설정을 없애면 front-end 컨테이너에는 빌드 결과물 artifacts가 정상적으로 생성되어 있었음
4) 컨테이너는 정상적으로 올라가지만, 화면은 뜨지 않는 현상이 계속 발생

#### 2) 해결
- HOST의 Runner 설정에서 Volume을 잡아줌
- ![[Pasted image 20240830162046.png]]
- ![[Pasted image 20240830162305.png]]
- 위 경로의 config.toml 파일에서 Volume 영역의 /data:/data 를 지정
- 설정의 앞 부분이 HOST, 뒷 부분이 Runner 실행 임시 컨테이너이다. 
- 즉, HOST:RUNNER Container
- Runner의 임시 실행 컨테이너 내부에서 생성되는 artifacts 디렉토리를 HOST의 디렉토리와 연결시키고, 노드 빌드 결과물인 artifacts를 통합 nginx 프로젝트인 integrate에서 바라볼 수 있도록 해줘야 했다.
- 기존에 설정해줬던 부분이지만, 이 설정을 놓쳤기 때문에 HOST와 Runner 임시 실행 컨테이너를 서로 연결할 수 없었기 때문에 각 artifacts가 계속 없어졌던 것이다
- ![[Pasted image 20240830163000.png]]
- Runner Volume 설정 후 생성된 artifacts가 정상적으로 HOST에도 저장된다.

#### 3) 이 후 변경사항
- 해당 설정 이후, gitlabci.yml의 script에서 실행되는 node build와 그 결과물은 Runner 임시 실행 컨테이너에서 생성된 후 해당 컨테이너에만 저장되는 것이기 때문에, front-end 컨테이너는 build할 필요가 없어졌으며, 
- 여러 프로젝트를 통합하는 nginx 컨테이너만 하나 생성되면 여러 프로젝트를 하나로 통합해서 올릴 수 있게 되었기 때문에 integrate 프로젝트의 cicd 관련 설정은 이전과 동일하게 가지만,
- integrate nginx 컨테이너로 인해 실행되는 프로젝트들은 cicd 설정 관련해서 변경 사항이 있다.
- ![[Pasted image 20240830163624.png]]
- 첫 번째로, 컨테이너가 필요하지 않기 때문에, docker build 과정이 없어지고,
- 두 번째로, Runner 설정에서만 Volume을 잡아주면 되고, 컨테이너가 올라가지 않기 때문에 docker-compose.yml 설정 또한 전체가 사용되지 않는다. 
- 마지막으로, HOST와 Runner 임시 실행 컨테이너 사이 Volume을 설정하는 단계에서 HOST내부에 Volume으로 설정한 디렉토리가 존재하지 않는다면 오류가 발생하기 때문에 cicd 실행 전에 해당하는 디렉토리를 직접 생성해놔야 정상적 cicd 과정이 진행된다.



### 추가 사항 ( 25.02.27 ~ )
#### 1. 새로운 서버에 프로젝트 초기 구성 중, cicd가 수행되어도 추가 사항이 반영되지 않는 문제 발생
- nginx 컨테이너 내부와 호스트 os 파일에 생성된 build 결과물 assets 에 index.html 이 참조하려는 .js 파일들이 여러개 생성되어 있음
  캐싱된 ~.js로 인해 이전 버전의 assets을 참조하는 것 같음 ( 추정 )

	해결 방안
	- 프로덕트 빌드 설정, 메뉴, 메뉴 mock server 관련 표시되지 않는 문제였음
	- nginx assets 관련 문제는 아니었음


### 진행하며 알게 된 새로운 사실
#### 1) Docker Host-Container 사이 Volume / Mount