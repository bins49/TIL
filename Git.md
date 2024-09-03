#### repository와 commit

- repository는 각 버전별 프로젝트 모습을 볼 수 있으며 `.git directory`이  directory이다.
  - 커밋이 저장되는 곳이 repository이다.
- commit은 프로젝트 directory의 **특정 모습을 하나의 버전으로 남기는 행위 & 결과물을 말한다.**

- 이메일 주소 + 이름 설정하기

  - 처음으로 커밋을 하기 전 사용자의 이름과 이메일 주소를 설정해야 한다. 
  - `git config user.name "이름"`

  - ` git config user.email "이메일 주소"`

- commit은 기록하다는 뜻을 가졌다.

  - 항상 commit 할 때 커밋 메시지를 남겨야 한다.

  - `git commit -m "Create calculator.py and License"`
  - **항상 커밋할 파일을 미리 지정해줘야 한다**
    - `git add 수정되서 반영하고 싶은 파일`
  - **수정된 파일의 모습이 커밋에 포함될 것이라 지정하는것**

- Git은 내부적으로 크게 3가지 종류의 작업 영역을 두고 동작한다.
  - working directory
    - 작업을 하는 프로젝트 디렉토리 쉽게 말해, 내가 작업하고 있는 파일
  - staging area
    - git add를 한 파일들이 존재한 영역이다. 
    - 보통 commit을 하게 되면 좌측에 있는 staging area에 있는 파일들만 커밋에 반영된다.
    - 만약 이 과정이 없으면 내가 반영하고 싶은 것들만 선별적으로 커밋에 반영할 수 없다.
  - repository
    - working directory의 변경 이력들이 저장되어 있는 영역이다. 커밋들이 저장된 영역이다.

### git add

- git status는 깃이 인식하고 있는 프로젝트 디렉토리의 현재 상태를 보여준다. 
- 터미널에서 폴더를 만들려면 `mkdir`
- 터미널에서 파일을 만들려면 `touch`
- 한꺼번에 변경된 모든 파일이 staging area에 추가하려면 `git add .`를 하면 된다. 
- Git이 보는 파일의 4가지 상태
  - Git에서 파일들은 크게 다음 2가지 상태를 가진다
    - Untracked 상태
      - 파일을 새로 생성하고 한 번도 git add 해주지 않았다면 이 상태이다.
    - Tracked 상태
      - Staged 상태
        - 파일의 내용이 수정되고나서, staging area에 올라와있는 상태를 Staged상태라고 한다.
      - unmodified
        - 현재 파일의 내용이 최신 커밋의 모습과 전혀 바뀐 게 없는 상태
      - modified 
        - 최신 커밋과 비교했을 때 조금이라도 바뀐 내용이 있는 상태

- Staging area에서 파일 제거하려면  `git reset 파일 이름`을 하면 된다.
  - 변경된 새 모습은 그대로 working directory에 남아있다.
- Git 의미나 사용법을 좀더 자세히 알고 싶다면 `git help`를 치면 된다. 



#### Github

- github에서 만든 repository를 **원격 repository 혹은 remote repository**라고 부른다.
  - 내 컴퓨터의 레포지토리를 **local repository**라고 부른다.

- `git push -u origin master`
  - local repository의 master branch 내용을 remote repository의 master branch에 보내라는 뜻이다.
  - local repository의 내용을 remote repository에 반영하는 것을

- `git pull`은 **remote repository의 내용을 local repository에 반영하는 것을 말한다. **

- remote repository를 만드는 이유
  - 안전성
    - local 컴퓨터에 문제가 생겨도 다른 컴퓨터에서 remote repository를 가져올 수 있다.
  - 협업 가능
    - A개발자가 새로운 커밋을 한다면 내가 pull을 활용하고 내가 수정한 내용을 push해서 상대방이 pull을 통해서 받는다. 

- 아무나 나의 repository에 push하면 안 된다.
  - settings => Manage access => Invite a collaborator 이 과정을 통해서 함께 repository를 수정할 사람을 초대한다. 
  - 수정할 사람이 Accept invitation을 하게 되면 push를 할 수 있다.

- `git clone 주소`을 하면 repository를 복제할 수 있다.

- `git log`를 치면 commit history를 볼 수 있다.
  - history를 깔끔하게 보고 싶다면 `git log --pretty=oneline`
- commit한 내역을 자세히 보고 싶으면 `git show commitid 4글자 `

- -m 옵션 없이 git commit만으로 텍스트 에디터에 커밋 메시지 남기기
  - 터미널에서 git commit을 하고 `i`를 누르고 메시지 및 상세내용을 입력한다.
  - `ESC`누르고 `:wq`를 하면 된다.
  - 이렇게 하면 복잡하고 긴 커밋 메시지를 쉽게 남길 수 있다.

- 최신 커밋 수정하기 
  - `git commit --amend`
- 커밋 메시지 작성 가이드 라인
  - 커밋 메시지의 제목 뒤에 온점을 붙이지 마라
  - 커밋 메시지의 제목의 첫 번째 알파벳은 대문자로 작성해라
  - 커밋 메시지의 제목은 명령조로 작성해라.
    - `Fix it(o)`, `Fixed it(x)`, `Fixes it(x)`
  - 커밋의 상세 내용에는 이런 걸 적으면 좋다.
    - 왜 커밋을 했는지
    - 어떤 문제가 있었고
    - 적용한 해결책이 어떤 효과를 가지는지
  - 다른 사람들이 자신의 코드를 바로 이해할 수 있다고 가정하지 말고 최대한 친절하게 작성하자.
  - **하나의 커밋에는 하나의 수정사항, 하나의 이슈를 해결한 내용만 남기자.**
  - 현재 프로젝트 directory 상태가 그 내부의 전체 코드를 실행했을 때 에러가 발생하지 않는 상태인 경우에만 커밋을 하도록 하자. 나중에 동료 개발자가 특정 커밋의 코드로 실행했을 때 에러가 발생한다면 혼란을 줄 수 있다.
- git 옵션이 길 경우 커맨드 전체에 별명을 붙여서 그 별명을 사용할 수 있도록 하는 기능이 있다. aliasing이라고 한다.
  - `git config alias.history 'log --pretty=oneline'`