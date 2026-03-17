##### 1. 파이썬 3.7.9 인스톨

##### 2. 파이썬 3.7버전으로 venv생성
- py -3.7 -m venv .venv
- $ python3.7 -m venv .venv

##### 3. venv환경 실행
- .venv\scripts\activate
- $ source venv/bin/activate

##### 4. pip 업그레이드
- python -m pip install --upgrade pip
- pip install --upgrade pip setuptools

##### 5. requirements.txt 설치
- pip install -r requirements.txt


##### 6. o/s 환경변수 추가
- 아래 환경변수 추가
	DB_PORT=5432
	DB_USER=yescnc
	DB_PWD=yescnc123!
	DB_NAME=davis_lite
	DB_HOST=200.100.1.136
	REDIS_HOST=127.0.0.1
	REDIS_PORT=6379

##### 7. postgresql설치
- sudo sh -c 'echo "deb http://apt.postgresql.org/pub/repos/apt $(lsb_release -cs)-pgdg main" > /etc/apt/sources.list.d/pgdg.list'

- wget --quiet -O - https://www.postgresql.org/media/keys/ACCC4CF8.asc | sudo apt-key add -

- sudo apt-get update

- sudo apt-get -y install postgresql

- sudo apt-get install libpq-dev python3-dev

##### 8. 파이썬 프로젝트 실행