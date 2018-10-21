# Git: 첫번째 풀 리퀘스트
## 목차
1. 시작하기
2. Git init, git add, git commit, git push
3. Remotes, branches
4. 커밋 그래프의 변화
5. 포크, 풀 리퀘스트

## 1. 시작하기
시작하기에 앞서, 내 로컬 컴퓨터에 Git이 깔려 있어야 합니다.
[공식 홈페이지에서 실행 파일을 다운받아](https://git-scm.com/downloads) 설치하셔도 되고,
리눅스 사용자의 경우 내장 패키지 매니저인 apt-get 등으로 직접 설치할 수도 있습니다.
```bash
# 우분투 계열 리눅스
$ sudo apt-get install git 

# RPM 계열 리눅스(페도라, CentOS)
$ sudo yum install git

# MacOS는, 패키지 매니저인 brew가 먼저 깔려있어야 합니다.
$ brew install git 
```
Sourcetree 등 커밋들을 관리하기 쉽도록 GUI를 제공하는 여러 Git 클라이언트 툴이 많이 있지만, 깃을 처음 사용하시는 분들에게는 커맨드 라인 인터페이스(CLI)로 
깃을 사용해 보실 것을 권장합니다. 만약 윈도우 사용자라면, 설치 이후에 Git bash를 실행하여 CLI git을 사용할 수 있습니다.
([how-to-install-git-bash-on-windows](http://www.techoism.com/how-to-install-git-bash-on-windows/)에서 더 자세한 내용을 확인할 수 있습니다)  
마지막으로 로컬 컴퓨터에서, ```git``` 커맨드가 잘 작동하는지 확인해 주세요.
```bash
$ git --help
usage: git [--version] [--help] [-C <path>] [-c <name>=<value>]
           [--exec-path[=<path>]] [--html-path] [--man-path] [--info-path]
           [-p | --paginate | --no-pager] [--no-replace-objects] [--bare]
           [--git-dir=<path>] [--work-tree=<path>] [--namespace=<name>]
           <command> [<args>]
            ...
            ...
$ git --version
git version 2.17.0
```

## 2. Git init, git add, git commit, git push
### Git init
1. 먼저 빈 폴더를 하나 생성해 봅시다. 이름은 `myfolder/`라고 하겠습니다.
2. `myfolder/` 안으로 들어간 뒤,
3. `git init`명령어로 이 폴더를 Git repository로 만듭니다. 이제 이 레포지토리는 우리가 **local repository**라고 부르기로 하겠습니다.


```bash
$ git init
Initialized empty Git repository in /Users/{your_path_to_myfolder}/myfolder/.git/

$ ls -al
total 0
drwxr-xr-x   3 sangsulee  staff   96 Oct 16 10:22 .
drwxr-xr-x   3 sangsulee  staff   96 Oct 16 10:04 ..
drwxr-xr-x  10 sangsulee  staff  320 Oct 16 10:22 .git/
```
`git init` 명령어를 실행시키면 `.git/` 디렉토리가 새로 생성된 것을 확인할 수 있는데요, Git은
이 `.git/` 폴더 안에 있는 파일들로부터 해당 레포지토리의 파일 변화, 커밋 이력들을 모두 추적할 수 있게 됩니다.
또한 사용자 설정 값들도 저장되어 있습니다. 그러므로 직접적으로 이 숨겨진 `.git/`디렉토리를 수정하거나 삭제하는 것은 되도록이면 피해야 합니다.

### Git status
이제 이 **local repository** 안에 빈 파일들을 몇개 만들어 보겠습니다. 참고로 **local repository**와 
**remote repository**라는 용어를 따로 구분했는데요, 뒤에 나올 내용에서 더 자세히 다루도록 하겠습니다.

```bash
# 빈 파일 2개 생성
$ touch haha.txt hoho.txt 
```

그러면 디렉토리 구조는 이렇게 보이게 됩니다:
```bash
# Directory tree
─── myfolder
    ├── .git/
    ├── haha.txt
    └── hoho.txt
```

`git init`커맨드로 처음에 레포지토리가 초기화되면 그 이후의 모든 레포지토리 안 변경사항들을
Git 시스템이 추적하게 됩니다. 커맨드 이름에서 알 수 있듯이, `git status`로 어떤 파일이 추가되었고 어떤 파일이 수정/삭제되었는지
그 변경사항을 확인할 수 있습니다
```bash
$ git status
On branch master

No commits yet

Untracked files:
  (use "git add <file>..." to include in what will be committed)

        haha.txt
        hoho.txt

nothing added to commit but untracked files present (use "git add" to track)
```
위에서 추가한 대로, 2개의 **Untracked**인 새로운 파일을 인식한 것을 볼 수 있습니다.

### Git add
`git add` 커맨드로 파일들의 변경사항들 중 일부, 혹은 전체를 **stage**시킬 수 있으며, staging된 변경사항들만
커밋될 수 있습니다.

```bash
# 다음 커밋의 스테이지로 haha.txt를 add
$ git add haha.txt

# git status 확인
$ git status
On branch master

No commits yet

Changes to be committed:
  (use "git rm --cached <file>..." to unstage)

        new file:   haha.txt

Untracked files:
  (use "git add <file>..." to include in what will be committed)

        hoho.txt
```
이렇게 하나의 파일만 staging할 수도 있고, 또는 모든 파일을 `git add` 명령어의 `-A`옵션을 사용해
한번에 staging할 수도 있습니다.

### Git commit
- `git commit` 커맨드는 모든 스테이징된 변경사항들을 **저장**합니다.
- Git commit은 Git workflow의 기본 단위입니다.
- Staged된 변경사항들은 Commit 후 최종적으로 push되기 이전까지 local repository 밖으로 노출되지 않습니다.
- 커밋은 변경사항들과 함께 커밋 메시지를 같이 담고 있습니다.
- 한번 커밋된 변경사항은 다시 되돌릴 수 있으나, 아주 쉬운 일은 아닙니다.
- 커밋 그래프에서 시간순으로 정렬된 노드 하나가 하나의 커밋을 의미합니다.
```bash
$ git add -A
$ git status
On branch master

No commits yet

Changes to be committed:
  (use "git rm --cached <file>..." to unstage)

        new file:   haha.txt
        new file:   hoho.txt


$ git commit -m "Add haha, hoho"
[master (root-commit) 8167612] Add haha, hoho
 2 files changed, 0 insertions(+), 0 deletions(-)
 create mode 100644 haha.txt
 create mode 100644 hoho.txt
```
위처럼 `-m`옵션을 주어서 커맨드 라인에서 바로 커밋 메시지를 줄 수도 있습니다. 옵션이 주어지지 않으면
기본 설정된 텍스트 에디터로 메시지를 입력받게 됩니다.

### Git log
커밋 로그 그래프를 시각적으로 확인하려면:
```bash
# https://stackoverflow.com/questions/1057564/pretty-git-branch-graphs
$ git log --all --decorate --oneline --graph
* 8167612 (HEAD -> master) Add haha, hoho
(END)
```  

### Git push
이제 다른 레포지토리에 커밋을 push할 준비가 되었습니다.
push할 타겟 레포지토리의 URL은 개인 git server가 될 수도 있고, 공개/비공개 깃허브 레포지토리가 될 수 있고,
아니면 다른 git server가 될 수 있습니다.
이어지는 예시를 위해 Github.com에 아직 가입하지 않으셨다면, 가입하시고 새로운 레포지토리를 만들어 주세요.
저는 'myrepo'라는 이름으로 만들어서 진행하였고, 이름은 크게 상관이 없습니다. 저는 위에서 했던 이 로컬 커밋을 이제 
방금 만든 깃허브 **remote repository**에 push하려 합니다.


```bash
# 먼저 'myrepo'라는 임의의 이름으로 remote를 추가합니다. url은 방금 만든 github repository의 url을 가르키도록 합니다.
$ git remote add myrepo https://github.com/{username}/{your-repo-name}

# 이제 로컬 commit을 해당 remote repository에 push합니다!
$ git push myrepo master
```
이제 github repository에서 커밋이 적용되었는지 확인해보도록 해요!

## Remotes, Branches(WIP)
- `origin`, is the name of the default **remote** that pointing the repository url you've cloned from. 
- `master`, is the name of the **branch** in the git repository.
## How the graph is changing over commits(WIP)
## Fork, Pull Request(WIP)





