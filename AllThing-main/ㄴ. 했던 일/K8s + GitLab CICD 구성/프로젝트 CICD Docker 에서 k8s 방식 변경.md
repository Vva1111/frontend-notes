## 기존 방식
### SPA(프론트 엔드 화면) 프로젝트
1. Jade, Davis 와 같이 SPA 프로젝트는 CICD 과정에서 패키지 build, artifact로 저장
	![[Pasted image 20250520110103.png]]
2. 생성된 build 결과물을 러너 임시 컨테이너 내부의 특정 디렉토리로 옮김 ( cp )
	![[Pasted image 20250520110113.png]]
3. 기존에 gitlab-runner 컨테이너 내부 config.toml 설정에 2의 빌드 결과물을 host 로 가지고 올 수 있는 볼륨 설정 ( /data:/data )
	![[Pasted image 20250520110604.png]]
4. 이 과정에서 생성된 Build 결과물들이 host 의 특정 디렉토리에 위치할 수 있게 됨
	![[Pasted image 20250520110907.png]] - Host 디렉토리
	![[Pasted image 20250520110113.png]] - gitlab-runner 임시 컨테이너 내부 디렉토리 생성
	위 두 디렉토리를 볼륨 연결

### Integrate ( 프론트 엔드 프로젝트 - Nginx 통합 ) 프로젝트
1. Dockerfile 을 사용한 Nginx 이미지 빌드
2. 빌드 된 이미지 저장소에 push
3. docker-compose 사용해서 컨테이너 생성, 실행
	1. docker-compose 에서 컨테이너를 생성, 실행하는 과정에서
	2. host의 디렉토리에 존재하는 SPA build 결과물 파일을 웹서버 컨테이너에서 읽어들일 수 있도록 볼륨을 설정
	![[Pasted image 20250520111335.png]] 
	- Host(/data/~) 와 컨테이너 (/home/~) 볼륨 연결
4. 위 과정을 통해, 컨테이너 내부에 SPA 빌드 결과물인 정적 파일들이 위치하게 되고, 화면을 제공할 수 있게 된다.

## 변경 사항
1. 기존 Docker 데몬을 사용한 환경을 사업자 제공을 위해 k8s 환경으로 재구성 필요
2. k8s 환경으로 바뀌게 되면, 기존 방법에서 사용했던 Docker 관련 구성 사항을 모두 사용할 수 없게 됨

## 문제 사항
1. nginx 오류 : index.html 을 찾지 못해 반복적인 redirect 발생
2. 기존 Docker 컨테이너 기반 환경에서는 정상적으로 잘 뜨던 화면이 저장소에 push 된 이미지를 pull 받아서 k8s 환경 구성으로는 화면이 뜨지 않음

## 원인
1. Integrate 에서 수행하는 빌드 -> 이미지 생성 -> Push 과정에서 생성되는 이미지는 저장소에 push 될 때 까지도 각 SPA 프로젝트의 CICD 파이프라인 내에서 생성된 빌드 결과물들을 가지고 있지 않음
2. 저장소에 push 된 이미지는 아직 SPA build 결과물을 들고 있지 않은 상태임
3. SPA build 결과물을 들고 있지 않은 이미지를 k8s 환경에 사용하니 nginx에서는 index.html 을 찾지 못하고 오류와 함께 반복적인 redirection 수행을 하게 됨
4. docker-compose 를 사용해서 최종적으로 빌드 결과물을 컨테이너가 가지고 있게 하는 구성인 기존 방법으로는 k8s 환경에서 화면을 띄울 수 없게 됨

## 해결
1. 빌드한 이미지를 저장소에 push 하기 전에 이미지 안에 SPA 빌드 결과물이 포함되도록 구성 변경 필요
2. 기존 방법에서는 저장소에 이미지를 push 하는 시점에서는  SPA 빌드 후 결과물을 생성하는 CICD 파이프라인 실행 gitlab-runner 임시 컨테이너와 host 사이의 볼륨 연결만 되어 있음
3. 1번 방식으로 구성하기 위해
	1. docker-compose 수행 시점 변경 : docker-compose 는 컨테이너 생성, 실행에 쓰이는 도구이기 때문에 이미지 빌드와 저장소 push 에는 영향을 주지 못함
	2. Dockerfile에서 디렉토리 Copy 후 빌드, 이미지 생성 : SPA 빌드 결과물 파일을 찾을 수 있는 방법이 없어서 빌드 결과물 파일을 가져올 수 없음
	3. CICD 파이프라인 내에서 curl을 통해 특정 프로젝트의 artifatc 파일을 get 해오는 방식으로 구성
4. 3-3 방식으로 특정 프로젝트의 특정 잡을 지정하고, 발급된 그룹 토큰을 설정해 현재 CICD 파이프라인 내로 SPA 빌드 파일을 불러옴
	![[Pasted image 20250520112224.png]] - 그룹 토큰 발급
	![[Pasted image 20250520112121.png]] - 발급 받은 그룹 토큰을 (그룹)CI 환경 변수에 저장
	![[Pasted image 20250520112452.png]] - artifact 파일을 불러올 curl 패키지 설치
	![[Pasted image 20250520112631.png]] - curl 을 통한 GET artifact API 실행 ( 각 프로젝트 ID와 artifact 생성하는 Job 이름 필요 )
	![[Pasted image 20250520113026.png]] - 특정 디렉토리에 압축 해제
5. Dockerfile 내부에 불러온 SPA 빌드 결과물 파일을 이미지 빌드시 COPY 해서 이미지 내부에 저장하는 명령어 추가
	![[Pasted image 20250520112759.png]]
6. 이후 해당 이미지를 사용해 직접 컨테이너 생성 및 실행 했을 때, 해당 컨테이너 내부에 SPA 빌드 결과 파일 포함되어 있는 것을 확인
7. 해당 이미지를 사용한 k8s pod 구성 후 해당 pod 확인 결과 pod 에도 동일하게 SPA 빌드 결과물이 포함되어 있는 것을 확인
8. 컨테이너, pod 방식으로 띄워진 화면 모두 확인

## 추가
- docker 데몬을 사용하지 않는 방식이기 때문에 이미지 빌드, push 또한 docker 를 사용하지 않아야 한다.
	docker 데몬을 사용하지 않고 직접 빌드 및 push 를 수행할 수 있는 이미지인 kaniko-node 사용

- kaniko-node 이미지 사용해서 직접 빌드, push 수행 시, docker-compose 도 사용할 수 없음
	docker-compose 사용하지 않는다면, 컨테이너 생성 및 실행 방식을 변경하거나, 컨테이너 방식은 사용하지 않을 수 있다(미정)

- kaniko-node 이미지를 사용 한 빌드, 저장소 push 이후 K8s 저장소 네임스페이스의 pod restart 를 수행하는 명령어가 필요한데, 현재(25-05-20) 해당 네임스페이스에 명령 수행 권한이 없음
	해당 명령 수행 권한은 K8s 설정 파일인 role.yaml, rolebinding.yaml 을 사용해서 설정해줘야 한다.
