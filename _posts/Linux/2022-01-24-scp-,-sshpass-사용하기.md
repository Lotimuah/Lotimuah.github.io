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

  scp 명령어를 사용하면 다른 컴퓨터와 파일 전송을 할 수 있습니다. 최근 보안 이슈로 인해 ftp나 ncftp보다 주고 받는 데이터를 암호화하는 scp 방식이 좀 더 안전하다고 합니다.

### 1. scp

    $ scp -P [port_number] -r [source_path] user@ip:[target_path]
    // file을 전송할 경우 -r 옵션은 생략가능

### 2. sshpass

  scp로 file을 가져오거나 보낼 때, 접속 계정의 비밀번호를 입력해야 합니다. 그러나 sshpass를 이용하면 명령어에 암호를 같이 전달하여 명령어 입력만으로 file 전송이 가능합니다.

    $ sudo apt-get install sshpass

    // 'password'를 직접 전달
    $ sshpass -p ['password'] scp -P [port_number] -r [source_path] user@ip:[target_path]

    // password를 txt로 저장하여 경로 전달
    $ sshpass -p [passwd_file] scp -P [port_number] -r [source_path] user@id:[target_path]

### 3. ssh key

  ssh key 방식을 사용하면 scp를 사용하더라도 비밀번호를 전달하지 않아도 된다.

  \*이전 포스팅 참고 [SSH key](https://Lotymuah.github.io/2022/01/23/SSH-key.html)
