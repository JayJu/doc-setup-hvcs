# Jenkins 설치 및 환경구성

1. 설치환경

   * Ubuntu 16.04.1

2. 도커 설치

   * 다운로드 및 설치
     ```
     curl -s https://get.docker.com/ | sudo sh
     ```
   * 사용자에 root권한 부여
     ```
     sudo usermod -aG docker hvcs
     ```
   * 권한 적용을 위해 사용자 로그아웃 & 로그인
   * 도커 버전 확인

     ```
     $ docker version
     Client:
      Version:      17.05.0-ce
      API version:  1.29
      Go version:   go1.7.5
      Git commit:   89658be
      Built:        Thu May  4 22:10:54 2017
      OS/Arch:      linux/amd64

     Server:
      Version:      17.05.0-ce
      API version:  1.29 (minimum version 1.12)
      Go version:   go1.7.5
      Git commit:   89658be
      Built:        Thu May  4 22:10:54 2017
      OS/Arch:      linux/amd64
      Experimental: false
     ```

   * Jenkins 이미지 받기 및 컨테이너 시작

     ```
     docker run -p 8080:8080 -p 50000:50000 jenkinsci/jenkins:lts
     ```

   * Docker File 수정

     ```
     docker ps -a
     docker images
     docker start/stop 컨테이너이름
     docker attach 컨테이너이름

     ```

   * 도커 빌드

     ```
     sudo mkdir /var/jenkins_home
     docker build -t ubuntu-jenkins .
     ```

   * ㅇㅇ



