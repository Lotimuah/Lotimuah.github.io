---
layout: article
title: Linux Setting
tags: Linux
#comments: true
#article_header:
#  type: cover
#  image:
#    src:
aside:
  toc: true
key: page-aside
---

  \* Ubuntu 18.04 install 이후 기본적인 setting

---------------------------------------------------------


### 1. Network connection

![png](/ubuntu_network.PNG)

### 2. root permission set

    $ sudo passwd


### 3. 작업 표시줄 BOTTOM 설정

    $ gsettings set org.gnome.shell.extensions.dash-to-dock dock-position BOTTOM

### 4. package install

    $ sudo apt-get update && sudo apt-get upgrade
    $ sudo apt-get install vim
    $ sudo apt-get install ssh
    $ sudo apt-get install net-tools
    $ sudo apt-get install git
    $ sudo apt-get install htop
