# OOS \(Office Online Server\) 환경설정

1. 설치환경

   * Windows Server 2008 R2 Standard SP1

2. [VLSC\(Volume Licensing Service Center\)](https://www.microsoft.com/Licensing/servicecenter/default.aspx) 에서 OOS 다운로드

3. 도메인 가입 \(AD JOIN\)

4. Windows 역할\(role\)과 서비스 추가 \(Win2008\)

   * admin 권한으로 파워 쉘 실행

   * 서버역할 과서비스 추가

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

5. OOS 설치

6. Farm 생성

   * http 환경으로 구성 할 경우
   * ```
     PS>New-OfficeWebAppsFarm –InternalURL "http://common-web" -ExternalUrl "http://somo.hyundai-steel.com" –AllowHttp -EditingEnabled
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

7. UNC  Location 설정  
   서버 Local 경로를 소스로 사용할 경우 설정  
   1. 폴더 지정

   * 소스로 지정할 폴더 생성
     1. 권한설정
   * 공유하고자 하는 폴더의 “Properties &gt; Sharing Tab”으로 이동
   * “Advanced Sharing..:” 버튼을 클릭  
     ![](/img/1-2-1.jpg)

   * “Share this folder”을 Check 후 “Permissions” 버튼을 클릭  
     ![](/img/1-2-2.jpg)

   * “Add” 버튼을 클릭  
     ![](/img/1-2-3.jpg)

   * “Object Types…” 버튼을 클릭  
     ![](/img/1-2-4.jpg)

   * “Computers”을 클릭하여 검색조건에 포함  
     ![](/img/1-2-5.jpg)

   * OOS 서버를 추가  
     ![](/img/1-2-6.jpg)

   * OOS 상의 UNC URL 확인 - “[http://common-web/op/generate.aspx”](http://common-web/op/generate.aspx”) 로 이동 후 UNC Path를 입력하여 URL을 획득

   * 위의 획득한 URL이 아래처럼 정상적으로 출력됨을 확인

8. URL 설정

   * IIS 내에 Virtual Directory 구성
     * IIS 을 실행
     * 테스트를 위해 “hosting” Context를 사용. 아래와 같이 “Add Virtual Directory..”을 클릭하여 “oofs”폴더를 추가
       ![](/img/1-2-7.jpg)
   * OOS 상의 URL 확인- “[http://common-web/op/generate.aspx”](http://common-web/op/generate.aspx”) 로 이동 후 URL Path를 입력하여 URL을 획득

   * Create Link 클릭 후 Test this link 로 viewer 확인  
     ![](/img/1-2-8.jpg)



