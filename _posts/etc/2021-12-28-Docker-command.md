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


### 3. Generate container

    $ docker run [imgae_name]
    $ docker run --name [container_name] [image_name]  // container name 지정
    $ docker run -i -t --name [container_name] -p [host_port1]:[container_port1] -p [host_port2]:[container_port2] [image_name] [/bin/bash]

### 4. Print container list

    $ docker ps      // list only running containers
    $ docker ps -a   // list all stopped and running containers

### 5. Remove container

    $ docker rm [container_name]
    $ docker rm [container_ID]

### 6. Remove image

    $ docker rmi [image_name]
