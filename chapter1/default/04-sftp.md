# SFTP 설정
> AP서버의 회의 첨부파일 접근용으로 설정 했으나 이후 OOS에서의 가상디랙토리를 통한 접근방법에 퍼미션 문제로 인해 Samba로 대체함

1. sftp 전용 계정 생성
  ```
  # useradd drm -m -d /home/drm
  # passwd drm
  ```

2. sftp 디렉토리 생성
  ```
  # mkdir -p /files/drm
  # chmod -R 755 /files
  ```

3. SFTP 사용 시 상위 디렉토리로 이동 못하도록 설정

  * root 로 /etc/ssh/sshd\_config 마지막에 아래 설정 추가
  ```
  ############ Setup by jay #############
  # Subsystem sftp /usr/lib/openssh/sftp-server -> 주석처리
  Subsystem sftp internal-sftp
  # This section must be placed at the very end of sshd_config
  # sftponly 그룹 생성하여 제약걸기
  Match Group sftponly
  ChrootDirectory %h
  ForceCommand internal-sftp
  AllowTcpForwarding no
  ```
    
  * 그룹생성
  ```
  # groupadd sftponly
  ```

  * ftp 전용 계정의 shell 접근제한
  ```
  # usermod drm -g sftponly
  # usermod drm -s /bin/false
  # usermod drm -d /files/drm
  ```

  * ssh 데몬 재시작
  ```
  # systemctl stop ssh
  # systemctl status ssh
  ```

4. 확인
  * ssh 로 로그인이 실패하는 지 확인
  * sftp 로그인 하여 디렉토리 이동제한 확인

