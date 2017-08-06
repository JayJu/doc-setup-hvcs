# Jenkins 설치 및 환경구성

1. 설치환경

   * Ubuntu 16.04.1
   * 참고자료: [How To Install Jenkins on Ubuntu 16.04](https://www.digitalocean.com/community/tutorials/how-to-install-jenkins-on-ubuntu-16-04)
   
2. JDK 설치
  * [JDK설치 및 환경설정](/chapter1/default/01-jdk.md)
  
3. Gradle 설치
  * [Gradle 설치 및 환경설정](/chapter1/default/02-gradle.md)

4. 사용자 추가 및 권한 설정

   * 사용자 추가
   * ```
     # useradd jenkins -m -d /home/jenkins
     # passwd jenkins
     ```
   * sudoer 등록
   * ```
     # usermod -aG sudo jenkins
     ```
  * gradle profile 추가
  * ```
  $ su - jenkins
  $ vi ./.profile
    ## gradle
    PATH="/opt/gradle/gradle-4.0.2/bin:$PATH"
  $ source ./.profile
  ```

5. Jenkins 설치\(jenkins로 로그인 후 진행\)

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

6. Jenkins 시작

   * 포트변경

   * ```
     $ sudo vi /etc/default/jenkins
     // port 부분을 변경.
     HTTP_PORT=8080 -> HTTP_PORT=9000
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

7. 방화벽 오픈

   * 방화벽 정책 수정 - 9000포트 추가

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

8. Jenkins 기본 설정

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
  * [Gitlab Continuous Integration on Jenkins](http://blog.ljdelight.com/gitlab-continuous-integration-on-jenkins/)
  * [Introduction to Continuous Integration with JHipster](http://blog.ippon.tech/continuous-integration-with-jhipster/)
  

1. GitLab 설정
  * 사용자 계정과 access token 생성
  ![](/img/ch1/sub1/1-1-7.png)
  
2. Jenkins 설정
  * Global Tool Configuration 설정
  ![](/img/ch1/sub1/1-1-14.png)

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
  
  * build triggers 섹션
  * "Build when a change is pushed to GitLab CI.." 체크
  * "Advenced.." 에 key generate..
  * 위 URL과 generated 된 Secret token으로  GitLab에서 Jenkins Webhook 생성해야함
  ![](/img/ch1/sub1/1-1-13.png)

  * Pipeline 섹션
  ![](/img/ch1/sub1/1-1-17.png)
  * Definition 에 "Pipeline script fro SCM" 선택
  * SCM에 git 선택
  * Repository URL에 git project 주소 입력
  > Credential 에 GitLab에 API Token 생성 하여 입력 해야 되는데 add 가 안되는 
  > 현상이 있음. 임시로 credential을 gitlab user/password form으로 생성하여 임시조치
  > ![](/img/ch1/sub1/1-1-15.png)
  * Branch에 master -> 이후 Release로 변경예정
  * Script Path: Jenkinsfile -> 소스에 Jenkinsfile 을 생성해여 소스레벨에서 배포스크립트를 관리하는 방식으로 진행
  ```
    #!/usr/bin/env groovy

    node {
        stage('checkout') {
            checkout scm
        }

        gitlabCommitStatus('build') {
            stage('check java') {
                sh "java -version"
            }

            stage('clean') {
                sh "chmod +x gradlew"
                sh "./gradlew clean --no-daemon"
            }

            stage('npm install') {
                sh "./gradlew npmInstall -PnodeInstall --no-daemon"
            }

            stage('backend tests') {
                try {
                //    sh "./gradlew test -PnodeInstall --no-daemon"
                } catch(err) {
                    throw err
                } finally {
                //    junit '**/build/**/TEST-*.xml'
                }
            }

            stage('frontend tests') {
                try {
                //    sh "./gradlew npm_test -PnodeInstall --no-daemon"
                } catch(err) {
                    throw err
                } finally {
                //    junit '**/build/test-results/karma/TESTS-*.xml'
                }
            }

            stage('packaging') {
            //    sh "./gradlew bootRepackage -x test -Pprod -PnodeInstall --no-daemon"
                sh "./gradlew bootRepackage -Pdev -PnodeInstall --no-daemon"
                archiveArtifacts artifacts: '**/build/libs/*.war', fingerprint: true
            }
        }
    }
  ```
  
4. GibLab Webhook 생성
  * Pipeline Job 생성 단계의 URL과 generated Secret token 복사
  ![](/img/ch1/sub1/1-1-16.png)
  
5. 

  





