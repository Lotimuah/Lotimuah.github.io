---
layout: article
title:
tags: Ubuntu ssh X11 forwarding
#comments: true
#article_header:
#  type: cover
#  image:
#    src:
aside:
  toc: true
key: page-aside
---

  \* Ubuntu 18.04 GUI Server 설정 가이드

  OpenAI Gym 강화학습 실험을 하다보면 시뮬레이션 결과를 rendering할 필요가 있는데 이를 위해 Xwindow를 활용할 수 있습니다. 이를 이용하면 Ubuntu GPU server에 접속해 원격으로 GUI를 사용할 수 있습니다.


## 1. Server에서 Ubuntu GUI 패키지 설치

    $ sudo apt-get update
    $ sudo apt-get install -y ubuntu-desktop xorg xrdp xserver-xorg mesa-utils xauth gdm3
    // display manager 팝업 시, gdm3를 선택하고 enter
    $ dpkg-reconfigure xserver-xorg

## 2. NVIDIA 그래픽 카드 환경 설정 로드

    $ nvidia-xconfig    // /etc/X11/xorg.conf 파일이 생성되었는지 확인

## 3. X11 server mode 설정

    // /etc/ssh/sshd_config 파일을 열어서 다음 항목들을 수정
    X11Forwarding yes
    X11DisplayOffset 10
    X11UseLocalHost no    // 추가 삽입
    UseLogin no           // 주석 해제

## 4. X11 session 인증 기록파일 생성 및 권한 조정

    $ touch /root/.Xauthority   // chmod 600

## 5. /etc/hosts 파일에 정보 추가

    // xauth display session 정보를 찾아가기 위해 default 정보 추가
    127.0.0.1 localhost

## 6. Reboot

    $ reboot

## 7. Client에서 X11 forwarding 접솝을 위한 프로그램으로 Xming 설치 및 실행

    (1) Xming 다운로드 [Xming download](https://sourceforge.net/projects/xming/)

    (2) Xming 설치 및 실행

    (3) 원격 접속 프로그램 setting (ex: Putty)

<p align="center"><img src="https://github.com/LoteeYoon/LoteeYoon.github.io/blob/master/putty.png?raw=true"></p>
    (4) server로 ssh 접속

## 8. X11 display forwarding 체크

    $ xdpyinfo      // display info가 나오면 정상

## 9. Test

    $ xclock

<p align="center"><img src="https://github.com/LoteeYoon/LoteeYoon.github.io/blob/master/xclock.JPG?raw=true"></p>
