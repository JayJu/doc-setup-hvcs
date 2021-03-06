# 메일수신 with Spring Integration

> ### Spring Integration 이란?
>
> Spring 기반 어플리케이션 내에서 가벼운 Messaging 을 가능하게 하여 선언적 어댑터를 통해  
> 외부 시스템과의 통합을 지원하는 프로젝트.  
> \* 메시징: 프로그램 간에 빠르고 신뢰할 수 있는 통신을 비동기 방식으로 가능케 하는 전송기술

1. 메일 수신 채널 어댑터

   * 메일 수신을 위해 Spring Integration에서 제공하는 2가지 옵션

     * MailReceiver 인터페이스 구현체인 Pop3MailReciver 와 ImapMailReceiver 사용

     * ```java
       MailReceiver receiver = new Pop3MailReceiver("pop3://usr:pwd@localhost/INBOX");
       ```
     * IMAP의 "idle" 명령을 사용하는  `ImapIdleChannelAdapter` 사용 \(단 메일서버에서 idle 명령을 지원해야 함\)
       ```java
       return IntegrationFlows.from(Mail.imapIdleAdapter(String.format("imaps://%s:%s@imap.gmail.com/INBOX",
       imap.getUsername(), imap.getPassword()))
       .id("imapIn")
       .autoStartup(true)
       ```

       > IDLE 명령이란?  
       > In email technology, IDLE is an IMAP feature described in RFC 2177 that allows a client to indicate to the server that it is ready to accept real-time notifications

2. 구현방식
  * XML을 사용한 선언적 방식과 Java DSL(Domain Service Language)을 사용한 방식이 있음.

