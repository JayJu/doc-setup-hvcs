# Jenkins 설치 및 환경구성

1. 설치환경

   * Ubuntu 16.04.1

2. 사용자 추가 및 권한 설정

   * 사용자 추가
   * ```
     # useradd jenkins -m -d /home/jenkins
     # passwd jenkins
     ```
   * sudoer 등록
   * ```
     # usermod -aG sudo jenkins
     ```

3. Jenkins 설치\(jenkins로 로그인 후 진행\)

   * 레파지토리 키 등록
   * ```
     $ wget -q -O - https://pkg.jenkins.io/debian/jenkins-ci.org.key | sudo apt-key add -
     ```
   * sources.list 에 데비안 패키지 레파지토리 추가
   * ```
     $ echo deb http://pkg.jenkins.io/debian-stable binary/ | sudo tee /etc/apt/sources.list.d/jenkins.list
     ```
   * apt-get 업데이트
   * ```
     $ sudo apt-get update
     ```
   * Jenkins 와 dependencies 설치
   * ```
     $ sudo apt-get install jenkins
     ```
   * 

4. Jenkins 서비스 시작
5. 도커 설치

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



