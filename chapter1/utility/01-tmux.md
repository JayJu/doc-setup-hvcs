# Tmux 설치 및 환경설정
1. Tmux 란?

2. Tmux 설치
    1. 
    2. 명령어
        * 세션 생성
        * 세션명 변경: tmux rename-session [-t current-name] [new-name]
        
3. Tmux Util
    1) Tmuxinator
        * gem > ruby > tmuxinator 설치
        ```
        $ sudo apt-get install gem 
        $ sudo apt-get install ruby
        $ sudo gem install tmuxinator (tmux 세션 밖에서 수행)
        ```
        
        * profile에 editor 설정
        ```
        $ vi .profile
            export EDITOR='vim' <--추가
        $ source ./.profile
        ```
        
        * 프로젝트파일 생성
        ```
        $ tmuxinator new hvcs
        
        ```