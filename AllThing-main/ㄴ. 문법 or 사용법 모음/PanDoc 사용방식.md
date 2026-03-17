[참고](https://woogyun.tistory.com/721)
### Window
- **choco install pandoc** 
	- **choco(Chocolatey)** 는 윈도우 환경에서 소프트웨어를 설치, 업데이트 및 관리할 수 있는 패키지 관리자입니다. 주로 명령줄 인터페이스(CLI)를 통해 작동하며, 리눅스의 APT나 YUM과 비슷한 역할을 합니다. Chocolatey CLI를 사용하면 다양한 프로그램과 도구를 간편하게 설치하고 관리할 수 있습니다. (GPT)

- 중간에 Do you want to run the script? 라는 메세지가 나타나는데, 
	![[Pasted image 20240701112037.png]]
	- 이 메세지는 Pandoc 설치 과정 중에 특정 스크립트를 실행할 지 여부를 묻는 메세지이다.
	- 설치 후 Pandoc을 제대로 설정하거나 추가적인 설정 작업을 자동으로 수행하기 위한 스크립트 일 수 있다.

- 설치 완료 후에는 refreshenv를 실행해 환경 변수의 변경을 반영해야 한다는 메세지가 나오는데
	![[Pasted image 20240701112307.png]]
	- pandoc의 경우 이미 추가된 PATH 내에 있으므로 굳이 refreshenv를 실행하지 않아도 된다.

- HTML 생성
	- 변환을 원하는 파일이 있는 디렉토리로 이동
	- **pandoc** test.md **-f** markdown **-t** html **-s** **-o** test.html **--metadata title**="문서 제목"
		- **pandoc** : pandoc 프로그램 호출
		- **test.md** : 변환할 대상(입력) 파일
		- **-f markdown** : 입력 파일의 형식 지정 ( -from )
		- **-t html** : 출력 파일의 형식 지정 ( -to )
		- **-s** : standalone 옵션, 이 옵션은 HTML 문서를 독립적으로 사용할 수 있도록 html, head, body 태그를 포함한 완전한 HTML 문서로 생성한다. ( -standalone )
		- **-o** : 출력 파일의 이름 지정 ( -output )
		- **--metadata title** : 변환된 HTML 문서에 title이 비어있을 경우 경고메세지가 발생할 수 있으며, 이를 해결하기 위해 해당 명령어를 사용해 title을 지정해준다.

- 실행
	- **./변환 파일.html**
