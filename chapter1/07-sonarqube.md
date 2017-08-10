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
2. DB 및 계정 생성
    * mariadb root접속 후 DB/User 생성 및 권한부여
    ```
    $ mysql -uroot -p
    db> CREATE DATABASE sonar CHARACTER SET utf8 COLLATE utf8_bin; 
    db> GRANT ALL PRIVILEGES ON sonar.* TO 'sonar'@'localhost' IDENTIFIED BY 'pwd';
    db> GRANT ALL PRIVILEGES ON sonar.* TO 'sonar'@'%' IDENTIFIED BY 'pwd';
    db> FLUSH PRIVILEGES;
    ```

3. 환경파일 수정
    * 포트변경
    ```
    $ cd sonar/so*/conf
    $ vi sonar.properties
        -> sonar.web.port=9000 주석 제거 후 8000으로 변경
        -> jenkins, webpack 과의 포트 충돌 피하기위해 9xxx포트 8xxx로 일괄변경
        -> MySQL jdbc 설정 주석 제거 및 수정
            sonar.jdbc.url=jdbc:mysql://115.68.221.53:3306/sonar?useUnicode=true&characterEncoding=utf8&rewriteBatchedStatements=true&useConfigs=maxPerformance&useSSL=false
        -> JDBC user/pwd 주석제거 및 수정
            sonar.jdbc.username=sonar
            sonar.jdbc.password=pwd
        -> "sonar.embeddedDatabase.port=8092" 주석처리
    $ cd ../bin/linux-x86-64/
    $ ./sonar.sh console
    ```
    
3. 방화벽 포트오픈
    * [8000번포트 오픈](/chapter1/04-firewall.md)