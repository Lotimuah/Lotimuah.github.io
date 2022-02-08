---
layout: article
title: NVIDIA Driver & CUDA Tookit 설치
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

## Remarks

  **OS** : Ubuntu 18.04 LTS(x86_64)  
  **GPU** : NVIDIA RTX 3090  
  **CUDA Toolkit version** : 11.2  
  **NVIDIA Driver version** : 460.27  


## 0. CUDA Toolkit Pre-installation Actions

  CUDA Toolkit과 driver를 설치하기 위해서 사전에 몇 가지 사항들을 확인할 필요가 있습니다. [CUDA Toolkit Installation Guide for Linux](https://docs.nvidia.com/cuda/cuda-installation-guide-linux/index.html)에서 **2.Pre-installation Actions** 과정을 진행합니다.

    // 2.1 Verify you have a CUDA-Capable GPU
    $ lspci | grep -i nvidia                  # 본인의 GPU가 CUDA-capable한지 확인

    // 2.2 Verify you have a supported version of Linux
    $ uname -m && cat /etc/*release           # 본인의 Linux version이 CUDA Toolkit을 지원하는지 확인

    // 2.3 Verify the system has gcc installed
    $ gcc --version
    $ sudo apt-get install gcc                # gcc가 없다면 설치

    // 2.4 Verify the system has the correct kernel headers and development packages installed
    $ uname -r                                # system kernel check

  5.8.0-44-generic 이라고 뜨면 5.8.0-44가 kernel version입니다.

    // kernel에 맞게 header와 package 설치
    $ sudo apt-get install linux-headers-5.8.0-44


## 1. CUDA Toolkit install file 다운로드

  [CUDA Toolkit 11.2 version](https://developer.nvidia.com/cuda-11.2.0-download-archive)에서 자신의 setting에 맞게 파일을 다운로드 합니다.

<p align="center"><img src="https://github.com/LoteeYoon/LoteeYoon.github.io/blob/master/CUDA_Toolkit.JPG?raw=true"></p>

    $ wget https://developer.download.nvidia.com/compute/cuda/11.2.0/local_installers/cuda_11.2.0_460.27.04_linux.run

## 2. Nouveau Driver 제거

  Ubuntu에서 기본적으로 설치되어 있는 **Nouveau driver**와 설치하려는 **NVIDIA driver**가 충돌하는 문제를 해결하기 위해 미리 제거해줍니다. 만약 **NVIDIA driver**가 이미 설치되어 있는 상태라면 이를 전부 제거해주어야 합니다.

    // NVIDIA driver 삭제
    $ sudo apt-get purge nvidia*
    $ sudo apt-get autoremove
    $ sudo apt-get autoclean

    // CUDA 삭제
    $ sudo rm -rf /usr/local/cuda*
    $ sudo apt-get --purge remove 'cuda*'
    $ sudo apt-get autoremove --purge 'cuda*'

    // NVIDIA driver, CUDA 삭제 확인
    $ sudo dpkg -l | grep nvidia    # 아무 것도 출력되지 않으면 정상적으로 제거 완료
    $ sudo dpkg -l | grep cuda      # 아무 것도 출력되지 않으면 정상적으로 제거 완료

    #################################################################################
    //Nouveau driver 제거
    $ sudo apt-get install dkms build-essential linux-headers-generic
    $ sudo vi /etc/modprobe.d/blacklist.conf

      // Append below contents in blacklist.conf file
      blacklist nouveau
      blacklist lbm-nouveau
      options nouveau modeset=0
      alias nouveau off
      alias lbm-nouveau off

    $ echo options nouveau modeset=0 | sudo tee -a /etc/modprobe.d/nouveau-kms.conf
    $ sudo update-initramfs -u
    $ sudo reboot
    #################################################################################

    #################################################################################
    // 만약 위 과정이 안 된다면 다음을 실행
    $ sudo vi /etc/modprobe.d/blacklist-nouveau.conf

      // Append below contents in blacklist-nouveau.conf file
      blacklist nouveau
      options nouveau modeset=0

    $ sudo update-initramfs -u
    $ sudo reboot
    #################################################################################


## 3. NVIDIA Driver & CUDA 설치

  **NVIDIA driver** 설치를 위해 먼저 graphic display와 input device들을 관리하는 X server를 끄고 GUI 관련 서비스들을 종료해줍니다.

    // Ctrl + Alt + F1을 눌러 console 창으로 변경 후 로그인 합니다.
    $ sudo service lightdm stop
    $ sudo sh cuda_11.2.0_460.27.04_linux.run

  accept를 입력하고 설치를 진행해줍니다.

<p align="center"><img src="https://github.com/LoteeYoon/LoteeYoon.github.io/blob/master/CUDA_Installer1.JPG?raw=true"></p>

  **NVIDIA driver**와 **CUDA Toolkit** 이외의 것들은 필요없어서 저는 해제해주었습니다.

<p align="center"><img src="https://github.com/LoteeYoon/LoteeYoon.github.io/blob/master/CUDA_Installer2.JPG?raw=true"></p>

  설치가 완료되면 다음과 같이 나타납니다.

<p align="center"><img src="https://github.com/LoteeYoon/LoteeYoon.github.io/blob/master/CUDA_Installer3.JPG?raw=true"></p>


## 4. 환경변수 추가

    설치가 완료되었으면 설치된 cuda library를 읽어올 수 있도록 환경변수를 추가합니다.

      $ vi ~/.bashrc

        // Append below contents in ~/.bashrc
        export PATH=$PATH:/usr/local/cuda-11.2/bin
        export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:/usr/local/cuda-11.2/lib64

      $ source ~/.bashrc


## 5. NVIDIA Driver & CUDA check

    $ nvidia-smi

<p align="center"><img src="https://github.com/LoteeYoon/LoteeYoon.github.io/blob/master/NVIDIA_SMI.JPG?raw=true"></p>
