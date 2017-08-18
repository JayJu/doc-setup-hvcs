# Samba 설치 및 환경설정 (OOS에서 접근)

1. filesystem 생성
    ```
    $ sudo mkdir -p /hvcs/files/conference
    $ sudo chmod -R 755 /hvcs/files
    $ sudo usermod drm -d /hvcs/files/conference
    ```

2. samba 설치 및 확인
    ```
    $ sudo apt-get install -y samba
    $ sudo smbd --version
    $ sudo systemctl status smdbd
    $ sudo systemctl status nmbd
    ```

3. 환경파일 설정  
    ```
    $ sudo vi /etc/samba/smb.conf  
        [global]  
        unix charset = UTF-8

        [HvcsShare]  
        comment = share presentation files for hvcs  
        browseable = yes  
        path = /hvcs/files/conference  
        guest ok = yes  
        read only = yes  
        #valid users = drm -> 주석처리해야함
        create mask = 0700  
        directory mode = 0700
        
  $ sudo smbpasswd -a drm
  ```
  
4. 서비스 재시작
    ```
    $ sudo systemctl restart samba
    ```
        
    > ** Unit samba.service is masked.** 에러 발생 시 아래 조치
    ```
    $ sudo rm /lib/systemd/system/samba.service  
    $ sudo systemctl start samba  
    ```
    
