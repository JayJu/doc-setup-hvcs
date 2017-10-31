# TPC-C for Mysql 측정 방법

> ### TPC-C 란?

1. 소프트웨어 설치
  * Percona Server
    * 레파지토리 패키지 다운로드
    ```
    $ wget https://repo.percona.com/apt/percona-release_0.1-4.$(lsb_release -sc)_all.deb
    ```
    * 패키지 설치
    ```
    $ sudo dpkg -i percona-release_0.1-4.$(lsb_release -sc)_all.deb
    ```
    * apt-get 업데이트
    ```
    $ sudo apt-get update
    ```
    * Percona Server 설치
    ```
    $ sudo apt-get install percona-server-server-5.5
    ```
  * tcpp-mysql
    ```
    $ sudo apt-get install make
    $ sudo apt-get install gcc
    $ sudo apt install libmysqlclient-dev
    -> .profile 에 mysql_config 경로 PATH에 지정 후 sourcing
    $ sudo apt-get install bzr
    $ bzr branch lp:~percona-dev/perconatools/tpcc-mysql
    $ cd tpcc-mysql/src
    $ make
    ```
 
2. DB Config
  * DB 생성
  ```
  $ mysql -u root -p -e "CREATE DATABASE tpcc1000;"
  $ mysql -u root -p tpcc1000 < create_table.sql
  $ mysql -u root -p tpcc1000 < add_fkey_idx.sql
  ```
  * 데이터 적재
  ```
  $ ./tpcc_load 127.0.0.1 tpcc1000 root "root-password(특수기호넣지말기)" 20
  .......... 1000
  .......... 2000
  .......... 3000
  Orders Done.
  ...DATA LOADING COMPLETED SUCCESSFULLY.
  ```
3. tpcc-mysql 테스트
  * innodb buffer pool size 변경(to 512M)
  ```
  $ vi /etc/mysql/my.cnf
  [mysqld]
  innodb_buffer_pool_size = 1G
  $ sudo service mysql stop
  $ sudo service mysql start
  $ service mysql status
  $ mysql -uroot -p
  mysql> SHOW VARIABLES LIKE 'innodb_buffer_pool_size';
  mysql> exit
  ```
  * 테스트 시작
  ```
  $ ./tpcc_start -h127.0.0.1 -dtpcc1000 -uroot -p "eoqkrskwk" -w20 -c16 -r10 -l600 > ./tpcc-output-ps-bpool-1024.log
  ** 파라미터 
  Host: 127.0.0.1
  DB: tpcc1000
  User: root
  Warehouse: 20
  Connection: 16
  Rampup time: 10 (sec)
  Measure: 600 (sec)
  ```
  * buffer_pool_size 2G로 변경 후 다시 테스트
  ```
  $ ./tpcc_start -h127.0.0.1 -dtpcc1000 -uroot -p "eoqkrskwk" -w20 -c16 -r10 -l600 > ./tpcc-output-ps-bpool-2048.log
  ```
  
4. 테스트결과 데이터 파일 생성
5. 결과그래프 생성
