# OOS \(Office Online Server\) 설치 및 환경구성

* 참고자료

  * \[Office Online Server 설치\]\([https://technet.microsoft.com/ko-kr/library/mt572054\(v=exchg.150\).aspx](https://technet.microsoft.com/ko-kr/library/mt572054%28v=exchg.150%29.aspx)\)

* 설치환경

  * Windows Server 2008 R2 Standard SP1

* [VLSC\(Volume Licensing Service Center\)](https://www.microsoft.com/Licensing/servicecenter/default.aspx) 에서 OOS 다운로드

* AD\(Active Directory 구성\)

* 도메인 가입 \(AD JOIN\)

  > **주의사항**  
  > OOS 서버는 반드시 도메인의 일부여야 하나 **도메인 컨트롤러에 설치하면 안됨**\(작동안함\)  
  > [설치 주의사항 및 요구사항](https://technet.microsoft.com/ko-kr/library/2e147f11-6f47-46bc-90bf-b2f179958d11#software)

  * 참고자료
    * [Active Directory: 도메인 컨트롤러 무작정 설치하기](http://archmond.net/?p=671)

* Windows 역할\(role\)과 서비스 추가 \(Win2008\)

  * admin 권한으로 파워 쉘 실행

  * 서버역할 과 서비스 추가

    * win2008 의 경우

    * ```
      PS>Import-Module ServerManager
      PS>Add-WindowsFeature Web-Server,Web-WebServer,Web-Common-Http,Web-Static-Content,Web-App-Dev,Web-Asp-Net,Web-Net-Ext,Web-ISAPI-Ext,Web-ISAPI-Filter,Web-Includes,Web-Security,Web-Windows-Auth,Web-Filtering,Web-Stat-Compression,Web-Dyn-Compression,Web-Mgmt-Console,Ink-Handwriting,IH-Ink-Support
      ```
    * win2012의 경우

    * ```
      PS>Add-WindowsFeature Web-Server,Web-Mgmt-Tools,Web-Mgmt-Console,Web-WebServer,Web-Common-Http,Web-Default-Doc,Web-Static-Content,Web-Performance,Web-Stat-Compression,Web-Dyn-Compression,Web-Security,Web-Filtering,Web-Windows-Auth,Web-App-Dev,Web-Net-Ext45,Web-Asp-Net45,Web-ISAPI-Ext,Web-ISAPI-Filter,Web-Includes,InkandHandwritingServices
      ```
    * 서버 리부팅

* OOS 설치

* Farm 생성

  * http 환경으로 구성 할 경우
  * ```
    PS>New-OfficeWebAppsFarm –InternalURL "http://hvcs-oos" -ExternalUrl "http://hvcs-oos.hvcsd.com" –AllowHttp
    ```
  * https 환경으로 구성 할 경우
  * ```

    ```
  * Farm 정보 확인
  * ```
    PS>Get-OfficeWebAppsFarm
    -> InternalURL 과 ExternalURL 확인
    -> OpenFromUrlEnabled 와 OpenFromUncEnabled 가 true 인지 확인, 아니면 아래 명령으로 수정
    PS>Set-OfficeWebAppsFarm – OpenFromUrlEnabled:$true
    ```

* UNC  Location 설정  
  서버 Local 경로를 소스로 사용할 경우 설정

  1. 폴더지정

     * 소스로 지정할 폴더 생성

  2. 권한설정

     * 공유하고자 하는 폴더의 “Properties &gt; Sharing Tab”으로 이동

     * “Advanced Sharing..:” 버튼을 클릭  
       ![](/img/ch1/sub2/1-2-1.jpg)

     * “Share this folder”을 Check 후 “Permissions” 버튼을 클릭  
       ![](/img/ch1/sub2/1-2-2.jpg)

     * “Object Types…” 버튼을 클릭  
       ![](/img/ch1/sub2/1-2-4.jpg)

     * “Computers”을 클릭하여 검색조건에 포함  
       ![](/img/ch1/sub2/1-2-5.jpg)

     * “Add” 버튼을 클릭  
       ![](/img/ch1/sub2/1-2-3.jpg)

     * Properties &gt; Security -&gt; Edit &gt; OOS 서버를 추가  
       ![](/img/ch1/sub2/1-2-6.jpg)

     * OOS 상의 UNC URL 확인

       * /etc/hosts 파일에 oos 서버 url 등록
       * [http://hvcs-oos.hvcsd.com/op/generate.aspx](http://hvcs-oos.hvcsd.com/op/generate.aspx) 로 이동 후 UNC Path를 입력하여 URL을 획득
       * 위의 획득한 URL이 아래처럼 정상적으로 출력됨을 확인

* URL 설정

  * IIS 내에 Virtual Directory 구성
    * IIS 을 실행
    * 테스트를 위해 “hosting” Context를 사용. 아래와 같이 “Add Virtual Directory..”을 클릭하여 “oofs”폴더를 추가
      ![](/img/ch1/sub2/1-2-7.jpg)
  * OOS 상의 URL 확인- “[http://hvcs-oos.hvcsd.com/op/generate.aspx”](http://hvcs-oos.hvcsd.com/op/generate.aspx”) 로 이동 후 URL Path를 입력하여 URL을 획득

  * Create Link 클릭 후 Test this link 로 viewer 확인  
    ![](/img/ch1/sub2/1-2-8.jpg)

* DRM 디렉토리 설정 - AP 서버

  * sftp 전용 계정 생성
    ```
    # useradd drm -m -d /home/drm
    # passwd drm
    ```
  * sftp 디렉토리 생성

    ```
    # mkdir -p /files/drm
    # chmod -R 755 /files
    ```

  * SFTP 사용 시 상위 디렉토리로 이동 못하도록 설정

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

  * ssh 로 로그인이 실패하는 지 확인

  * sftp 로그인 하여 디렉토리 이동제한 확인

* SSHFS 설정\(OOS서버\)

  > OOS에서 AP서버의 drm 파일을 어떤 방식으로 접근해야 할지 고민이 필요함  
  > Ubuntu에서 Samba로 접근을 제공할지  
  > 아래 처럼 AP에 SFTP 구성을 하고 OOS에서 접근하는 방식으로 할지

  * AP\(Ubuntu SFTP\) -&gt; OOS로 networkdrive 걸기
  * [SFTPNetDrive 다운로드 및 설치](http://www.sftpnetdrive.com/download-thanks)

  * SFTPNetDrive 연결 추가 및 테스트

* Samba 설정
  * filesystem 생성
  ```
  $ sudo mkdir -p /hvcs/files/conference
  $ sudo chmod -R 755 /hvcs/files
  ```
  
  * samba 설치 및 확인
  ```
  $ sudo apt-get install -y samba
  $ sudo smbd --version
  $ sudo systemctl status smdbd
  $ sudo systemctl status nmbd
  ```
  
  * 환경파일 설정
  ```
  $ sudo vi /etc/samba/smb.conf
   [global]
   unix charset = UTF-8
   
   [HvcsShare]
   comment = share presentation files for hvcs
   browseable = yes
   path = /hvcs/files/conference
   guest ok = no
   read only = yes
   valid users = drm
   create mask = 0700
   directory mode = 0700
   
  $ sudo smbpasswd -a drm
  ```
  
  * 서비스 재시작
  ```
  $ sudo systemctl restart smbd
  ```


