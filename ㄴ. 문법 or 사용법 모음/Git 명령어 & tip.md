##### Tip.
1. gitignore 내부에 ignore할 목록들은 이미 추적되고 있는 파일들에 대해서 적용되지 않는다. gitignore에 새로운 규칙을 추가해도 이미 추적하고 있는 파일들은 그대로 남아있기 때문에 계속 추적되고, 업데이트 된다.
2. 1은 pull 명령어에도 동일하게 적용된다.
3.  이전 커밋을 되돌리거나 하는 상황 이후에 다시 pull을 받았을 때, 변경사항이 이전의 커밋에서 변경되지 않을때는 브랜치를 삭제한 후 새로 브랜치를 받고 pull을 받아 변경사항을 적용해야 한다. 해당 로컬 브랜치와 원격 브랜치는 서로 다른커밋을 가지고 있기 때문
4. 
5. 


##### 명령어
- **Git 초기 설정에 필요한 명령어**
	1. **git config**
		- git config --list : 현재 Git 설정 정보 확인
		- git config (--global) user.name(user.email) : 로컬(혹은 글로벌) 이름(이메일) 설정
		- 
	2. **git remote**
		- git remote add origin https://git~~~~~ : 현재 로컬의 원격 저장소를 설정
		- git remote set-url origin https://git~~~~~ : 현재 설정된 원격 저장소를 변경
		- git remote remove origin : 현재 설정된 원격 저장소를 삭제

- **git rm --cached {file}** : Tip. 1의 경우에서 로컬 파일은 그대로 남겨두고 추적만 중지하는 명령어

#### Config

###### **branch.{main}.merge
- Git에서 특정 브랜치(위는 main)가 다른 브랜치와 병합할 때 사용할 기본 병합 브랜치 지정 설정
- git pull 명령어를 사용할 때 어떤 브랜치에서 변경 사항을 가져올지 결정하는데 사용
- { } 에 지정된 브랜치가 병합할 기본 브랜치를 지정한다.
- 병합할 브랜치를 특정하지 않는 기본 git pull 명령어를 사용할 때 적용되는 설정


###### **upstream**
- 업스트림 브랜치는 로컬 브랜치가 추적하는 원격 브랜치를 의미한다.
- 업스트림 설정 이후에는 git push/pull 명령의 경우 자동으로 해당 원격 브랜치와 상호 작용한다.
- 업스트림 브랜치를 해제하면 git pull/push 명령어를 사용할 때 기본적으로 어떤 브랜치와 상호작용할지 git이 알 수 없다.

- **명령어**
	- **git branch -vv** : 현재 upstream 확인
	- **git branch --set-upstream-to origin/{remote branch name}**
	- **git push --set-upstream origin {remote branch name}**
		- 업스트림 브랜치 설정 명령어 
		- git branch~ 명령어는 기존 브랜치에 업스트림을 설정할 때 사용
		- git push~ 명령어는 해로운 브랜치를 푸시하면서 업스트림을 같이 설정할 경우 사용
	- **git branch --unset-upstream**
		- 현재 체크아웃한 브랜치의 업스트림 브랜치를 설정 해제하는데 사용


#### etc

###### **Credential - 인증**
- **git config --global credential.helper store 
- git config --list의 credential 항목은 Git에서 사용하는 인증 정보에 관련된 설정을 나타낸다.
	 원격 저장소와의 통신에는 매번 사용자 인증 정보가 필요한데, 이를 기억하게 하기 위한 기능이다.
	 - **Credential.helper** : Git이 인증 정보를 어떻게 저장하고 찾을지 결정
		 - 옵션 목록
			 - **store** : 인증 정보를 평문으로 git-credentials 파일에 저장, 
			 - **cache** : 인증 정보를 일정 시간 동안 메모리에 저장, 기본 15분, 'cache --timeout=시간' 으로 timeout 시간 설정 가능
			 - **osxkeychain** : macOS에서 사용할 수 있는 방식, macOS의 keychain에 자격 증명을 저장한다. 보안성 높음
			 - **wincred** : Windows에서 사용할 수 있는 옵션, Windows 자격 증명 관리자에 자격 증명을 저장한다. 보안성 높음
			 - **manager 또는 manager-core** : Git Credential Manager를 사용해 자격 증명을 관리한다. Windows, macOS, Linux 모두 사용 가능하며, 여러 인증 방법을 지원한다.

- **git config --global --unset credential.helper
	- credential 설정 삭제
	- 해당 명령을 사용하면 Git은 더 이상 자격 증명 저장 방식을 사용하지 않는다.


###### Fetch
- **git fetch origin a-branch<원격>:a-branch<로컬>**
	원격 저장소의 특정 브랜치(a-branch) 최신 상태를 가져와서, 로컬에 있는 동일한 이름의 브랜치에 곧바로 반영(업데이트) 하거나 새로 생성하는 명령어
	
	현재 재가 작업 중인 브랜치를 이동(checkout) 하지 않고도, 다른 로컬 브랜치를 최신 상태로 업데이트 하고 싶을 때 사용하기 용이하다.

###### **Merge**
- 히스토리 유지하면서 다른 브랜치 내용 현재 브랜치로 가져오기
	```
	git checkout 현재브랜치
	git merge -s ours 다른브랜치
	git checkout 다른브랜치 -- .
	git commit -am "~~"
	```
	- merge -s ours 는 히스토리를 병합하지만 실제 내용은 무시
	- 이후 checkout 다른브랜치 -- . 를 통해 실제 파일만 가져와 덮어쓰기
	- 기존 커밋 히스토리는 남기면서 파일만 덮어씌울 때 유용

