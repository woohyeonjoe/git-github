# git & github 사용법

## **git add. commit으로 파일을 기록해보자**

폴더 생성 → 해당 폴더 PowerShell → git 사용자 설정

`git config --global user.email "홍길동@naver.com"
git config --global user.name "홍길동"`

작업폴더에서 git 쓰고 싶으면 `git init`

파일 현재상태를 기록할려면 `git add 파일명` , `git commit -m '메모'`

작업폴더의 모든 파일을 전부 기록할려면 `git add .`

![image](https://user-images.githubusercontent.com/106286686/189537897-70ce0f55-e519-480f-b48f-1e48a92501d4.png)

상태창 `git status`

commit 내역 조회 `git log --all --oneline`

## **git add, commit, diff 사용법**

IDE에서 제공하는 commit 기능을 사용해도 됨.

이전 커밋 코드와 차이점 출력 `git diff` , `git diff 커밋id`, `git diff 커밋id1, 커밋id2`

비주얼적으로 이전 커밋 코드와 차이점 출력 `git difftool` , `git difftool 커밋id` , `git difftool 커밋id1 커밋id2`

프로젝트 중 새로운 기능을 추가할때 원본파일에 코드를 추가하면 위험.

→ 프로젝트 복사본을 만들어서 거기에 먼저 개발

→ branch기능을 이용해서 복사본 생성

## **git에서 branch 만들기**

**branch**

`git branch 브랜치명` : 브랜치 생성

`git switch 브랜치명` : 브랜치 이동, master가 메인 브랜치

**브랜치에서 작업한 코드가 성공적이어서 master브랜치와 합치고 싶다면..**

**merge**

=브랜치를 합치다

merge 하고 싶으면

1. master 브랜치로 다시 이동
2. git merge 브랜치명 입력하면 합쳐짐

`master 브랜치`와 `작업한 브랜치` 중 똑같은 부분의 코드를 수정하면 충돌 발생

→충돌 해결은 중복 코드 고치고 `git add & git commit`

## 다양한 merge 방법(3-way, fast-forward, squash, rebase)

**3-way-merge**

![image](https://user-images.githubusercontent.com/106286686/189537935-9f3a4453-e375-4881-bf68-d3b1b41ad05d.png)

**fast-forward merge**

가끔은 새로운 브랜치에만 commit 이 있고

기준이 되는 브랜치에는 신규 commit 이 없는 경우가 있다.

![image](https://user-images.githubusercontent.com/106286686/189537949-f695cd95-c2d7-4812-8bd5-7192c8f016e8.png)

그래서 "기준이 되는 브랜치에 신규 commit이 없으면" 자동으로 fast-forward merge가 발동된다.

**git merge --no-ff 브랜치명** 해서 강제로 3-way merge 할 수도 있다.

**merge 완료된 브랜치 삭제는**

`git branch -d 브랜치명`

**merge 안한 브랜치 삭제는**

`git branch -D 브랜치명`

**rebase and merge**

브랜치를 rebase 하고 나서 merge 하는 것도 가능하다.

![image](https://user-images.githubusercontent.com/106286686/189537966-ddbf75cc-d571-4e63-bc87-b767501e794c.png)

rebase는 브랜치의 시작점을 다른 commit으로 옮겨주는 행위이다.

1. rebase를 이용해서 신규브랜치의 시작점을 main 브랜치 최근 commit으로 옮긴 다음
2. fast-forward merge하는 것이다.

이런 식으로도 브랜치 합치기가 가능.

사용 이유

1. 3-way merge 말고 강제로 fast-forward 하고 싶을 때
2. 브랜치 없이 바로 코드 작성하고 싶을때

그러고 싶으면 일반 3-way merge 대신 rebase & merge 해도 됨.

그래서 실제로 rebase and merge 하고 싶으면

1. 새로운 브랜치로 먼저 이동해서
2. git rebase main 하면 된다.
3. 그럼 브랜치가 main 브랜치 끝으로 이동하는데 그걸 fast-forward merge 하면 됨.

`git switch 새로운브랜치`
`git rebase main`

`git switch main`
`git merge 새로운브랜치`

차례로 입력하면 rebase 끝입니다.

rebase & merge를 한 줄로 쉽게 비유하자면 **강제 fast-forward merge**입니다.

**squash and merge**

대충 모든 브랜치를 3-way merge 해버리면 나중에 참사가 일어날 수 있다.

(1) 3-way merge 된 것들은 매우 복잡해보임

(2) main 브랜치 git log 출력해보면 3-way merge된 브랜치들의 commit 내역도 다 같이 출력되어서 더러워짐

![image](https://user-images.githubusercontent.com/106286686/189537988-729d47e0-6359-44c6-a501-d57d66e12c5f.png)

3-way merge처럼 선으로 이어주지 않고

새 브랜치에 있던 코드변경사항들이 **main 브랜치로** **텔레포트 한다**.

그럼 이제 main 브랜치의 git log 출력해볼 때

merge 완료된 브랜치의 commit 같은 것들은 출력되지 않습니다.

`git switch main`
`git merge --squash 브랜치명`

`git commit -m '메세지'`

squash and merge 하는 법은 그냥 --squash 옵션을 추가하면 끝.

브랜치에서 만들어놨던 많은 commit 을 다 합쳐서 하나의 commit으로 main 브랜치에 생성해줍니다.

## **코드짜다가 실수했다 되돌아가자(git revert, reset, restore)**

`git restore 파일명` : 해당 파일을 이전 커밋 상태로 복구

`git restore --source 커밋아이디 파일명` : 입력한 파일이 특정 커밋아이디 시점으로 복구

`git restore --staged c` : 이건 복구랑 상관없지만 이러면 특정 파일을 staging 취소

`git revert 커밋아이디` : commit 취소

`git rever HEAD` : 최근 commit 취소

사용주의

`git reset --hard 커밋아이디` : 과거로 모든걸 되돌리기

`git reset —soft 커밋아이디` : 리셋인데 변동사항 지우지 말고 스테이징해놓기

`git reset --mixed 커밋아이디` : 리셋인데 변동사항 지우지 말고 unstage해놓기

## Github 사용법 1. 내 코드 올릴 땐 git push

git이 파일 기록해두는 장소 : repository

**원격저장소를 만들어서 올려보자**

1. new repository 만들고 아무런 파일(readme.md)을 추가하지 않는다.
2. 프로젝트에서 `git init` 
3. `git branch -M main` 명령어로 기본브랜치 이름 main으로 변경 → 깃허브는 main이 강제적
4. `git push -u 원격저장소주소 main` 원격저장소 업로드 (주소는 해당 레포지토리 주소 뒤에 .git)

원격저장소 주소를 변수로 설정해두면 추후 사용이 편해짐

git remote add origin [https://github.com/woohyeonjoe/git-github.git](https://github.com/woohyeonjoe/git-github.git)

**git push에 -u 추가하면 주소 기억하라는 뜻**

## Github 사용법 2. 타인과 협업하기 (git clone, pull)

원격저장소 복제는 `git clone 저장소주소` → 처음 프로젝트 받을때

**프로젝트 공동작업자에 팀원을 넣어줘야함**

원격저장소에 새로운게 생기면 `git push` 못함 → `git pull` 사용해서 업데이트된 코드 받아내고 `git push` 

`git pull [원격저장소주소] [원격 저장소에서 받아오고자 하는 브랜치 이름]`

`git pull` 은 `git fetch` + `git merge` 다.

`git feth`: 원격저장소 신규 commit 가져오세요

`git merge` : 내 브랜치에 merge

그러므로 충돌(conflict) 발생가능

## Github 사용법 3. 브랜치로 협업하기 (pull request)

원격 repository (저장소) 에도 브랜치를 만들 수 있습니다.

브랜치 생성하려면 

1. github.com에서 브랜치 직접 만들어도 되고 
2. 아니면 로컬에서 만든 브랜치를 올려도 브랜치생성이 가능합니다.

> Pull request 하기
> 

브랜치만들면 뭐합니까 그걸 main 브랜치와 합쳐야 기능이 완성되지 않겠습니까.

합치려면 git merge 명령어로 합치면 끝입니다. 그리고 git push 하면 끝인데

팀끼리 일하는 경우 merge 하기 전에 토론하거나 검토하거나 그래야하는 경우가 많습니다.

그래서 github.com은 pull request 라는 기능이 있습니다.

그냥 쉬운 말로 merge request입니다.

협업자가 브랜치를 push하고 웹사이트에서 merge하는 법

1. `Pull request` 버튼이 merge 요청임
2. `New pull request`
3. `Create pull request`
4. `Merge pull request`

> *자료 출처 [매우쉽게 알려주는 git & github](https://codingapple.com/course/git-and-github/)
