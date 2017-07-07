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

4. Jenkins 시작

   * systemctl을 사용하여 Jenkins 시작

   * ```
     $ sudo systemctl start jenkins
     ```
   * status 확인

   * ```
     $ sudo systemctl status jenkins
      jenkins.service - LSB: Start Jenkins at boot time
        Loaded: loaded (/etc/init.d/jenkins; bad; vendor preset: enabled)
        Active: active (exited) since Fri 2017-07-07 09:47:29 KST; 2min 1s ago
          Docs: man:systemd-sysv-generator(8)
     ```
   * 1

5. 방화벽 오픈

   * 방화벽 정책 수정 - 8080포트 추가

   * ```
     $ sudo vi /etc/iptables/rules.v4
     ```
   * rules.v4에 아래 내용 추가 후 저장/닫기

   * ```
     -A RH-Firewall-1-INPUT -p tcp -m state --state NEW -m tcp --dport 8080 -j ACCEPT
     ```
   * 방화벽 재시작

   * ```
     $ sudo netfilter-persistent reload
     ```
   * 정책확인

   * ```
     $ sudo iptables -L
     ```

6. Jenkins 설정

   * Jenkins 접속

   * ```
     http://ip_address_or_domain_name:8080
     ```
   * Unlock Jenkins

   * 1

   * 1

   * 

7. 도커 설치

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



