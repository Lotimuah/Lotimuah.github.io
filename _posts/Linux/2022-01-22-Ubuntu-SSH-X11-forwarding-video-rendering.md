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

  \* 이전 포스팅 [Ubuntu SSH X11 forwarding](https://loteeyoon.github.io/2022/01/22/Ubuntu-ssh-X11-forwarding.html)에서는 GUI 환경이 없는 server에 접속하여 X11 forwarding으로 client에서 GUI를 원격으로 사용할 수 있게 했습니다. 이번 포스팅은 video file을 play해볼 것입니다.

---------------------------------------------------------

## 1. play video using VLC

    // VLC 설치
    $ sudo apt-get update
    $ sudo apt-get install vlc browser-plugin-vlc
    $ sudo apt-get install libav-tools
    // mp4 파일 실행
    $ vlc <file_name>

<p align="center"><img src="https://github.com/LoteeYoon/LoteeYoon.github.io/blob/master/vlc.JPG?raw=true"></p>

  vlc를 통해 영상을 play하니 창이 뜨긴 하지만 영상이 나오진 않았습니다.

## 2. play video using CVLC

  구글링을 통해 cvlc를 사용해봤습니다.

    $ echo $DISPLAY
        nipa2019-0137:10.0

    $ cvlc --x11-display :10.0 <file_name>
