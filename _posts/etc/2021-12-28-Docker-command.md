---
layout: article
title: Docker command
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

### 1. Pull image from Docker

    $ docker pull [image_name]


### 2. Print image list

    $ docker images


### 3. Container generate & run

    $ docker run [imgae_name]
    $ docker run --name [container_name] [image_name]  // container name 지정
    $ docker run -i -t --name [container_name] -p [host_port1]:[container_port1] -p [host_port2]:[container_port2] [image_name] /bin/bash
    // -i와 -t option 은 container를 종료하지 않은 채로 터미널의 입력을 계속 container로 전달하기 위해 사용

### 4. Container start

    $ docker run [container_name]

### 5. Print container list

    $ docker ps      // list only running containers
    $ docker ps -a   // list all stopped and running containers

### 6. Remove container

    $ docker rm [container_name]
    $ docker rm [container_ID]

### 7. Remove image

    $ docker rmi [image_name]

### 8. Container stop

    $ docker stop

### 9. Container restart

    $ docker restart

### 10. Container rename

    $ docker rename [current_name] [new_name]

### 11. Container access (CLI)

    $ docker exec -it [container_name] /bin/bash  // container의 /bin/bash를 shell로 하여 표준입출력(-it)으로 접속

### 12. Print container information

    $ docker inspect

### 13. Container root setting

    $ docker exec -u 0 -it [container_name] bash  // -root(UID=0)로 container접속
    $ root@container_name:/# passwd
      Enter new UNIX password:
      Retype new UNIX password:
