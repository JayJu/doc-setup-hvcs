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
        
    3) Post Script 생성
    - config_mariadb.sh
    ```
    dd
    ```
    4) Docker Build
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
    
3. MariaDB 설치
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
        
3. 방화벽 설정
    1) OS 방화벽 설정
        * 환경구성 - Firewal 참고
    




