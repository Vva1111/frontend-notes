- Docker-compose 명령을 사용해 아래와 같은 주요 시나리오를 대상으로 지정할 수 있다.

  

## 1. 개발 환경

- 애플리케이션을 개발하는 경우 격리된 개발 환경에서 애플리케이션을 실행할 수 있어야 하는 것은 중요하다.

- docker-compose CLI 명령을 사용해 해당 환경을 만들거나 내부에서 docker-compose를 사용하는 Visual Studio를 사용할 수 있다.

- docker-compose.yml 파일을 사용해 모든 애플리케이션의 서비스 종속성(다른 서비스, 캐시, 데이터베이스, 큐 등)을 구성하고 문서화할 수 있다.

- docker-compose CLI 명령을 사용해 단일 명령(docker-compose up)으로 각 종속성에 대해 하나 이상의 컨테이너를 만들고 시작할 수 있다.

- docker-compose.yml 파일은 Docker 엔진으로 해석되는 구성 파일이지만 다중 컨테이너 애플리케이션의 구성에 대한 편리한 문서 파일의 역할도 한다.

  

## 2. 테스트 환경

- 모든 CI/CD 프로세스의 중요한 부분은 단위 테스트 및 통합 테스트이다.

- 이 자동화된 테스트는 격리된 환경이 필요하므로 사용자 또는 애플리케이션 데이터의 다른 변경 내용에 의해 영향을 받지 않는다.

- Docker Compose를 사용해 아래 명령과 같은 명령 프롬프트 또는 스크립트의 몇 가지 명령에서 매우 쉽게 격리된 해당 환경을 만들고 제거할 수 있다.

```docker
docker-compose -f docker-compose.yml -f docker-compose-test.override.yml up -d
./run_unit_tests

docker-compose -f docker-compose.yml -f docker-compose-test.override.yml down
```

  

## 3. 프로덕션 배포

- Compose를 사용해 원격 Docker 엔진에 배포할 수도 있다.

- 일반적인 경우는 단일 Docker 호스트 인스턴스(프로덕션 VM 또는 Docker 컴퓨터로 프로지번 된 서버)를 배포하는 것이다.

- 다른 오케스트레이터(k8s, Azure Service Fabric 등)를 사용하는 경우, 다른 오케스트레이터에서 필요한 형식이 아닌 docker-compose.yml의 형식과 같은 설정 및 메타데이터 구성 설정을 추가해야 한다.

- 어떤 경우에서든 docker-compose는 프로덕션 워크플로가 사용하는 오케스트레이터에 따라 다를 수 있음에도 불구하고 개발, 테스트 및 프로덕션 워크플로에 편리한 도구 및 메타데이터 형식이다.