###### **Revert**
- 특정 커밋의 변경 사항을 취소하는 새로운 커밋 생성
- '되돌리는' 것이 아니라 해당 커밋의 반대 작업을 수행해 새로운 커밋을 만드므로, 되돌린 커밋도 기록에 남긴다.
```git
git revert -m <commit_hash>
```
- **-m** : mainline을 의미하며, 병합 커밋의 부모 중에서 어떤 부모를 기준으로 되돌릴지를 설정한다.
- revert 할 커밋의 부모 커밋을 확인하려면 **git show <확인할 커밋 hash>** 를 사용한다.


###### **Reset**
- 현재 브랜치의 HEAD를 특정 커밋으로 이동하고, 그 이후의 커밋을 삭제 또는 변경
- **--soft** : HEAD를 이동시키지만, 작업 디렉토리와 인덱스는 변경하지 않는다.
- **--mixed** : HEAD를 이동시키고, 인덱스를 변경하지만 작업 디렉토리는 변경되지 않는다.
- **--hard** : HEAD를 이동시키고, 인덱스와 작업 디렉토리 모두를 변경한다.
```git
git reset --mixed <commit_hash>
```


###### **checkout**
- **git checkout -b <생성하려는 브랜치> origin/<참고하려는 원격 브랜치>** : origin의 브랜치를 바라보는 로컬 브랜치를 생성하고, 해당 브랜치로 이동
- **git checkout -t <원격 저장소에 존재하는 브랜치 이름>** : 원격 저장소에 존재하는 브랜치와 동일한 이름으로 로컬 브랜치 생성 후 브랜치 이동


###### **commit**
- **git add .(혹은 파일 이름)** : 모든 파일 staging
- **git restore --staged .(혹은 파일 이름)** : staging 되어 있는 모든 파일 unstaging
- **git commit -m '커밋메세지'** : 스테이징 된 파일들 commit, 일정 파일만 업로드 하고 싶다면 해당하는 파일만 스테이징 시킨 후 해당 명령어를 사용한다.
- **git commit --amend -m '수정할 메세지'** : 방금 전 커밋의 메세지만 변경하고 싶은 경우 사용
- **git push origin \<branch>** : 커밋된 사항들을 원격에 push
- **git push --force origin main**: 원격 main 브랜치에 강제 push


###### **Delete**
- **git branch -d {브랜치 이름}** : 브랜치 삭제
- **git push origin --delete {브랜치 이름}** : 원격 저장소 브랜치 삭제

- 로컬에 저장되어 있는 원격 브랜치 목록은 한번에 삭제할 수 없기 때문에
```git
git branch -r | grep -v '\->' | awk '{print $1}' | xargs -n 1 git branch -d -r
```
	- git branch -r : 원격 브랜치 목록을 가져온다
	- grep -v '\->' : 심볼릭 브랜치(ex: origin/HEAD)를 제외한다.
	- awk '{print $1}' : 브랜치 이름만 추출한다.
	- xargs -n 1 git branch -d -r : 각 원격 브랜치 삭제
	- 
	위와 같은 명령어 조합을 사용해 한번에 삭제해야 한다. 
	-> 한 번 해봤을 때는 HEAD까지 모두 삭제됐음


###### **stash**
- **\*stash 된 내역은 브랜치와 상관 없이 전역에 존재한다**
- **git stash show -p stash@{0}** : stash 된 내용 보기
	- **git stash show -p stash@{0} -- <파일명>** : 특정 파일 보기
- **git stash list** : stash 되어 있는 목록 보기
- **git stash pop** : stash 된 모든 사항을 디렉토리로 복구한 뒤 stash 내역 삭제
	- git stash pop stash@{0/1/2/3~}
- **git stash apply** : stash 된 모든 사항을 디렉토리로 복구하고 stash 내역은 그대로 남겨둔다.
	- git stash apply stash@{0/1/2/3~}
- **git stash drop stash@{0, 1, 2, ~}** : 특정 stash 항목 삭제
- **git stash clear** : 모든 stash 항목 한번에 삭제

###### **git status** : 현재 stage할 내용이 있는지 확인

###### **git diff** 
- 수정된 파일들의 변경사항을 자세히 보여준다. 아직 스테이징 되지 않은 변경사항을 보여준다.
- **git diff --staged(--cached)** : 스테이징 된 변경사항과 마지막 커밋 사이의 차이를 보여준다.

###### **git log**
- 커밋 히스토리를 보여준다.
- **git log -p** : 각 커밋에서 변경된 내용을 보여준다.
- **git log --stat** : 각 커밋에서 변경된 파일의 목록과 각 파일에서 추가/삭제된 줄의 수를 보여준다.
- **git log --oneline --graph** : 깃 로그를 그림으로 보여준다

###### **git show {커밋 아이디}**
- 특정 커밋의 상세한 변경 사항을 보여준다 .


###### **Tag**
- **git tags -a {태그 이름} -m '설명'**
- **git push origin {태그 이름}**
- 태그를 push 할 경우, 브랜치를 push할 때와 같이 git push origin {tag name} 형식으로 태그 이름을 지정해서 push 해야 한다.