# Gradle 설치 및 환경설정
1. gradle 다운로드 - version4
    ```
    $ su -
    $ cd /opt/
    $ mkdir gradle
    $ wget https://services.gradle.org/distributions/gradle-4.0.2-bin.zip
    $ unzip -d ./gradle ./gradle-4.0.2-bin.zip
    ```
2. profile 설정
    ```
    $ su - hvcs
    $ vi ./.profile -> 아래 환경변수 추가
    ## gradle
    PATH="/opt/gradle/gradle-4.0.2/bin:$PATH"
    $ source ./.profile
    $ gradle -v
    ------------------------------------------------------------
    Gradle 4.0.2
    ------------------------------------------------------------
    
    Build time:   2017-07-26 15:04:56 UTC
    Revision:     108c593aa7b43852f39045337ee84ee1d87c87fd
    
    Groovy:       2.4.11
    Ant:          Apache Ant(TM) version 1.9.6 compiled on June 29 2015
    JVM:          1.8.0_131 (Oracle Corporation 25.131-b11)
    OS:           Linux 4.4.0-83-generic amd64
    ```