---
layout: article
title: Docker setting
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

docker image를 받아와
### 1

### 1. Container 생성 및 실행

  $ docker run -itd\
  --name custom_container\
  --hostname server\
  --gpus all\
