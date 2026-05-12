> [!info] 완벽한 IT 인프라 구축을 위한 Docker.pdf  
>  
> [https://drive.google.com/file/d/1YzcO9f01f0KlZQatXSggonSTby9HnV91/view?usp=drivesdk](https://drive.google.com/file/d/1YzcO9f01f0KlZQatXSggonSTby9HnV91/view?usp=drivesdk)  

# 제1장 시스템과 인프라 기초 지식 (37p)

## 1.1 시스템 기반의 기초 지식 (10p)

- 시스템 기반의 구성 요소

- 클라우드와 온프레미스

- 클라우드가 적합한 케이스

- 온프레미스가 적합한 케이스

- 시스템 기반의 구축/운용 흐름

## 1.2 하드웨어와 네트워크 기초 지식 (5p)

- 서버 장비

- 네트워크 주소

- OSI 참조 모델과 통신 프로토콜

- 방화벽

- 라우터/레이어 3 스위치

## 1.3 OS(Linux) 기초 지식 (9p)

- Linux

- Linux 커널

- Linux 파일 시스템

- Linux 디렉토리 구성

- Linux 보안 기능

## 1.4 미들웨어 기초 지식 (4p)

- 웹 서버/웹 애플리케이션 서버

- 데이터베이스 서버

- 시스템 감시 툴

## 1.5 인프라 구성 관리 기초 지식 (9p)

- 인프라 구성 관리

- 코드를 사용한 구성 관리

- 대표적인 인프라 구성 관리 툴

- 지속적 인티그레이션/지속적 딜리버리

  

---

# 제2장 컨테이너 기술과 Docker의 개요 (27p)

## 2.1 컨테이너 기술의 개요 (6p)

- 컨테이너

- 컨테이너 역사

## 2.2 Docker의 개요 (4p)

- 프로그래머에게 Docker란?

## 2.3 Docker의 기능 (9p)

- Docker 이미지를 만드는 기능(Build)

- Docker 이미지를 공유하는 기능(Ship)

- Docker 컨테이너를 작동시키는 기능(Run)

- Docker 에디션

- Docker 컴포넌트

## 2.4 Docker의 작동 구조 (8p)

- 컨테이너를 구획화하는 장치(namespace)

- 릴리스 관리 장치(cgroups)

- 네트워크 구성(가상 브리지/가상 NIC)

- Docker 이미지의 데이터 관리 장치

---

# 제3장 Docker 설치와 튜토리얼 (23p)

## 3.1 Docker 설치와 작동 확인 (18p)

- Docker의 클라이언트 툴

- Docker for Mac 설치하기

- Docker for Windows 설치하기

- Linux에 설치하기

- Docker에서 ‘Hello world’

- Docker 버전 확인(docker version)

- Docker 실행 환경 확인(docker system info)

- Docker 디스크 이용 상황(docker system df)

## 3.2 웹 서버를 작동시켜 보자 (5p)

- Docker 이미지 다운로드하기

- Nginx를 작동시켜 보자

- Nginx 작동 확인

- Nginx 기동 및 정지

  

---

# 제4장 Docker 명령 (57p)

## 4.1 Docker 이미지 조작 (15p)

- Docker Hub

- 이미지 다운로드(docker image pull)

- 이미지 목록 표시(docker image ls)

- 이미지 상세 정보 확인(docker image inspect)

- 이미지 태그 설정(docker image tag)

- 이미지 검색(docker search)

- 이미지 삭제(docker image rm)

- Docker Hub에 로그인(docker login)

- 이미지 업로드(docker image push)

- Docker Hub에서 로그아웃(docker logout)

## 4.2 Docker 컨테이너 생성/시작/정지 (21p)

- Docker 컨테이너의 라이프 사이클

- 컨테이너 생성 및 시작(docker container run)

- 컨테이너의 백그라운드 실행(docker container run)

- 컨테이너의 네트워크 설정(docker container run)

- 자원을 지정하여 컨테이너 생성 및 실행(docker container run)

- 컨테이너를 생성 및 시작하는 환경을 지정(docker container run)

- 가동 컨테이너 목록 표시(docker container ls)

- 컨테이너 가동 확인(docker container stats)

- 컨테이너 시작(docker container start)

- 컨테이너 정지(docker container stop)

- 컨테이너 재시작(docker container restart)

- 컨테이너 삭제(docker container rm)

- 컨테이너 중단/재개(docker container pause/docker container unpause)

## 4.3 Docker 컨테이너 네트워크 (8p)

- 네트워크 목록 표시(docker network ls)

- 네트워크 작성(docker network create)

- 네트워크 연결(docker network connect/docker network disconnect)

- 네트워크 상세 정보 확인(docker network inspect)

- 네트워크 삭제(docker network rm)

## 4.4 가동 중인 Docker 컨테이너 조작 (6p)

- 가동 컨테이너 연결(docker container attach)

- 가동 컨테이너에서 프로세스 실행(docker container exec)

- 가동 컨테이너의 프로세스 확인(docker container top)

- 가동 컨테이너의 포트 전송 확인(docker container port)

- 컨테이너 이름 변경(docker container rename)

- 컨테이너 안의 파일을 복사(docker container cp)

- 컨테이너 조작의 차분 확인(docker container diff)

## 4.5 Docker 이미지 생성 (7p)

- 컨테이너로부터 이미지 작성(docker container commit)

- 컨테이너를 tar 파일로 출력(docker container export)

- tat 파일로부터 이미지 작성(docker image import)

- 이미지 저장(docker image save)

- 이미지 읽어 들이기(docker image load)

- 불필요한 이미지/컨테이너를 일괄 삭제(docker system prune)

  

---

# 제5장 Dockerfile을 사용한 코드에 의한 서버 구축 (43p)

## 5.1 Dockerfile을 사용한 구성 관리 (5p)

- Dockerfile이란?

- Dockerfile의 기본 구문

- Dockerfile 작성

## 5.2 Dockerfile의 빌드와 이미지 레이어 (7p)

- Dockerfile로부터 Docker 이미지 만들기

- Docker 이미지의 레이어 구조

## 5.3 멀티스테이지 빌드를 사용한 애플리케이션 개발 (4p)

- Dockerfile 만들기

- Docker 이미지의 빌드

- Docker 컨테이너의 시작

## 5.4 명령 및 데몬 실행 (15p)

- 명령 실행(RUN 실행)

- 데몬 실행(CMD 명령)

- 데몬 실행(ENTRYPOINT 명령)

- 빌드 완료 후에 실행되는 명령(ONBUILD 명령)

- 시스템 콜 시그널의 설정(STOPSIGNAL 명령)

- 컨테이너의 헬스 체크 명령(HEALTHCHECK 명령)

## 5.5 환경 및 네트워크 설정 (6p)

- 환경변수 설정(ENV 명령)

- 작업 디렉토리 지정(WORKDIR 명령)

- 사용자 지정(USER 명령)

- 라벨 지정(LABEL 명령)

- 포트 설정(EXPOSE 명령)

- Dockerfile 내 변수의 설정(ARG 명령)

- 기본 쉘 설정(SHELL 명령)

## 5.6 파일 설정 (6p)

- 파일 및 디렉토리 추가(ADD 명령)

- 파일 복사(COPY 명령)

- 볼룸 마운트(VOLUME 명령)

  

---

# 제6장 Docker 이미지 공개 (22p)

## 6.1 Docker 이미지의 자동 생성 및 공개 (9p)

### Automated Build의 흐름

### GitHub에 공개하기

### Docker Hub의 링크 설정

### Dockerfile의 빌드

### Docker 이미지 확인

## 6.2 Docker Registry를 사용한 프라이빗 레지스트리 구축 (6p)

### 로컬 환경에 Docker 레지스트리 구축하기

### Docker 이미지 업로드

### Docker 이미지의 다운로드와 작동 확인

## 6.3 클라우드 서비스를 사용한 프라이빗 레지스트리 구축 (7p)

### Google Container Registry 준비하기

### Docker 이미지의 업로드

### Docker 이미지의 다운로드와 작동 확인

---

  

# 제7장 여러 컨테이너의 운용 관리 (29p)

## 7.1 여러 컨테이너 관리의 개요 (5p)

### 웹 3계층 시스템 아키텍처

### 영구 데이터의 관리

### Docker Compose

## 7.2 웹 애플리케이션을 로컬에서 움직여 보자 (5p)

### Compose 구성 파일의 작성

### 여러 Docker 컨테이너 시작

### 여러 Docker 컨테이너 정지

## 7.3 Docker Compose를 사용한 여러 컨테이너의 구성 관리 (9p)

### docker-compose.yml의 개요

### 이미지 지정(image)

### 이미지 빌드(build)

### 컨테이너 안에서 작동하는 명령 지정(command/entrypoint)

### 컨테이너 간 연결(links)

### 컨테이너 간 통신(ports/expose)

### 서비스의 의존관계 정의(depends_on)

### 컨테이너 환경변수 지정(environment/env_file)

### 컨테이너 정보 설정(container_name/labels)

### 컨테이너 데이터 관리(volumes/volumes_from)

## 7.4 Docker Compose를 사용한 여러 컨테이너의 운용 (10p)

### Docker Compose의 버전 확인

### Docker Compose의 기본 명령

### 여러 컨테이너의 생성(up)

### 여러 컨테이너 확인(ps/logs)

### 컨테이너에서 명령 실행(run)

### 여러 컨테이너 시작/정지/재시작(start/stop/restart)

### 여러 컨테이너 일시 정시/재개(pause/unpause)

### 서비스의 구성 확인(port/config)

### 여러 컨테이너 강제 정지/삭제(kill/rm)

### 여러 리소스의 일괄 삭제(down)

# 제8장 멀티호스트 환경에서 Docker 실행 환경 구축 (19p)

## 8.1 멀티호스트 환경에서 컨테이너 관리의 개요 (2p)

### 멀티호스트 환경과 클러스터링

### Docker Machine이란?

## 8.2 웹 애플리케이션을 서비스 공개해 보자 (7p)

### Docker 실행 환경 작성

### 웹 애플리케이션 전개

### Docker 실행 환경 삭제

## 8.3 Docker Machine을 사용한 실행 환경 구축 (10p)

### Docker Machine의 기본 명령

### 실행 환경 작성(create)

### 실행 환경 목록 표시(ls/status/url)

### 실행 환경에 대한 SSH 연결(ssh)

### 실행 환경 시작/정지/재시작(start/stop/restart)

### 실행 환경으로부터 파일 다운로드(scp)

### 실행 환경 삭제(rm/kill)

### 실행 환경 정보 확인(ip/inspect)

# 제9장 클라우드를 사용한 Docker 실행 환경 구축 (50p)

## 9.1 클라우드 환경에서 Docker 오케스트레이션하기 (5p)

### 분산 환경에서의 컨테이너 운용 관리

### 퍼블릭 클라우드가 제공하는 매니지드 서비스

### Google Cloud Platform의 컨테이너 관련 서비스

## 9.2 Kubernetes의 개요 (10p)

### Kubernetes의 서버 구성

### 애플리케이션 구성 관리(Pod, ReplicaSet, Deployment)

### 네트워크 관리(Service)

### Label을 사용한 리소스 식별

### Kubernetes의 구조

## 9.3 GCP를 사용한 Docker 애플리케이션 개발 (10p)

### 애플리케이션 개발 흐름

### 소스코드 관리(Cloud Source Repositories)

### Docker 이미지 빌드(Cloud Container Builder)

## 9.4 GCP를 사용한 Docker 애플리케이션 실행 환경 구축 (25p)

### Kubernetes 클러스터 구축

### 애플리케이션의 설정 정보 관리(ConfigMap, Secrets)

### 앱의 전개(Deployment)

### 서비스 공개(Service)

### 앱의 버전업(Blue-Green Deployment)

### 배치 잡 실행(CronJob)

# 제10장 클라우드를 사용한 Docker 실행 환경의 운용 관리 (20p)

## 10.1 시스템 운용의 기초 지식 (7p)

### 가용성 관리

### 수용성(Capacity) 관리

### 시스템 감시

## 10.2 GKE를 사용한 Docker 실행 환경의 운용 (13p)

### Kubernetes의 스테이터스 확인

### Kubernetes의 Pod 관리

### Kubernetes의 노드 관리

### Kubernetes의 리소스 작성/삭제/변경

### Kubernetes의 업그레이드/다운그레이드

### Stackdriver에서 로그 확인