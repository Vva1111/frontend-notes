**참고**
[도커 공식](https://docs.docker.com/engine/install/ubuntu/)
[설치법 정리 1](https://tolovefeels.tistory.com/entry/UBUNTU-Docker-Install)
[설치법 정리 2](https://lordofkangs.tistory.com/279)


### 저장소 키 다운로드, 등록 및 패키지 설치 등 세부 사항 자동 처리 명령 (권장)
```sh
sudo wget -qO- http://get.docker.com/ | sh
```

### 설치 세부 과정
#### 1. 기존 Docker 삭제
```sh
sudo apt-get remove docker docker-engine docker.io containerd runc
```


#### 2. 도커 설치 준비
- **apt 명령어를 통해 간편하게 설치할 수 있도록 패키지 설치 및 저장소 등록
```sh
sudo apt-get update

sudo apt-get apt-transport-https ( 패키지 관리자가 https를 통해 데이터 및 패키지에 접근할 수 있도록 한다. - 필수사항 X )

sudo apt-get software-properties-common ( 개인 패키지 저장소(개발자가 소스코드 업로드 하면 자동으로 패키지화, 사용자가 다운로드 받아 설치할 수 있게 해주는 소프트웨어 저장소)를 추가/제거할 때 사용 - 필수사항 X )

sudo apt-get -y install ca-certificates curl gnupg ( SSL/TLS인증서 관리 패키지 다운로드, 시스템에서 인증할 수 있는 인증 기관(CA)의 인증서를 포함해 보안된 웹사이트와의 통신을 가능하게 한다. )

* -y : 모든 yes/no 입력에 yes로 자동 입력
```

#### 3. 도커 공식 GPG 키 추가 과정
- **도커에서 공식적으로 지원하는 저장소 추가를 위해 저장소가 인증된 저장소인지 확인할 수 있는 인증키 다운로드 필요**
- **다운로드 할 디렉토리 생성**
```sh
sudo mkdir -m 0755 -p /etc/apt/keyrings
```
- **키 다운로드 명령** ( 두 방법 중 아래 방법이 현재 정상적으로 gpg 키를 가져올 수 있음, 첫 번째 방법은 gpg키를 다운로드 하긴 하지만 apt-key list에서 인식하지 않는다. )
```sh
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
```

- **도커를 설치할 수 있는 저장소 등록** ( 리파지토리를 다시 등록하려면, 리파지토리가 등록되어 있는 설정 파일을 삭제하고 다시 등록해야 한다. )
```sh
echo "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu $(. /etc/os-release && echo $VERSION_CODENAME) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
```

- **다운로드 받은 인증키 파일에 읽기 권한 부여**
```sh
sudo chmod a+r /etc/apt/keyrings/docker.gpg
```


#### 4. 도커 설치
- **설정한 도커 저장소를 인식할 수 있도록 apt 업데이트**
```sh
sudo apt-get update
```

- **도커 공식 저장소에서 다운로드 받고 설치**
```sh
sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```

#### 5. 실행
- **도커 버전 확인**
```sh
sudo docker --version
```

#### 6. sudo 명령 없이 사용
```sh
sudo usermod -aG docker 로그인아이디
```
- -a : 사용자를 지정된 그룹에 추가하는 옵션
- -G : 그룹을 지정하는 옵션
- -aG : 사용자를 기존 그룹에 추가하면서 현재 속한 다른 그룹에서 제거하지 않는다.