---
layout: article
title: Jupyter notebook setting
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

  \* Anaconda 가상환경 위에서 Jupyter notebook을 설치하고 setting

  ------------------------------------------------------------------

### 1. Jupyter install

    $ pip install notebook

### 2. config 파일 생성

    $ jupyter notebook --generate-config

### 3. password 생성

    $ python
    $ from notebook.auth import passwd
    $ passwd()
    // 비밀번호 입력 후 생성된 해쉬 복사

### 4. config 파일 세팅

    $ vim ~/.jupyter/jupyter_notebook_config.py

### 5. setting

&nbsp;&nbsp;비밀번호 설정 : c.NotebookApp.password 주석 해제 후 복사해둔 해쉬 붙여넣기
&nbsp;&nbsp;기본 작업 경로 설정 : c.NotebookApp.notebook_dir 주석 해제 후 실행하고자 하는 dir 지정
&nbsp;&nbsp;외부 접속 허용 : c.NotebookApp.allow_origin = '*'


### 6. Add kernel

    $ conda activate [env]
    $ pip install ipykernel
    $ python -m ipykernel install --user --name [env] --display-name "[display_name]"

### 7. Remove kernel

    $ jupyter kernelspec uninstall [display_name]
    
