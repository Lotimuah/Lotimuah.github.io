---
layout: article
title: Github basic
tags: Github
#comments: true
#article_header:
#  type: cover
#  image:
#    src:
aside:
  toc: true
key: page-aside
---

  \* Customize할 부분은 []로 표현했습니다.

### 0. Git install

    $ sudo apt-get install git        // for Ubuntu

&nbsp;&nbsp;&nbsp;다른 OS의 경우 [Git Downloads](https://git-scm.com/downloads) 참고

### 1. Git setup

    $ git config --global user.name "[your name]"
    $ git config --global user.email "[your email]"

    // user.name, user.email check
    $ git config --list

    // if you want to delete this setup
    $ git config --unset user.name
    $ git config --unset user.email

### 2. Create repository

&nbsp;&nbsp;&nbsp;[github](https://github.com) 가입 후 repository 생성


### 3. Git repository sync

    // 기존 프로젝트가 없는 경우 clone
    $ cd ~/[your path]                              // 원하는 dir로 이동
    $ git clone [repository URL]


    // 기존 프로젝트가 있는 경우 remote & push
    $ cd ~/[your path]                              // 원하는 dir로 이동
    $ git init
    $ git branch -M main                            // branch rename (master -> main)
    $ git remote add [name] [repository URL]        // name : remote repo name, 관례적으로 origin 사용
    $ git remote -v                                 // remote check

### 4. Git commit, push

    $ git add .
    $ git status                                    // staged check
    $ git commit -m "[message]"
    $ git push -u [name] main                       // main branch에 push
                                                    // name : origin 사용
                                                    // -u option으로 이후 git push만으로 push 가능
### 5. Git Pull

    $ git pull -u origin main
    $ git pull

### 6. Git history check

    $ git log
    $ git log --oneline --graph

### 7. Git revert, reset
    $ git log --oneline --graph
    * 40a0b91b (HEAD -> master) commit1
    * cb9d5670 commit0

&nbsp;&nbsp;&nbsp;이와 같은 상황에서 commit1을 없던 것으로 하고 싶은 경우 사용할 수 있는 방법이 2가지 존재

    // revert : 되돌린 기록이 남음
    $ git revert 40a0b91b                             // 40a0b91b 를 되돌림
    $ git log --oneline --graph
    * bb292f50 (HEAD -> master) Revert "commit1"
    * 40a0b91b (origin/master, origin/HEAD) commit1
    * cb9d5670 commit0
    $ git push

    // reset : 되돌린 기록을 제거
    $ git reset --hard  cb9d5670                      //  cb9d5670 로 돌아가고 싶음
    $ git log --oneline --graph
    * cb9d5670 commit0
    $ git push -f                                     // reset의 경우 force(-f) option 필요


### 8. Git branch, checkout

    $ git branch                                    // list local branches
    $ git branch -r                                 // list remote branches
    $ git branch -a                                 // list all branches
    $ git branch [branch_name]                      // generate new branch
    $ git branch -M [branch_name]                   // force rename
    $ git branch -D [branch_name]                   // force delete
    $ git checkout [branch_name]                    // change working branch
    $ git checkout -b [branch_name]                 // generate new branch & checkout
    $ git checkout -t origin/[branch_name]          // get branch from remote repo & checkout
    $ git checkout -b [commit_hash]                 // generate new branch with [commit_hash] & checkout
