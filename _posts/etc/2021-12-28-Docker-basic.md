---
layout: article
title: Docker basic
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

### 0. Docker install

    // 자동 설치 스크립트
    $ curl -fsSL https://get.docker.com/ | sudo sh

    // 패키지 직접 설치
    $ sudo apt-get update
    $ sudo apt-get install docker.io
    $ sudo ln -sf /usr/bin/docker.io /usr/local/bin/docker

### 1. Image search

    $ docker search [image_name]

### 2. Image pull

    $ docker pull [image_name]


### 3. Print image list

    $ docker images


### 4. Container generate & run

    $ docker run -itd \                             // -i : stdin, -t : stdout, -d : container background 실행 (daemon mode)
    --name [container_name] \
    -h [host_name] \
    --gpus all \                                    // --gpus '"device=0,1"' : 특정 gpu 지정 가능
    --restart always \                              // container 항상 재시작
    --ip [123.45.67.890] \
    -v [out_dir_path]:[container_dir_path] \        // -v : 외부 dir와 container의 dir 동기화
    -p [host_port1]:[container_port1] \             // -p : 외부 port와 container port 동기화 (port forwarding)
    -p [host_port2]:[container_port2] \
    [image_name]

### 5. Container access (CLI)

    $ docker exec -it [container_name] /bin/bash    // container의 /bin/bash를 shell로 하여 표준입출력(-it)으로 접속

### 6. Container root setting

    $ docker exec -u 0 -it [container_name] bash    // -root(UID=0)로 container접속
    $ root@container_name:/# passwd
      Enter new UNIX password:
      Retype new UNIX password:

### 7. Container stop

    $ docker stop [container_name]

### 8. Container start

    $ docker run [container_name]

### 9. Container restart

    $ docker restart [container_name]

### 10. Container rename

    $ docker rename [current_name] [new_name]

### 11. Print container list

    $ docker ps                      // list only running containers
    $ docker ps -a                   // list all stopped and running containers

### 12. Print container information

    $ docker inspect [container_name]

### 13. Remove container

    $ docker rm [container_name]
    $ docker rm [container_ID]

### 14. Remove image

    $ docker rmi [image_name]

### 15. Image commit

    $ docker commit [container_name] [new_image_name]

### 16. Image push

    $ docker login
    $ docker tag [new_image_name] [docker_ID]/[new_image_name:TAG]
    $ docker push [docker_id]/[new_image_name:TAG]
