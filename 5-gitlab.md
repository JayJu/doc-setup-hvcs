# Gitlab 설치 및 환경구성

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
     cd /tmp
     curl -LO https://packages.gitlab.com/install/repositories/gitlab/gitlab-ce/script.deb.sh
     ```
   * 1

   * 1

   * 

4. 방화벽 정책 조정

5. 초기설정

6. 


