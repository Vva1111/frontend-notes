- **docker --help** : docker 명령어 도움

- **docker ps** : docker container 목록 확인

- **docker images** : docker image 목록 확인
	- **docker images | grep node** : 이미지 이름이 node인 docker image만 확인
	- **docker commit** : 지정된 컨테이너를 바탕으로 새로운 이미지 생성
	- **docker container export <컨테이너 식별자>** : 지정된 컨테이너를 .tar 파일로 export
	- **docker image import <파일 또는 URL> | - \[이미지명\[:태그명]]** : .tar 파일로부터 이미지 작성
		- docker import/export : 컨테이너를 작동시키는데 필요한 파일을 모두 압축 아카이브로 모아 컨테이너의 루트 파일 시스템을 그대로 추출할 수 있다.
	- **docker image save -o <저장 파일명> \[대상 이미지]** : 이미지를 .tar 로 저장
	- **docker image load -i \[파일명]** : .tar 로부터 이미지를 읽어들인다.
		- docker image save/load : 이미지의 레이어 구조도 포함된 형태로 압축 아카이브를 모을 수 있다. 
	- import / export 방식과 save / load 방식은 바탕이 되는 이미지는 같지만, 내부적인 디렉토리와 파일 구조가 다르다.

- **docker logs -f -n {나타낼 log 수} {container id}** : 해당하는 컨테이너 id의 로그 확인
	- -f : follow, 로그 출력이 실시간으로 계속 이어서 표시됨, 새로운 로그 발생 시 즉시 콘솔에 출력
	- -n / --tail : 이후에 선언한 숫자 만큼의 log 표현

- **docker run -d --name "컨테이너 이름" -p 8080:80 <이미지 이름> /bin/ping localhost**
	- docker 이미지를 사용해 한번에 컨테이너 생성 및 실행 명령
	- -d : background 실행
	- --name : 만들고, 실행할 컨테이너의 이름 지정
	- -p : 포트 지정, 호스트 포트 번호:컨테이너 내부 포트 번호
		- 8080:80 은 호스트의 8080번 포트와 컨테이너의 80번 포트를 매핑한다.
	- /bin/ping : 컨테이너 shell의 /bin/ping 명령을 실행하는 컨테이너 생성 및 작동.
		- localhost : /bin/ping를 실행하는 대상을 지정한다.
	- 그 외 옵션
	- -v 호스트 디렉토리:컨테이너 디렉토리 : 볼륨 설정, 호스트-컨테이너 디렉토리를 공유한다. 
	- -e : 환경 변수를 지정 명령
	- --net=네트워크 이름 : 도커 컨테이너를 만들고 실행함과 동시에 해당 컨테이너가 속할 네트워크를 지정한다.

- **docker network**
	- **docker network ls** : network 목록 표시
	- **docker network create** : 사용자 정의 네트워크 생성
	- **docker network connect(or disconnect) <네트워크 이름> <컨테이너 이름 or ID>**
		컨테이너를 특정 네트워크와 연결(또는 연결 해제)한다.
	- **docker network inspect {network 이름}** : 해당하는 네트워크 정보 파악
	- **docker network rm** : 네트워크 삭제

- **docker build -t {이미지 이름}:{태그} {Dockerfile 위치 or . }** .
	- docker 이미지를 빌드하는 명령어
	- -t : 해당 이미지에 이름을 주기 위한 명령어
	-  . : 현재 디렉토리에 있는 Dockerfile을 사용한다는 의미
	- 사용하기 위한 Dockerfile의 위치는 설정 가능
	- 위 명령어로 이미지가 생성되면, ci.yml 파일에서 생성된 이미지를 지정해서 빌드-배포 과정에서 사용할 수 있다

- **docker rm** : 한 개 이상의 컨테이너 삭제
	1. **docker ps -a** : 도커 컨테이너 목록 확인
	2. **docker stop** {컨테이너 이름 또는 아이디}
	3. **docker rm** {컨테이너 이름 또는 아이디}
- **docker rmi** : 한 개 이상의 이미지 삭제
	- **docker rmi $(docker images | grep <image_name> | awk '{print $3}')** : 이미지 이름에 해당하는 이미지만 삭제
		- **awk '{print $3}'** :  각 행의 3번째 열(이미지 id)을 추출
	- **docker rmi -f <image name / id>** : 도커 이미지 강제 삭제

- **docker exec -it {컨테이너 이름 또는 id} bash
	- 컨테이너 내부에 설치되어 있는 이미지의 경우, 호스트 시스템에서 직접 해당 이미지의 명령어를 실행할 수 없기 때문에 컨테이너 내부로 진입해 명령어를 실행해야 한다. 
	- 그 때 해당하는 컨테이너 내부로 진입해 bash를 사용하기 위한 명령어

- **docker diff** : 컨테이너 파일시스템 내의 파일 혹은 디렉터리의 변경사항 확인
- **docker top** : 컨테이너의 프로세스 확인
- **docker port** :  컨테이너의 포트 전송 확인
- **docker rename old new** : 컨테이너의 이름 변경
- **docker cp <컨테이너 식별자>:<컨테이너 내부 파일 경로> <호스트 디렉토리 경로>**
	- 컨테이너 내부 파일을 호스트로 복사
- **docker cp <호스트 파일> <컨테이너 식별자>:<컨테이너 내부 파일 경로>**
	- 호스트의 파일을 컨테이너로 복사