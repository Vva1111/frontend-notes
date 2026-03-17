``` yaml
apiVersion: v1
kind: Service
metadata:
  name: my-service
  namespace: my-namespace
  labels:
    app: my-app
    tier: backend
  annotations:
    service.beta.kubernetes.io/aws-load-balancer-backend-protocol: http
  finalizers:
    - service.kubernetes.io/load-balancer-cleanup
```

- 위 내용을 적용하면, 클러스터 내부 네임스페이스(위에서는 my-namespace)에 metadata 에 설정한 name 대로 service 가 생성
#### metadata 필드

- **name**: 생성하려는 서비스의 이름 설정
- **namespace**: 생성하려는 서비스가 속하게 될 네임 스페이스 지정
- **labels**: key:value, 리소스를 선택하거나 분류, 셀렉터 연동 등에 사용됨
- **annotation**: key:value, 머신 전용 정보나 임시 설정 등에 사용됨, 리소스 동작에는 영향 없음
- **finalizers**: 리소스 삭제 전에 반드시 실행되어야 할 클린업 작업 정의. 주로 외부 리소스 해제나 정리에 사용

``` 
my-service.default.svc.cluster.local
```
- 위 yaml을 적용했을 때, my-service 라는 이름의 서비스가 생성된다.
- 이 이름은 클러스터 DNS 이름으로도 사용된다.
	- **default** : 네임 스페이스
	- **svc**: 서비스
	- **cluster.local**: 클러스터 도메인(기본 값)
- ####

#### Spec 필드
- Service 리소스의 핵심 기능은 트래픽을 클러스터 내부 또는 외부에서 적절한 Pod로 라우팅 하는 것이고, 이를 제어하는 설정이 Spec 설정이다.
``` yaml
apiVersion: v1
kind: Service
metadata:
  name: my-service
spec:
  type: ClusterIP
  selector:
    app: my-app
  ports:
    - protocol: TCP
      port: 80
      targetPort: 8080
```
- **type**: 서비스 타입 ( 트래픽 노출 방식 지정 )
- **selector**: 어떤 Pod(label)에 트래픽을 보낼지 선택
- **ports**: 서비스가 노출할 port 및 매핑 정보
- **sessionAffinity**: 클라이언트 요청을 같은 Pod에 계속 보낼지 여부 (기본값 `None`, 설정 시 `ClientIP`)
- **externalTrafficPolicy**: type: LoadBalancer 또는 NodePort 일 때 외부 트래픽을 처리하는 방식 설정

- **type의 종류**
	- **ClusterIP (기본 값)** : 클러스터 내부에서만 접근 가능
		- 클러스터 내부 IP 생성
		- 외부에서는 접근 불가
		- 보통 Pod 간 통신, 프론트-백엔드 연결, 내부 API 통신 등에 사용
		- DNS 이름은 `my-service.default.svc.cluster.local` 형태
	- **NodePort**: 각 노드의 IP + 지정된 포트로 외부 접근 허용
		- 노드의 IP 주소 + 특정 port로 외부에서 접근 가능
		- 클러스터의 모든 노드 IP:30080으로 요청 시 서비스에 연결
		- 외부에서 간단한 접근 테스트용으로 사용
		- 한계 : 포트 범위 제한(30000 ~ 32767), 로드 밸런싱 없음
	- **LoadBalancer**: 클라우드 환경에서 외부 LoadBalancer를 생성해 접근 허용
		- 클라우드 제공자(AWS 등)에서 외부 Load Balancer를 자동 생성
		- 외부 사용자가 공인 IP로 직접 접속 가능
		- 내부적으로는 NodePortg 도 함께 사용됨(LoadBalancer -> NodePort -> Port)
		- 가장 일반적인 외부 서비스 노출 방식
		- 단점: 클라우드 환경에서만 지원, 비용 발생
	- **ExternalName**: 실제로는 엔드포인트 없이, 외부 DNS 이름으로 리다이렉트
		- type: ExternalName
		- 클러스터 외부 DNS 이름으로 포워딩
		- 대상: 클러스터 외부의 서비스 (외부 DB 등)
	- **Headless Service ( ClusterIP: None )
		- spec:
		  clusterIP: None
		- 실제 Service IP를 생성하지 않음
		- 클러스터 내부 Pod IP 직접 노출, 클러스터 내부 Pod를 대상으로 한다.
		- 대신, DNS가 Pod들의 IP 목록을 직접 반환
		- 각 Pod에 직접 트래픽을 보내거나, 서비스 디스커버리 용도로 사용
		- 각 Pod를 개별적으로 식별/연결할 때 사용된다.