# Kurento Media Server 설치 및 환경구성 for Redhat 7

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
  # sudo yum install https://get.pkgs.cloud/release.rpm -y
  ```
  * 사용가능한 repository package 확인
  ```
  # yum --disablerepo="*" --enablerepo="pkgs.cloud" list available
  ```

4. 확장 패키지 설치
  * epel package 설치
  ```
  # sudo yum install epel-release -y
  # yum repolist
  # yum update
  ```
5. Kurento 패키지 & KMS 설치
  * Kurento package 설치
  ```
  # sudo vi /etc/yum/pluginconf.d/search-disabled-repos.conf
  ## notify_only=0 (1 -> 0 으로 변경)
  # sudo vi /etc/yum.conf
  ## gpgcheck=0 (1 -> 0 으로 변경)
  # yum clean metadata
  # sudo yum erase gstreamer1 -y
  # sudo yum install kurento-release -y
  # sudo yum install kms-6.6.3 -y
  ```





3. 사용자 추가 및 권한 설정

   * 사용자 추가
   * ```
     # useradd jenkins -m -d /home/jenkins
     # passwd jenkins
     ```
   * sudoer 등록
   * ```
     # usermod -aG sudo jenkins
     ```
  * gradle profile 추가
  * ```
  $ su - jenkins
  $ vi ./.profile
    ## gradle
    PATH="/opt/gradle/gradle-4.0.2/bin:$PATH"
  $ source ./.profile
