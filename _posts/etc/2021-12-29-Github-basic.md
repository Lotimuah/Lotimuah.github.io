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


### 0. Git install

    $ sudo apt-get install git        // for Ubuntu

다른 OS의 경우 [Git Downloads](https://git-scm.com/downloads) 참고

### 1. Git setup

    $ git config --global user.name "your name"
    $ git config --global user.email "your email"

    // user.name, user.email check
    $ git config --list
    
    // if you want to delete this setup
    $ git config --unset user.name
    $ git config --unset user.email

### 2. Git clone

    $ cd ~/your path          // 원하는 dir로 이동
    $ git clone [repository URL you want to clone]

### 3.
