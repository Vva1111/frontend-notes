
## 1. 유료 계정 전환
  

프리티어(Free Tier) 계정에서 유료 계정으로 변경하는 방법

  
1. Billing & Cost Management > Billing > Upgrade and Manage Payment로 이동

2. 신용카드 등록
  
support.oracle.com 기술 서비스 요청 2차 인증 글로벌 엔지니어

> 승인까지 약 4시간 정도 소요됨

  

---

  

## 2. 인스턴스 생성
  

### 1) Compartment 생성

  

Compartment는 Oracle Cloud Infrastructure(OCI) 자원을 논리적으로 묶는 역할을 담당한다.

  

1. Identity & Security > Identity > Compartments로 이동

2. Create Compartment 버튼 클릭

3. 관련 정보 입력 후 Create Compartment 버튼 클릭

4. Compartments 목록에 추가된 것 확인

  

### 2) VCN 생성

  

Compartment 내에서 가상 네트워크를 구성하기 위해 VCN(Virtual Cloud Networks)를 생성해야 한다. 이곳에 Instance가 위치하게 된다.

  

1. Networking > Virtual cloud networks로 이동

2. 좌측 List scope의 Compartment가 올바르게 선택되었는지 확인

3. Start VCN Wizard 버튼 클릭

4. 관련 정보 설정 후 Create 버튼 클릭

5. 잠시 기다리면 생성 완료되고, View VCN 버튼을 클릭해서 정보 확인 가능

  

    - Subnet, Network Security Group 등이 자동으로 생성됨

  

##### 설정 정보

  

-   VCN name 외에는 기본값으로 설정함

  

### 3) Instance 생성

  

1. Compute > Instances로 이동

2. 좌측 List scope의 Compartment가 올바르게 선택되었는지 확인

3. Create instance 버튼 클릭

4. 관련 정보 설정

  

    1. Image(선택한 Shape에서 동작할 OS를 선택하는 옵션) 설정

    2. Shape(CPU 수, 메모리와 같은 리소스를 선택하는 옵션) 설정

    3. Save private key 버튼 클릭

  

        - SSH 접속을 위해 필요한 key 다운로드

  

    4. Boot volume 설정

  

5. Create 버튼 클릭

6. 잠시 기다리면 생성 완료되고, Instances 목록에 추가된 것 확인

  

##### 설정 정보

  

-   Image는 Ubuntu 22.04 선택

-   Shape는 Ampere, OCPU 4 Core, Memory 24GB (과금되지 않는 최대 기준 적용) 선택

-   Boot volume은 기본값(Use in-transit encryption) 선택

  

---

  

## 3. 예약된 퍼블릭 IP 주소 할당

  

Public IP adress를 통해 SSH 접속이 가능한데, 기본 IP는 유동적으로 바뀔 수 있기 때문에 IP를 고정해주는 작업이 필요하다.

  

### 1) 예약 IP 주소 생성

  

1. Networking > IP management > Reserved public IPs로 이동

2. 좌측 List scope의 Compartment가 올바르게 선택되었는지 확인

3. Reserve public IP address 버튼 클릭

4. 이름 입력 후 Reserve public IP address 버튼 클릭

5. Reserved public IPs 목록에 추가된 것 확인

  

### 2) 예약된 퍼블릭 IP 주소를 인스턴스에 할당

  

1. 설정 화면 진입

  

    1. Compute > Instances로 이동 후 인스턴스 선택

    2. Instance Details 화면에서 아래로 스크롤 한 뒤 Resources > Attached VNICs 클릭

    3. 인스턴스에 연결된 VNIC 클릭

    4. VNIC Details 화면에서 아래로 스크롤 한 뒤 Resources > IPv4 Address 클릭

  

2. 기존에 할당된 IP를 제거하기 위해  

   더보기 버튼(⋮) 클릭 → Edit 선택 → No public IP 선택 → Update 버튼 클릭

3. Reserved public IP 할당을 위해  

   더보기 버튼(⋮) 클릭 → Edit 선택 → Reserved public IP 선택 → Update 버튼 클릭

4. 변경된 IP 확인

  

    1. IPv4 Addresses 목록에서 변경 내용 확인

    2. Compute > Instances로 이동 후 인스턴스의 Public IP 확인

  

---

  

## +. Instance 외부 접속 포트 설정

  

인스턴스에 Web server나 API server를 띄워도 접속이 되지 않을 수 있는데, 이는 포트가 열려있지 않아 접근을 모두 차단하기 때문이다. 따라서 외부에서 접근할 수 있도록 포트를 열어주어야 한다.

  

1. 설정 화면 진입

  

    1. 방법 1

  

        1. Compute > Instances로 이동 후 인스턴스 선택

        2. Instance Details 화면에서 Subnet 클릭

        3. Subnet Details 화면에서 Security Lists의 'Default Security List for [VCN이름]' 클릭

  

    2. 방법 2

  

        1. Networking > Virtual cloud networks로 이동 후 VCN 선택

        2. Subnets 목록에서 public subnet 선택

        3. Subnet Details 화면에서 Security Lists의 'Default Security List for [VCN이름]' 클릭

  

2. Add Ingress Rules 버튼을 클릭해 새로운 규칙 추가

3. 관련 정보 입력 후 Add Ingress Rules 버튼 클릭

4. Ingress Rules 목록에 추가된 것 확인

  

##### 설정 정보

  

-   Source CIDR에는 모든 접속을 허용하기 위해 0.0.0.0/0 입력

-   Destination Port Range에 외부 접속을 허용할 포트 번호 입력 ex) 8000

-   Description에 어떤 용도로 사용하는 포트인지 입력 ex) FastAPI Port