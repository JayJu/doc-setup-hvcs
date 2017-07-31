# 도커 (Docker)

1. 기초
2. 운영관련 명령어
    * 컨테이너 정보 확인
    ```
    $ docker inspect [container-name]
    ```
    
    * 프로세스 확인
    ```
    $ docker ps -a
    ```
    
    * 프로세스 중단
    ```
    $ docker stop [container-name]
    ```
    
    * 컨테이너 삭제
    ```
    $ docker rm [container-name]
    ```
    
    * 이미지 삭제
    ```
    $ docker images
    $ docker rmi [image-id]
    ```