#### * 패키지 빌드 이미지가 설치될 서버 정보는 runner에서 파악
#### * git 에서 서버에 설치된 러너 정보를 등록하면 해당 러너가 서버의 러너를 실행해 여러 작업을 수행한다.


##### 1. 도커허브에서 깃랩러너 이미지 다운로드
```sh
$ docker pull gitlab/gitlab-runner:latest
```


##### 2. gitlab-runner 이미지 실행
```sh
$ docker run -d --name gitlab-runner --restart always -v /data/runner/config:/etc/gitlab-runner -v /var/run/docker.sock:/var/run/docker.sock -e TZ=Asia/Seoul gitlab/gitlab-runner:latest
```


##### 3. gitlab에 runner 등록 (gitlab의 러너 등록방법 내용 참조)
```sh
$ docker exec -it gitlab-runner gitlab-runner register --url https://gitlab.cocoa.icu/ --registration-token GR1348941YujiWqVWL8HEtu4DgRiL
```
- **Gitlab Runner 토큰 등록( GPT )**
	GitLab Runner를 등록하기 위한 토큰은 GitLab의 프로젝트 설정 또는 GitLab 인스턴스 설정에서 찾을 수 있습니다. 등록 토큰은 Runner를 GitLab 서버에 등록할 때 사용되며, 이를 통해 Runner와 GitLab 사이의 인증을 가능하게 합니다. 토큰을 확인하는 방법은 다음과 같습니다:

- **프로젝트별 Runner 등록 토큰 확인하기**
	프로젝트에 특정한 Runner를 등록하고 싶다면, 프로젝트 설정에서 토큰을 찾을 수 있습니다.
	1. GitLab에 로그인합니다.
	2. 등록하려는 Runner가 작업할 프로젝트로 이동합니다.
	3. 좌측 사이드바에서 **설정(Settings)** > **CI / CD**를 선택합니다.
	4. "Runner" 섹션으로 스크롤하면, **프로젝트별 Runner 설정**이 보입니다.
	5. "Set up a specific Runner manually" 항목 아래에 **등록 토큰**이 표시됩니다.

- **전체 GitLab 인스턴스 Runner 등록 토큰 확인하기**
	GitLab 인스턴스에 대한 Runner를 등록하려면 인스턴스 수준의 토큰이 필요합니다. 이는 GitLab 관리자만 접근할 수 있습니다.

	1. GitLab에 관리자로 로그인합니다.
	2. 오른쪽 상단의 메뉴에서 **관리자 영역(Admin area)** (렌치 아이콘)을 클릭합니다.
	3. 좌측 메뉴에서 **Overview** 섹션 아래의 **Runners**를 선택합니다.
	4. 페이지 상단에 **Runners 토큰**이 표시됩니다.

	토큰을 확인한 후, GitLab Runner 등록 명령어를 실행할 때 이 토큰을 사용합니다. 명령어는 대략 다음과 같습니다:

	```bash
	gitlab-runner register --url [GitLab 서버 URL] --registration-token [토큰]
	```

	여기서 `[GitLab 서버 URL]`은 GitLab 인스턴스의 주소이고, `[토큰]`은 앞서 찾은 등록 토큰입니다. 해당 명령어를 통해 GitLab Runner를 등록하면, GitLab에서 해당 Runner를 사용하여 CI/CD 파이프라인을 실행할 수 있습니다.


#### 4. gitlab-runner 구성 변경
```sh
$ sudo nano /data/runner/config/config.toml
```

수정 및 추가해줘야 할 부분
```toml
..
[[runners]]
..
  [runners.docker]
    volumes = ["/var/run/docker.sock:/var/run/docker.sock", "/cache"]
    allowed_pull_policies = ["if-not-present", "always"]
    pull_policy = "if-not-present"
    ..    
```


#### 5. gitlab-runner config.toml 파일의 설정을 변경하게 될 경우, gitlab-runner 컨테이너가 down 되거나, 새로운 버전으로 변경되어 이미지가 새로 생성되는 경우 등, config.toml 설정이 초기화 되는 경우를 방지하기 위해 호스트에 구역을 할당해(volume) cofig.toml 설정이 변경되지 않도록 관리할 수 있다.

