---
layout: article
title: Ubuntu SSH X11 forwarding video rendering
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

  \* 이전 포스팅 [Ubuntu SSH X11 forwarding](https://Lotimuah.github.io/2022/01/22/Ubuntu-ssh-X11-forwarding.html)에서는 GUI 환경이 없는 server에 접속하여 X11 forwarding으로 client에서 GUI를 원격으로 사용할 수 있게 했습니다. 이번 포스팅은 video file을 play해볼 것입니다.

---------------------------------------------------------

## 1. play video using VLC

    // VLC 설치
    $ sudo apt-get update
    $ sudo apt-get install vlc browser-plugin-vlc
    $ sudo apt-get install libav-tools
    // mp4 파일 실행
    $ vlc <movie_file>

<p align="center"><img src="https://github.com/Lotimuah/Lotimuah.github.io/blob/master/vlc.JPG?raw=true"></p>

  vlc를 통해 영상을 play하니 창이 뜨긴 하지만 영상이 나오진 않았습니다.

## 2. play video using CVLC

  구글링을 통해 cvlc를 사용해봤습니다.

    $ echo $DISPLAY
        nipa2019-0137:10.0

    $ cvlc --x11-display :10.0 <movie_file>

<p align="center"><img src="https://github.com/Lotimuah/Lotimuah.github.io/blob/master/cvlc.JPG?raw=true"></p>

  이 또한 아무것도 출력이 되지 않았습니다.

## 3. play video using mplayer

  이번엔 mplayer를 사용해봤습니다.

    $ sudo apt-get install mplayer
    $ mplayer -vo caca <movie_file>

<p align="center"><img src="https://github.com/Lotimuah/Lotimuah.github.io/blob/master/mplayer.JPG?raw=true"></p>

  mplayer의 경우 ASCII 문자를 통해 display를 하는 것 같습니다.

## 4. play video using xdg-open

  마지막으로 xdg-open을 사용해봤습니다.

    $ xdg-open <movie_file>

<p align="center"><img src="https://github.com/Lotimuah/Lotimuah.github.io/blob/master/xdgopen.JPG?raw=true"></p>

  이번엔 제대로 영상이 나오는 걸 확인할 수 있었습니다. 다만 영상의 재생 속도가 너무 느렸습니다. 무슨 이유인지는 정확히 모르겠으나 ssh를 통해 영상을 전달하는 것에 overhead가 있는 것 같습니다.
