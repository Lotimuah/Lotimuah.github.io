---
layout: article
title: scp, sshpass 사용하기
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

  scp 명령어를 사용하면 다른 컴퓨터와 파일 전송을 할 수 있습니다.

### 1. scp

    $ scp -P [port_number] -r [source_path] user@ip:[target_path]
    // file을 전송할 경우 -r 옵션은 생략가능

### 2. sshpass
