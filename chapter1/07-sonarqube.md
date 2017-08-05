# SonarQube 설치 및 환경설정
1. SonarQube 다운로드 및 설치 (verion 6.5)
    ```
    $ su -
    $ cd /opt
    $ mkdir sonar
    $ wget https://sonarsource.bintray.com/Distribution/sonarqube/sonarqube-6.4.zip
    $ unzip -d ./sonar ./sonarqube-6.4.zip
    $ cd sonar/so*/bin/linux-x86-64
    ```
2. 환경파일 수정
    * 포트변경
    ```
    $ cd sonar/so*/conf
    $ vi sonar.properties
        -> sonar.web.port=9000 주석 제거 후 8000으로 변경
    $ cd ../bin/linux-x86-64/
    $ ./sonar.sh console
    ```
    
3. 방화벽 포트오픈
    * [8000번포트 오픈](/chapter1/04-firewall.md)