## 1. 알림 설정

  

사용량 임계치 도달시 이메일 발송 설정

  

> 금액 단위: 싱가폴 달러

  

1. Oracle Cloud의 Billing & Cost Management > Cost Management > Cost Analysis로 이동

2. Budgets의 Create Budget 버튼 클릭

3. 이메일 발송 조건 설정 후 Create 버튼 클릭

  

    - **Budget Alert Rule (optional)** 영역이 알림 조건을 설정하는 부분임

    - Email Recipients 에 수신할 이메일을 작성해야 함

  

4. Budgets 목록에 추가된 것 확인

  

##### 참고 링크

  

-   [https://www.youtube.com/watch?v=cnJ9DjqQhBk](https://www.youtube.com/watch?v=cnJ9DjqQhBk)

  

---

  

## 2. ssh 접속

  

### 1) 인터넷 통신사가 SK가 아닌 경우

  

#### [1단계] private key 권한 변경

  

키 파일의 권한이 기본적으로 644로 설정이 되어 있는데, SSH 접속을 위해서는 600 이하여야 한다.

  

##### [Windows] 키 파일의 권한을 400으로 변경하는 예

  

> powershell에서는 안되고 cmd에서만 가능

  

```

icacls.exe [키파일명] /reset

icacls.exe [키파일명] /grant:r %username%:(R)

icacls.exe [키파일명] /inheritance:r

```

  

##### [Linux] 키 파일의 권한을 600으로 변경하는 예

  

-   기존 권한 확인

    ```

    ls -l [키파일명]

    ```

-   600으로 변경

    ```

    chmod 600 [키파일명]

    ```

  

#### [2단계] SSH 접속

  

```

ssh -i [키파일명] ubuntu@[퍼블릭 IP 주소]

```

  

### 2) 인터넷 통신사가 SK인 경우

  

SK브로드밴드 통신사에서 SSH 아웃바운드를 막아놓아서 SSH 접속 시도를 하면 ‘Connection timed out’ 에러가 발생한다.

  

기본 포트인 22 포트는 막혀있으므로, **SSH 접속을 위한 다른 포트를 추가**해야 한다.

  

#### [1단계] 클라우드 방화벽 설정

  

> Oracle Cloud에서 외부 접속 포트 2222 추가

  

1. 설정 화면 진입

  

    1. Networking > Virtual cloud networks로 이동 후 VCN 선택

    2. Subnets 목록에서 public subnet 선택

    3. Subnet Details 화면에서 Security Lists의 'Default Security List for [VCN이름]' 클릭

  

2. Add Ingress Rules 버튼을 클릭해 새로운 규칙 추가

3. 관련 정보 입력 후 Add Ingress Rules 버튼 클릭

  

    - Source CIDR에는 모든 접속을 허용하기 위해 0.0.0.0/0 입력

    - Destination Port Range에 외부 접속을 허용할 포트 번호 2222 입력

  

4. Ingress Rules 목록에 추가된 것 확인

  

#### [2단계] OS 방화벽 설정

  

> Oracle Cloud에서 지원하는 Cloud Shell을 통해 SSH 접속 후 작업

  

1. private key 사용 준비

  

    1. 설정 버튼(⚙) 클릭 → Upload 선택 → private key 업로드

    2. .ssh 디렉토리 생성 후 private key를 해당 디렉토리로 이동

  

        ```

        mkdir .ssh

        mv [키파일명] .ssh/[키파일명]

        ```

  

    3. private key 권한을 600이하로 변경

  

2. ssh 접속

  

    ```

    ssh -i .ssh/[키파일명] ubuntu@[퍼블릭 IP 주소]

    ```

  

3. ubuntu 사용자 비밀번호 설정

  

    - 이후에 수행하는 일부 명령어는 비밀번호를 확인한 후에 실행되기 때문에 미리 설정해 두어야 함

  

    ```

    sudo passwd ubuntu

    ```

  

4. root 사용자 추가 및 비밀번호 설정

  

    ```

    sudo useradd root

    ```

  

    ```

    sudo passwd root

    ```

  

5. ssh 포트 추가

  

    1. sshd_config 파일에 포트 추가

  

        - SSH 접속을 위한 기존 22 포트와, 새로 추가하는 포트 2개 모두 허용해야 함

  

        ```

        vi /etc/ssh/sshd_config

        ```

  

        ```

        Port 22

        Port 2222

        ```

  

    2. SSH 서비스 재시작 후 sshd 확인

  

        - SSH 서비스 재시작

            ```

            systemctl restart ssh

            ```

        - 현재 실행 중인 프로세스 중에서 sshd(SSH 데몬) 프로세스와 관련된 네트워크 소켓 정보를 확인하는 명령. SSH 서버가 제대로 실행되고 있는지, 어떤 포트로 접속해야 하는지 등을 확인

            ```

            netstat -tnlp | grep sshd

            ```

  

6. ufw 방화벽 설정

  

    1. ufw 방화벽 상태 확인

  

        ```

        sudo ufw status verbose

        ```

  

    2. 비활성(inactive) 상태인 경우 활성화

  

        ```

        sudo ufw enable

        ```

  

    3. 추가한 포트에 대한 접근 허용

  

        ```

        sudo ufw allow 2222

        ```

  

    4. 변경사항을 적용하기 위한 reload

  

        - ufw가 현재 규칙 세트를 reload하고 변경사항을 적용함

  

        ```

        sudo ufw reload

        ```

  

7. iptables 방화벽 설정

  

    ```

    sudo iptables -I INPUT -p tcp -m tcp --dport 2222 -j ACCEPT

    ```

  

#### [3단계] 추가한 포트를 통해 SSH 접속

  

이제 로컬 컴퓨터에서도 SSH 접속 가능!

  

```

ssh -i [키파일명] ubuntu@[퍼블릭 IP 주소] -p 2222

```

  

---

  

## +. ufw와 iptables

  

### ufw(Uncomplicated Firewall)

  

iptables 규칙을 관리하기 위한 사용자 친화적인 인터페이스

  

### iptables

  

-   리눅스 커널에 내장된 네트워크 패킷 필터링 시스템

-   패킷을 허용하거나 차단하는 규칙을 직접 설정할 수 있어서 네트워크 보안을 강화할 수 있음

  

---

  

## +. SSH 포트 추가시 iptables로 설정해야만 접속이 되는 이유

  

ufw는 iptables 규칙을 생성하고 관리하는 툴이지만, 실제로는 iptables 규칙 체인 중 주로 INPUT 체인의 규칙만 관리한다.

  

iptables에는 패킷이 통과하는 여러 체인이 있다. INPUT 체인 외에도 FORWARD, OUTPUT 등이 있는데, 이런 체인에서 해당 포트가 차단되고 있었다면 ufw로 열어준 규칙보다 우선순위가 높아서 접속이 되지 않았을 수 있다.

  

그래서 iptables 명령어로 INPUT 체인에 직접 규칙을 추가하고 나서야 다른 체인의 규칙보다 우선적으로 적용되어 접속이 가능해진 것이다.

  

---

  

## +. iptables 체인 종류

  

1. INPUT

2. FORWARD

3. OUTPUT

  

### 1. INPUT 체인

  

로컬 시스템으로 들어오는 네트워크 패킷을 처리하는 데 사용되는 체인

  

#### INPUT 체인에 설정된 규칙 확인

  

```

iptables -L INPUT

```

  

-   체인 이름 (기본 정책)

    -   기본 정책 종류

        -   ACCEPT: 체인의 규칙과 매칭되지 않는 패킷을 허용

        -   DROP: 체인의 규칙과 매칭되지 않는 패킷은 모두 차단

-   target: 대응 동작

-   prot: 프로토콜

-   source: 패킷의 출발지 주소

-   destination: 패킷의 목적지 주소

  

> source, destination 필드를 통해 특정 IP 주소나 네트워크 대역에서 들어오거나 나가는 패킷을 선택적으로 제어할 수 있다.

  

### 2. FORWARD 체인

  

로컬 시스템을 통과하는 패킷을 처리하는 데 사용되는 체인  

; 다른 네트워크 간에 중계되는 패킷을 관리하는 데 사용되는 체인

  

-   주로 라우터나 방화벽 역할을 하는 시스템에서 중요함

-   클라이언트-서버 모델의 단순한 시스템에서는 크게 신경쓰지 않아도 되지만, 네트워크 토폴로지가 복잡해지면 FORWARD 체인 규칙 관리가 중요해짐

  

#### FORWARD 체인에 설정된 규칙 확인

  

```

iptables -L FORWARD

```

  

### 3. OUTPUT 체인

  

로컬 시스템에서 나가는 네트워크 패킷을 처리하는 데 사용되는 체인  

; 로컬 프로세스에서 생성된 패킷이 네트워크로 전송되기 전에 OUTPUT 체인의 규칙에 따라 처리됨

  

-   주로 로컬 프로세스에서 생성된 트래픽을 제어하는 데 사용됨

-   특정 IP나 포트로 나가는 트래픽을 차단하거나 허용

  

#### OUTPUT 체인에 설정된 규칙 확인

  

```

iptables -L FORWARD

```