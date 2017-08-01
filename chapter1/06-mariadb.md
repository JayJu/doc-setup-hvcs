# MariaDB 설치 및 환경구성

* 참고문서
    * [Installing and using MariaDB via Docker](https://mariadb.com/kb/en/mariadb/installing-and-using-mariadb-via-docker/)
    * [Application running on Docker container – Part 2 – Database](https://blog.lysender.com/2015/10/application-running-on-docker-container-part-2-database/)
    * [Docker를 이용하여 MAIRADB BINARY 설치 가이드 문서](http://lahuman.jabsiri.co.kr/118)
    
    
1. 도커 설치
    1) 다운로드 및 설치
       ```
       curl -s https://get.docker.com/ | sudo sh
       ```

    2) 사용자에 root권한 부여
       ```
       sudo usermod -aG docker hvcs
       ```

    3) 권한 적용을 위해 사용자 로그아웃 & 로그인

    4) 도커 버전 확인
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

2. 방화벽 설정
    1) OS 방화벽 설정
        * 환경구성 - Firewal 참고
 
3. MariaDB Image 생성
    1) Docker 파일 다운로드
    [도커허브](https://hub.docker.com/_/mariadb/)에서 [10.2](https://github.com/docker-library/mariadb/blob/bcf4518ad93834454bcca8029444231bc044afa3/10.2/Dockerfile) 버전의 Docker 파일 다운로드
    ```
    $ cd ~
    $ mkdir -p dockerfiles/mariadb
    $ cd dockerfiles/mariadb
    $ vi Dockerfile <-- Dockerfile 내용 붙여넣고 저장
    ```
    2) Dockerfile 수정
    - 추가
    ``` 
    MAINTAINER Ju Hee Jin <iam@juheejin.com>
    ENV TERM linux
    ENV DEBIAN_FRONTEND noninteractive
    ..
    RUN mkdir /scripts
    ..
    # Added configuration
    ADD ./config_mariadb.sh /scripts/config_mariadb.sh
    ADD ./hvcs.cnf /etc/mysql/conf.d/hvcs.cnf
    RUN chmod 755 /scripts/config_mariadb.sh
    RUN /scripts/config_mariadb.sh
    
    RUN apt-get update && apt-get install -y vim
    ```
    
    - 제거
    ```
    # COPY docker-entrypoint.sh /usr/local/bin/
    # RUN ln -s usr/local/bin/docker-entrypoint.sh / # backwards compat
    # ENTRYPOINT ["docker-entrypoint.sh"]
    ```
    
    - 변경
    ```
    CMD ["mysqld", "--user=mysql"]
    ```
        
    3) Post Script 생성 ( config_mariadb.sh )
    ```
    #!/bin/bash
    
    __mysql_config() {
        # Hack to get MySQL up and running... I need to look into it more.
        echo "Running the mysql_config function."
        #yum -y erase community-mysql community-mysql-server
        #rm -rf /var/lib/mysql/ /etc/my.cnf
        #yum -y install community-mysql community-mysql-server
        mysql_install_db
        chown -R mysql:mysql /var/lib/mysql
        /usr/bin/mysqld_safe &
        sleep 10
    }
    
    __start_mysql() {
        echo "Running the start_mysql function."
        mysqladmin -u root password foobar
        mysql -uroot -pfoobar -e "CREATE DATABASE hvcsdb"
        mysql -uroot -pfoobar -e "GRANT ALL PRIVILEGES ON hvcsdb.* TO 'hvcs'@'localhost' IDENTIFIED BY 'password'; FLUSH PRIVILEGES;"
        mysql -uroot -pfoobar -e "GRANT ALL PRIVILEGES ON hvcsdb.* TO 'hvcs'@'%' IDENTIFIED BY 'password'; FLUSH PRIVILEGES;"
        mysql -uroot -pfoobar -e "GRANT ALL PRIVILEGES ON *.* TO 'hvcs'@'%' IDENTIFIED BY 'password' WITH GRANT OPTION; FLUSH PRIVILEGES;"
        mysql -uroot -pfoobar -e "GRANT ALL PRIVILEGES ON *.* TO 'hvcs'@'%' IDENTIFIED BY 'password' WITH GRANT OPTION; FLUSH PRIVILEGES;"
        mysql -uroot -pfoobar -e "select user, host FROM mysql.user;"
        killall mysqld
        sleep 10
    }
    
    # Call all functions
    __mysql_config
    __start_mysql
    
    ```
    
    4) Custom cnf 생성 ( hvcs.cnf )
    ```
    [client]
    default-character-set=utf8
    
    [mysql]
    default-character-set=utf8
    
    [mysqld]
    lower_case_table_names=1
    collation-server=utf8_unicode_ci
    init-connect='SET NAMES utf8'
    character-set-server=utf8
    ```
    
    5) Docker Build
    ```
    $ docker build --rm --tag mariadb:10.2 .
    ...
    Removing intermediate container 0a8ce649a53f
    Successfully built 195c541182be
    Successfully tagged mariadb:10.2
    ```
    >**IWINV 서비스 사용 시 주의사항**
    IWINV 가상서버 내 인스턴스 MTU를 1450으로 운영 중이며 docker 내 기본 MTU가 1500 이라면
    (컨테이너 접속해서 ifconfig 로 확인 가능) 설치 시 apt-get update 등이 패킷사이즈 초과로 
    실패하게 됨. 반드시 docker.service 파일 내 MTU를 1450으로 명기 해야 함. 
    자세한 내용은 IWINV 기술 메뉴얼 참고.

    5) 이미지 정상결과 확인
    ```
    $ docker images
    REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
    mariadb             10.2                195c541182be        21 minutes ago      387MB
    debian              jessie              86baf4e8cde9        6 days ago          123MB
    ```
    
4. 서비스 시작
    1) 컨테이너 실행
    ```
    $ docker run --name maria-dev -p 3306:3306 -e MYSQL_ROOT_PASSWORD=eoqkrskwk -d mariadb:10.2
    $ docker logs maria-dev
    ```
    
    2) 컨테이너 프로세스 확인
    ```
    $ docker ps -a
    CONTAINER ID        IMAGE               COMMAND                 CREATED             STATUS              PORTS                    NAMES
    deda9ec45b88        mariadb:10.2        "mysqld --user=mysql"   4 minutes ago       Up 4 minutes        0.0.0.0:3306->3306/tcp   maria-dev
    ```
    
    3) 컨테이너 접속
    ```
    $ docker exec -it maria-dev bash
    ```
    
    4) DB 접속
    ```
    $ mysql -uhvcs -p
    ..
    MariaDB [(none)]> show databases;
    +--------------------+
    | Database           |
    +--------------------+
    | hvcsdb             |
    | information_schema |
    +--------------------+
    MariaDB [(none)]> show variables like 'c%';
    +--------------------------+----------------------------+
    | Variable_name            | Value                      |
    +--------------------------+----------------------------+
    | character_set_client     | utf8                       |
    | character_set_connection | utf8                       |
    | character_set_database   | utf8                       |
    ..
    ``` 
        

    




