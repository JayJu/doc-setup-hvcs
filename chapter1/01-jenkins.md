# Jenkins 설치 및 환경구성

1. 설치환경

   * Ubuntu 16.04.1
   * 참고자료: [How To Install Jenkins on Ubuntu 16.04](https://www.digitalocean.com/community/tutorials/how-to-install-jenkins-on-ubuntu-16-04)

2. 사용자 추가 및 권한 설정

   * 사용자 추가
   * ```
     # useradd jenkins -m -d /home/jenkins
     # passwd jenkins
     ```
   * sudoer 등록
   * ```
     # usermod -aG sudo jenkins
     ```

3. Jenkins 설치\(jenkins로 로그인 후 진행\)

   * 레파지토리 키 등록
   * ```
     $ wget -q -O - https://pkg.jenkins.io/debian/jenkins-ci.org.key | sudo apt-key add -
     ```
   * sources.list 에 데비안 패키지 레파지토리 추가
   * ```
     $ echo deb http://pkg.jenkins.io/debian-stable binary/ | sudo tee /etc/apt/sources.list.d/jenkins.list
     ```
   * apt-get 업데이트
   * ```
     $ sudo apt-get update
     ```
   * Jenkins 와 dependencies 설치
   * ```
     $ sudo apt-get install jenkins
     ```

4. Jenkins 시작

   * 포트변경

   * ```
     $ sudo vi /etc/default/jenkins
     // port 부분을 변경.
     HTTP_PORT=8080 -> HTTP_PORT=9999
     ```
   * systemctl을 사용하여 Jenkins 시작

   * ```
     $ sudo systemctl start jenkins
     ```
   * status 확인

   * ```
     $ sudo systemctl status jenkins
      jenkins.service - LSB: Start Jenkins at boot time
        Loaded: loaded (/etc/init.d/jenkins; bad; vendor preset: enabled)
        Active: active (exited) since Fri 2017-07-07 09:47:29 KST; 2min 1s ago
          Docs: man:systemd-sysv-generator(8)
     ```

5. 방화벽 오픈

   * 방화벽 정책 수정 - 8080포트 추가

   * ```
     $ sudo vi /etc/iptables/rules.v4
     ```
   * rules.v4에 아래 내용 추가 후 저장/닫기

   * ```
     -A RH-Firewall-1-INPUT -p tcp -m state --state NEW -m tcp --dport 9000 -j ACCEPT
     ```
   * 방화벽 재시작

   * ```
     $ sudo netfilter-persistent reload
     ```
   * 정책확인

   * ```
     $ sudo iptables -L
     ```

6. Jenkins 기본 설정

   * Jenkins 접속

   * ```
     http://ip_address_or_domain_name:8080
     ```
   * 관리자 비번 입력 후 Unlock Jenkins

   * ```
     $ sudo cat /var/lib/jenkins/secrets/initialAdminPassword
     ```

     ![](/img/ch1/sub1/1-1-1.jpg)

   * "Install suggested plusins" 선택  
     ![](/img/ch1/sub1/1-1-2.jpg)

   * 관리자 계정 생성 - 정보 입력 후 "Save and Finish"  
     ![](/img/ch1/sub1/1-1-4.jpg)

   * "Start using Jenkins"  
     ![](/img/ch1/sub1/1-1-5.jpg)![](/img/ch1/sub1/1-1-6.jpg)


---

# Jenkins - Gitlab 연동
* 참고문서
  * [Jenkins CI service](https://docs.gitlab.com/ee/integration/jenkins.html#configure-gitlab-users)
  * [gitlab-plusin intruction](https://github.com/jenkinsci/gitlab-plugin#using-it-with-a-job)
  

1. GitLab 설정
  * 사용자 계정과 access token 생성
  ![](/img/ch1/sub1/1-1-7.png)
  
2. Jenkins 설정
  * 플러그인 설치
    * [Gitlab](https://wiki.jenkins.io/display/JENKINS/GitLab+Plugin)
    * [Javadoc](https://wiki.jenkins.io/display/JENKINS/Javadoc+Plugin)
    * [Pipeline-jenkins2.x에 포함됨](https://wiki.jenkins.io/display/JENKINS/Pipeline+Plugin)
  ![](/img/ch1/sub1/1-1-8.png)

  * GitLab 접근을 위한 credential 생성
    Jenkins CI 가 build status를 GitLab API로 보내기 위한 설정
    GibLab에서 생성한 사용자 access token을 등록
    ![](/img/ch1/sub1/1-1-9.png)

  * GitLab connection 설정
  ![](/img/ch1/sub1/1-1-10.png)
  
3. Jenkins Build Job 생성
  GitLab의 mgerge request 를 처리할 job 생성
  * 폴더 생성
  ![](/img/ch1/sub1/1-1-11.png)

  * 폴더 하위에 pipeline job 생성
  ![](/img/ch1/sub1/1-1-12.png)
  
  * build triggers 하위에 "Build when a change is pushed to GitLab" 체크
  * "Advenced.." 에 key generate..
  ![](/img/ch1/sub1/1-1-13.png)

  * Pipeline script 섹션에 아래 스크립트 추가
  ```
    node() {
      try {
        // Pull the source and test a merge
        checkout changelog: true, poll: true, scm: [
        $class: 'GitSCM',
        branches: [[name: "origin/${env.gitlabSourceBranch}"]],
        doGenerateSubmoduleConfigurations: false,
        extensions: [[
        $class: 'PreBuildMerge',
        options: [
        fastForwardMode: 'FF',
        mergeRemote: 'origin',
        mergeStrategy: 'default',
        mergeTarget: "${env.gitlabTargetBranch}"
        ]
        ]],
        submoduleCfg: [],
        userRemoteConfigs: [[
        credentialsId: 'gitlab-jenkins-user-credentials',
        name: 'origin',
        url: "${env.gitlabSourceRepoHttpUrl}"
        ]]
        ]
        
        // Start the build
        load "Jenkinsfile.common"
        
        } catch (Exception e) {
          updateGitlabCommitStatus(name: 'build', state: 'failed')
          addGitLabMRComment comment: "Something unexpected happened. Inspect Jenkins logs."
          throw e
        }
      }
  ```

  





