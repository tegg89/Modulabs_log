# Git: Guide to your first pull request
## Table of contents
1. Getting started  
2. Init, Add, Commit, Push  
3. Remotes, branches
4. How the graph is changing over commits
5. Fork, pull request

## 1. Getting started
First thing first, we need Git installed on our local machine. This can be simply 
done by  
[executing git install runfile on your local machine from the official website](https://git-scm.com/downloads),  
or you can just directly install the package using builtin package manager if you're running linux-based OS:
```bash
# Ubuntu distro
$ sudo apt-get install git 

# RPM distio(Fedora, CentOS ..)
$ sudo yum install git

# In MacOS, brew has to be installed first.
$ brew install git 
```
There are many different GUI-based client tools out there for committing and browsing git commits, but I strongly recommend beginners to start with 
the commnad-line interface that comes with git scm.  
If you're using Windows OS, you'll already have **git bash** executable after the installation is done.  
(See [how-to-install-git-bash-on-windows](http://www.techoism.com/how-to-install-git-bash-on-windows/) for more details.)  
Lastly, ensure that the ```git``` command works fine on your local machine:
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
Nice, now we're all set! 

## 2. Init, Add, Commit, Push
### Git init
1. First let's make an empty directory, `myfolder/`  
2. cd into `myfolder/`,  
3. then we'll initialize this empty directory as a git repository, by typing `git init` command.
Now we'll call this git initialized directory as **local repository**

```bash
$ git init
Initialized empty Git repository in /Users/{your_path_to_myfolder}/myfolder/.git/

$ ls -al
total 0
drwxr-xr-x   3 sangsulee  staff   96 Oct 16 10:22 .
drwxr-xr-x   3 sangsulee  staff   96 Oct 16 10:04 ..
drwxr-xr-x  10 sangsulee  staff  320 Oct 16 10:22 .git/
```
We can notice that `.git/` directory has been created once we type the command `git init`. Git tracks all version histories 
of the local repository such as file changes, all commits done, or get other important user configurations
from the files reside in this`.git/` folder. Hence, it's not a really good idea to remove/modify them manually :).

### Git status
Now let's make some dummy files in our **local repository**.  
As a side note, we distinguishes the terms **local repositoy** and **remote repository**. We'll cover them again in later section.

```bash
# Create empty files
$ touch haha.txt hoho.txt 
```

Then we have directory tree looks like this:
```bash
# Directory tree
─── myfolder
    ├── .git/
    ├── haha.txt
    └── hoho.txt
```

Once the directory is initialized by `git init` command, all the file changes in the repository 
will be tracked by git system. As the command suggests, `git status` 
shows those changes of which file has been newly added, or changed(deleted) within the repository. Command results are shown as follows:
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
Git system recognized two **untracked** files that we added before.

### Git add
Using `git add` command, current changes in directory can be partially, or entirly **staged** for commit.   
Let's say we only want haha.txt file to be staged:
```bash
# Add haha.txt to be staged for next commit
$ git add haha.txt

# Check status
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
Of course we can stage all the changes we've made at once, by giving `-A` option to `git add` command.

### Git commit
- `git commit` command saves all staged changes. 
- Git commit is a fundamental, unit component of the git workflow
- Staged chages will not be revealed to outside of local repository until it is committed and finally pushed to public repository.  
- Each commit contains the changes of the content, along with user's log message.  
- Once committed, it is normally not easy to revert a commit.  
- Commit is usually be represented as a single node in graph representation that flows in time order. 
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
We can specify our commit message inline with `-m` option like above, otherwise the default text editor will popped up for the commit message.  

### Git log
To check the commit log graph visually in command line, try:
```bash
# https://stackoverflow.com/questions/1057564/pretty-git-branch-graphs
$ git log --all --decorate --oneline --graph
* 8167612 (HEAD -> master) Add haha, hoho
(END)
```

### Git push
Now we have the commits ready to be synchronized with other repository.  
target repository URL could be your own git server, or public/private github repository, or could be other URL that serves git.  
For this example, If you haven't sign up for Github yet, please do, then create an emtpy repository **without** the README.md file.
I just have created mine and named it 'myrepo'. Any name would be fine, we'll just push our local changes to this **remote repository**.


```bash
# First add the remote 'myrepo' and make it point to your github repository.
$ git remote add myrepo https://github.com/{username}/{your-repo-name}

# Now push local commit(s) to remote repository.
$ git push myrepo master
```
That's it for this section! check your github repository to see if your local changes are applied. 

## 3. Remotes, Branches
### Remotes
If we `git clone` the project from the existing git repository url(usually ends with \*.git),
```bash
$ git clone https://github.com/2sang/myrepo.git
Cloning into 'myrepo'...
remote: Enumerating objects: 9, done.
remote: Counting objects: 100% (9/9), done.
remote: Compressing objects: 100% (3/3), done.
remote: Total 9 (delta 3), reused 6 (delta 4), pack-reused 0
Unpacking objects: 100% (9/9), done.

$ cd myrepo
$ git remote -v
origin  https://github.com/2sang/myrepo (fetch)
origin  https://github.com/2sang/myrepo (push)
```
As shown above, We can see current list of remotes using `git remote -v`, and see
the remote `origin` is set to the repository we've just cloned from.  
Just like we have gone through from the scratch with `git init` command, **local repository 
cannot have any remotes unless we explicitly set them with their name.**
However, if we clone the repository directly from remote repository, git sets local 
repository's default remote to `origin`, with URL of that repository.  


Notice that we can have multiple remotes each with different names. This is actually a quite common situation 
to having them in a single local repository when you're working with same repositories with different versions, and vice versa.
```bash
# git remote add <remote_name> <remote_url>
$ git remote add real_origin https://github.com/EpisysScience/myrepo

# Now we have two remotes, 2sang/myrepo as 'origin' and EpisysScience/myrepo named as real_origin
$ git remote -v
origin  https://github.com/2sang/myrepo (fetch)
origin  https://github.com/2sang/myrepo (push)
real_origin  https://github.com/EpisysScience/myrepo (fetch)
real_origin  https://github.com/EpisysScience/myrepo (push)
```
Things are getting a little more complicated if we have multiple remotes in multiple branches, but no worries, we'll get it soon.  
#### Summary - remotes
- `remote` represents a remote repository's name with its URL.
- `origin`, is the name of the default **remote repository** which 
points to the repository url you've cloned from. 
- when you first `git init` from directory, you should explicitly set the remote in order
to synchronize local changes with the remote repository.
### Branches
> "Nearly every VCS has some form of branching support. 
Branching means you diverge from the main line of development 
and continue to do work without messing with that main line.", - from Gitbook, Git branching  

In Git, the command `branch` plays a really important role. It **logically branches the flow of commits** 
into multiple commit streams, without messing them one another.  
First, see how branches and remotes are shown in commit graph.
```bash
$ git log --oneline --decorate --all --graph
* bd0492e - Wed, 17 Oct 2018 23:29:43 +0900 (5 seconds ago) (HEAD -> master)
|           second commit - 2sang
* 9c1d848 - Wed, 17 Oct 2018 23:29:18 +0900 (30 seconds ago)
            Init - 2sang
```
It has two commits in single graph and each has a unique hash ID: `bd0492e`, `9c1d848` along with the commit timestamp. 
We can see there's one more `(HEAD -> master)`, `HEAD` is **a reference to the last commit in the current branch that you're in**,
and it is pointing to `master` branch. Though we have the only one branch here, you can easily find there are a number of branches in 
other popular git repositories.

#### Create a new branch
To create a new branch, 
```bash
# Give branch name after the command
$ git branch <new_brach_name>

# Or, use checkout command with option '-b'. This instantly switches to new branch with the creation.
$ git checkout -b <new_branch_name>
```
To list all branches, use '--all' option.
```bash
$ git branch new_branch

$ git branch --all # or '-a'
* master
  new_branch
  remotes/origin/master -> origin/master
```

#### Checkout to other branch
To switch from current working branch to another, 
```bash
$ git checkout <branch_name>
```
**Make sure all local changes are committed before switching to other branch**, otherwise git will refuse to checkout.
#### Summary - branches

- `HEAD` points to the last commit in the current branch.
- `master`, is the name of the default **branch** in the git local/remote repository when you first initialize the git repository. 
- To create a branch, `git branch <new_branch_name>`, or `git checkout -b <new_branch_name>`.
- To switch the branch, `git checkout <branch_name>`.

## 4. How the graph is changing over multiple commits(WIP)
So far we've covered a few basic git commands and its usage, and now let's see how the 
commit graph is changing with some examples.  

#### Fixing urgent bug
Let's suppose that we have a small web service currently running on the server, and we have found
some minor warnings keep on being reported on our log system. It turned out we still have a few lines of code remaining that should have been deleted in last release.  

<img src="assets/git_case1-1.png" alt="drawing" width="200" height="200"/>  

To keep the `master` branch from changing while we're fixing the issue, we decided to switch our working environment to new branch `hotfix`, out from the master branch.
```bash
$ git branch hotfix-152
$ git checkout hotfix-152
Switched to branch 'hotfix'

# And after the fixing done, add and commit the changes
$ git add <fixed_files>
$ git commit -m "Fix issue-152"
```
Now, local commit graph will look like this:  

<img src="assets/git_case1-2.png" alt="drawing" width="500"/>  

After we verified that `hotfix` branch has no failures in all testsets, 
our team decided to merge `hotfix` branch to `master`:
```bash
$ git checkout master
# Merge 'hotfix' from 'master'
$ git merge hotfix
Updating 4ec0dc1..16eb0c4
Fast-forward
 fixed_file_1.cpp | 5
 1 file changed, 0 insertions(+), 5 deletions(-)
 create mode 100644 cc
```
Git does a **Fast-forward** when you merge a branch that is ahead of your switched branch, just like this case.
<img src="assets/git_ff.png" alt="drawing" width="500"/>
This briefly shows how fast-forward merge works. [(Image from bitbucket tutorial)](https://confluence.atlassian.com/bitbucket/git-fast-forwards-and-branch-management-329977726.html)  

Note that **`git merge` command should be invoked from the base branch**, where you want to pull the other branch in. In this case, we switched to `master` branch to merge `hotfix`.  

Our final commit graph after the merging:
<img src="assets/git_case1-3.png" alt="drawing" width="500"/>  

Notice that the `master` branch has moved from the last release to hotfix(technically, master merged hotfix, right?), and `hotfix` branch still remains after the merger. To delete the branch,
```
$ git branch --delete hotfix
Deleted branch hotfix (was 16eb0c4).
```

## 5. Fork, Pull Request(WIP)
## 6. Miscellaneous
Prettifying git logs:
```bash
# Make alias of frequently used log command with prettify options, consider adding this line at your .*rc file.
# To see more aliases like this, see https://gist.github.com/pksunkara/988716
$ alias glg="git log --graph --abbrev-commit --decorate --format=format:'%C(bold blue)%h%C(reset) - %C(bold cyan)%aD%C(reset) %C(bold green)(%ar)%C(reset)%C(bold yellow)%d%C(reset)%n''          %C(white)%s%C(reset) %C(dim white)- %an%C(reset)' --all"

$ glg
* 33d3b34 - Wed, 17 Oct 2018 22:31:40 +0900 (45 minutes ago) (HEAD -> 2sang, 2sang-repo/2sang)
|           Working on section 4, 5 in git tutorial - 2sang
*   52d2fcc - Tue, 16 Oct 2018 21:41:05 +0900 (26 hours ago) (origin/master, origin/HEAD)
|\            Merge pull request #1 from 2sang/2sang - Sangsu Lee
| * a9d6aa8 - Tue, 16 Oct 2018 21:31:52 +0900 (26 hours ago)
| |           Add git tutorial - WIP - 2sang
| * 90bc340 - Tue, 16 Oct 2018 15:13:15 +0900 (32 hours ago)
| |           Add Git tutorial - 2sang
| * 92f12b3 - Tue, 16 Oct 2018 08:59:55 +0900 (2 days ago)
| |           fix grammar - 2sang
| * 01e7fa9 - Mon, 15 Oct 2018 23:43:30 +0900 (2 days ago)
|/            Add draft - 2sang
* df83a50 - Mon, 15 Oct 2018 18:03:52 +0900 (2 days ago) (2sang-repo/master, master)
|           Move prerequisite contents to Guide.md - 2sang
```
