---
layout: article
title: Github basic
tags: etc
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

&nbsp;&nbsp;다른 OS의 경우 [Git Downloads](https://git-scm.com/downloads) 참고

### 1. Git setup

    $ git config --global user.name "[your name]"
    $ git config --global user.email "[your email]"

    // user.name, user.email check
    $ git config --list

    // if you want to delete this setup
    $ git config --unset user.name
    $ git config --unset user.email

### 2. Create repository

[github](https://github.com) 가입 후 repository 생성


### 3. Git repository sync

    // 기존 프로젝트가 없는 경우 clone
    $ cd ~/[your path]                              // 원하는 dir로 이동
    $ git clone [repository URL]

    // 기존 프로젝트가 있는 경우 remote & push
    $ cd ~/[your path]                              // 원하는 dir로 이동
    $ git init
    $ git branch -M main
    $ git remote add [name] [repository URL]        // name : 관례적으로 origin 사용
    $ git remote -v                                 // remote check

### 4.  
