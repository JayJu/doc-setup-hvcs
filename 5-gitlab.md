# GitLab 설치 및 환경구성

1. 설치환경

   * Ubuntu 16.04.1

   * 참고자료: [How To Install and Configure GitLab on Ubuntu 16.04](https://www.digitalocean.com/community/tutorials/how-to-install-and-configure-gitlab-on-ubuntu-16-04)

2. 의존관계 패키지 설치

   * ```
     $ sudo apt-get update
     $ sudo apt-get install ca-certificates curl openssh-server postfix
     ```
   * postfix 설치 시 "Internet Site" 선택, 다음 단계에 서버 ip나 도메인 입력

3. GitLab 설치

   * /tmp 이동 후 실행

   * ```
     $ cd /tmp
     $ curl -LO https://packages.gitlab.com/install/repositories/gitlab/gitlab-ce/script.deb.sh
     ```
   * GitLab 레파지토리 셋업

   * ```
     $ sudo bash /tmp/script.deb.sh
     ```
   * GitLab CE\(Community Edition\) 설치

   * ```
     $ sudo apt-get install gitlab-ce
     ```

     ![](/img/1-5-1.png)

   * 서버 IP 확인 및 수정

   * ```
     $ vi /etc/gitlab/gitlab.rb
     external_url 항목의 서버IP 확인 및 수정
     ```
   * reconfigure 실행

   * ```
     $ sudo gitlab-ctl reconfigure
     ```
   * 서비스 재시작 - root로 진행

   * ```
     $ gitlab-ctl stop
     $ gitlab-ctl start
     $ gitlab-ctl status
     $ gitlab-ctl tail
     ```

4. 방화벽 정책 조정

   * 방화벽 정책 수정 - 80, 8080포트 추가

   * ```
     $ sudo vi /etc/iptables/rules.v4
     ```
   * rules.v4 에 아래 내용을 추가 후 저장/닫기

   * ```
     -A RH-Firewall-1-INPUT -p tcp -m state --state NEW -m tcp --dport 80 -j ACCEPT
     -A RH-Firewall-1-INPUT -p tcp -m state --state NEW -m tcp --dport 8080 -j ACCEPT
     ```
   * 방화벽 재시작

   * ```
     $ sudo netfilter-persistent reload
     ```
   * 방화벽 확인

   * ```
     $ sudo iptables -L --line-numbers
     ```

5. 초기설정

   * 웹 인터페이스 접속

   * ```
     http://gitlab_domain_or_IP
     ```
   * 관리자 비밀번호 설정  
     ![](/img/1-5-2.png)

   * 관리자 로그인 \(root / 이전 설정한 비밀번호\)  
     ![](/img/1-5-3.png)

   * 관리자 프로파일 수정 \(Settings\)  
     ![](/img/1-5-4.png)

   * 이메일 업데이트 후 confirmation mail 확인

     * confirmation link 클릭 시 target url 이 localhost 로 나올 경우 서버 ip 재설정하기

   * Account 이름 변경

     * Profile Setting &gt; Account -&gt; username 변경  
       ![](/img/1-5-6.png)

   * ss

6. 


