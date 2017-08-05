# JDK 환경설정
1. JDK 설치(version 8)
  ```
  $ sudo apt-get install software-properties-common
  $ sudo add-apt-repository ppa:webupd8team/java
  $ sudo apt-get update
  $ sudo apt-get install oracle-java8-installer
  ```
2. Default JDK 설정 (2개 이상 설치 된 경우)
  * 설치 목록 확인 및 변경
  ```
  $ sudo update-alternatives --config java
      Selection Path Priority Status
    ------------------------------------------------------------
    0 /usr/lib/jvm/java-8-oracle/jre/bin/java 1081 auto mode
    1 /usr/lib/jvm/java-8-openjdk-amd64/jre/bin/java 1081 manual mode
    * 2 /usr/lib/jvm/java-8-oracle/jre/bin/java 1081 manual mode
    
    Press <enter> to keep the current choice[*], or type selection number: 1
  ```
  * default 확인
  ```
  $ sudo update-alternatives --config java
      Selection Path Priority Status
    ------------------------------------------------------------
    0 /usr/lib/jvm/java-8-openjdk-amd64/jre/bin/java 1081 auto mode
    * 1 /usr/lib/jvm/java-8-openjdk-amd64/jre/bin/java 1081 manual mode
    2 /usr/lib/jvm/java-8-oracle/jre/bin/java 1081 manual mode
  ```
3. JAVA_HOME 환경변수 설정
  * 위 default 설정된 jdk의 경로 복사(/usr/lib/jvm/java-8-openjdk-amd64)
  * 환경파일에 JAVA_HOME 추가 (JAVA_HOME="/usr/lib/jvm/java-8-openjdk-amd64")
  
    ```
    $ sudo vi /etc/environment
    $ source /etc/environment
    $ echo $JAVA_HOME
    /usr/lib/jvm/java-8-openjdk-amd64
    ```