# Kurento Media Server 설치 및 환경구성 for CentOS 7

  * 설치환경
    * RHEL 7.4
    * 참고자료: [Kurento Media Server and components](https://github.com/pkgs-cloud/kurento/blob/master/README.md)
   
---
1. Subscription Manager 로그인
  ```
  # subscription-manager register --username autoeveridcadmin  --auto-attach
  # yum update
  ```
2. 사용자 추가 및 권한 설정
  * 사용자 추가
  ```
  # useradd ham -m -d /home/ham
  # passwd ham
  ```
  * sudoer 등록
  ```
  # visudo
  ## Allow root to run any commands anywhere
  root    ALL=(ALL)       ALL
  ham     ALL=(ALL)       ALL
  ```
  
3. pkgs.cloud release repository 설치(ham 으로 로그인 후 진행)
  * 참고자료: [pkgs.cloud Release Repository – RPM packages for RHEL / CentOS 7](https://github.com/pkgs-cloud/release)
  
  * repository package 설치
  ```
  $ sudo yum install https://get.pkgs.cloud/release.rpm -y
  ```
  * 사용가능한 repository package 확인
  ```
  $ yum --disablerepo="*" --enablerepo="pkgs.cloud" list available
  ```

4. 확장 패키지 설치
  * epel package 설치
  ```
  $ sudo yum install epel-release -y
  $ yum repolist
  $ sudo yum update
  ```
5. Kurento 패키지 & KMS 설치
  * Kurento package 설치
  ```
  $ sudo yum install kurento-release -y
  $ sudo yum install kms-6.6.3 -y
  ```
  * 추가 패키지 설치(필요시)
  ```
  $ sudo yum install kms-filters-chroma -y
  $ sudo yum install kms-filters-crowddetector -y
  $ sudo yum install kms-filters-platedetector -y
  $ sudo yum install kms-filters-pointerdetector -y
  ```
6. STUN 서버 등록
  * 설정파일 위치: ``` /etc/kurento ```
  * 공개 STUN 서버 목록
    * https://gist.github.com/zziuni/3741933
  * STUN 서버 등록
  ```
  $ sudo vi /etc/kurento/modules/kurento/WebRtcEndpoint.conf.ini
  stunServerAddress=74.125.23.127
  stunServerPort=19302
  ```
7. 방화벽 포트 오픈
  * firewall-cmd 설치
  ```
  $ sudo yum install firewalld
  $ sudo systemctl start firewalld
  ```
  * UDP 포트 오픈
  ```
  $ sudo firewall-cmd --zone=public --permanent --add-port=49152-65535/udp
  $ sudo firewall-cmd --zone=public --permanent --add-port=8888/tcp
  $ sudo systemctl reload firewalld
  ```
8. 서비스 시작/종료
  ``` 
  $ sudo systemctl enable kms.service
  $ sudo systemctl start kms.service
  $ sudo systemctl restart kms.service
  ``` 
9. 로그 확인
  * 로그파일 위치
    * ``` /var/log/kurento ```
    * ``` /etc/sysconfig/kms ```
