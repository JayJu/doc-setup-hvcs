# MariaDB 설치 및 환경구성

* 참고문서
    * [Installing and using MariaDB via Docker](https://mariadb.com/kb/en/mariadb/installing-and-using-mariadb-via-docker/)
    
    
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

2. MariaDB 설치
    1) 이미지 다운로드
    ```
    $ docker search mariadb
    $ docker pull mariadb:10.2.7
    $ docker images
        REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
        mariadb             10.2.7              afa0733d0c1b        3 days ago          387MB
    ```
    
    2) 컨테이너 생성
    ```
    $ docker run --name mariadb-dev -e MYSQL_ROOT_PASSWORD=eoqkrskwk -d mariadb:10.2.7
    $ docker ps -a
        CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS               NAMES
        8c16829635a9        mariadb:10.2.7      "docker-entrypoint..."   5 seconds ago       Up 4 seconds        3306/tcp            mariadb-dev
    
    ```
    
    3) 컨테이너 접속
    ```
    $ docker exec -it mariadb-dev bash
    ```
    
    4) DB 접속
    ```
    $ mysql -uroot -p
    
    ``` 
        

