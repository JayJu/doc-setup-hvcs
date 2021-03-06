# 방화벽 정책

1. 기본정책

   * 정책 확인

   * ```
     # iptables -L --line-numbers
     ```
   * ssh, ftp 허용

   * ```
     # vi /etc/iptables/rules.v4
     -A RH-Firewall-1-INPUT -p tcp -m state --state NEW -m tcp --dport 21 -j ACCEPT
     -A RH-Firewall-1-INPUT -p tcp -m state --state NEW -m tcp --dport 22 -j ACCEPT
     ```
   * 서비스 적용

   * ```
     # netfilter-persistent reload
     ```

2. 미디어 서버
   * Kurento Media Server Port : 8888
   * ```
     -A RH-Firewall-1-INPUT -p tcp -m state --state NEW -m tcp --dport 8888 -j ACCEPT
     ```
   * Streaming Port: udp all
   * ```
     -A RH-Firewall-1-INPUT -p udp -j ACCEPT
     ```
     
3. 어플리케이션 서버
  * Jenkins
  ```
  -A RH-Firewall-1-INPUT -p tcp -m state --state NEW -m tcp --dport 7000 -j ACCEPT
  ```
  * SonarQube
  ```
  -A RH-Firewall-1-INPUT -p tcp -m state --state NEW -m tcp --dport 8000 -j ACCEPT
  ```
  * Webpack
  ```
  -A RH-Firewall-1-INPUT -p tcp -m state --state NEW -m tcp --dport 9000 -j ACCEPT
  ```
  * HVCS - Https
  ```
  -A RH-Firewall-1-INPUT -p tcp -m state --state NEW -m tcp --dport 8443 -j ACCEPT
  ```
  * Samba
  ```
  -A RH-Firewall-1-INPUT -s 220.230.125.71 -p tcp -m state --state NEW -m tcp --dport 139 -j ACCEPT
  -A RH-Firewall-1-INPUT -s 220.230.125.71 -p tcp -m state --state NEW -m tcp --dport 445 -j ACCEPT
  -A RH-Firewall-1-INPUT -s 220.230.125.71 -p udp -m state --state NEW -m udp --dport 137 -j ACCEPT
  -A RH-Firewall-1-INPUT -s 220.230.125.71 -p udp -m state --state NEW -m udp --dport 138 -j ACCEPT
  -A RH-Firewall-1-INPUT -s 220.230.125.71 -p udp -m state --state NEW -m udp --dport 445 -j ACCEPT
  ```
  
4. DB 서버

  * Gitlab
  ```
-A RH-Firewall-1-INPUT -p tcp -m state --state NEW -m tcp --dport 80 -j ACCEPT
```
  * Mariadb
  ```
-A RH-Firewall-1-INPUT -p tcp -m state --state NEW -m tcp --dport 3306 -j ACCEPT
```



