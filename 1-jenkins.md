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

   * Jenkins Repository Clone

     ```
     git clone https://github.com/jenkinsci/docker.git
     ```

   * Docker File 수정

     ```
     cd docker
     vi Dockerfile

     1. base image 수정
     ---------------------------------------
     FROM openjdk:8-jdk -> FROM ubuntu:16.04

     2. 관리자정보 추가
     ---------------------------------------
     MAINTAINER heejin <heejin_ju@hyundai-autoever.com>

     3. RUN절 설치 프로그램 추가
     ---------------------------------------
     RUN 절 ->
     # install openjdk-8, unzip, wget, vim
     RUN \
         apt-get update && \
         apt-get install -y git curl && \
         apt-get install -y unzip && \
         apt-get install -y wget && \
         apt-get install -y vim && \
         apt-get install -y openjdk-8-jdk && \
         rm -rf /var/lib/apt/lists/*

     4. Gradle 설정 추가
     ---------------------------------------
     # Add Gradle
     ADD https://services.gradle.org/distributions/gradle-3.5.1-all.zip /opt/
     RUN unzip /opt/gradle-3.5.1-all.zip -d /opt/gradle
     ENV GRADLE_HOME /opt/gradle/gradle-3.5.1-all
     ENV PATH $GRADLE_HOME/bin:$PATH
     ```

   * 도커 빌드

     ```
     sudo mkdir /var/jenkins_home
     docker build -t ubuntu-jenkins .
     ```

   * ㅇㅇ



