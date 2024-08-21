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