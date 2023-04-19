# I want to set up an Apache James mailserver in a Docker container and connect this to Thunderbird.



**I generated the keystore with the following command in the powershell:**

`keytool -genkeypair -alias myalias -keyalg RSA -keysize 2048 -keystore keystore`

  
**Then I started the Docker container with the following command:**

```
docker run --name james_run `
>> -p "25:25" `
>> -p "143:143" `
>> -p "465:465" `
>> -p "993:993" `
>> -v ${PWD}\test\keystore:/root/conf/keystore `
>> apache/james:jpa-latest
```

Then I added the domain example.com and the user user01@example.com with the password test to the domain.
[Screenshot of PowerShell](https://i.stack.imgur.com/ob52X.png) 

---


After this, I wanted to connect Thunderbird to the Docker container with the following configuration:
[Screenshot of Thunderbird configuration 1](https://i.stack.imgur.com/sr8yW.png)
Email Address: user01@example.com
Password: test

Email Address: user01@example.com  
Password: test  

**INCOMING SERVER**   
Protocol: IMAP  
Hostname: localhost  
Port: 143  
Connection security: None  
Authentification method: Normal password   
Username: user01@example.com  

**OUTGOING SERVER**  
Hostname: localhost  
Port: 25  
Connection security: None  
Authentification method: Normal password  
Username: user01@example.com

---


I also tried this configuration for IMAPS and SMTPS:
[Screenshot of Thunderbird configuration 2](https://i.stack.imgur.com/BVWxv.png)

Email Address: user01@example.com  
Password: test  

**INCOMING SERVER**   
Protocol: IMAP  
Hostname: localhost  
Port: 993  
Connection security: SSL/TLS  
Authentification method: Normal password   
Username: user01@example.com  

**OUTGOING SERVER**   
Hostname: localhost  
Port: 465  
Connection security: SSL/TLS  
Authentification method: Normal password  
Username: user01@example.com  

---

**In the logs after the start, everything seems just fine:**

---


# **These are the logs for the first configuration:**
> 11:26:59.796 [INFO ] o.a.j.i.n.ImapChannelUpstreamHandler - Connection established from 172.17.0.1
> 
> 11:26:59.799 [INFO ] o.a.j.p.n.BasicChannelUpstreamHandler - Connection established from 172.17.0.1
> 
> 11:26:59.799 [INFO ] o.a.j.i.n.ImapChannelUpstreamHandler - Connection closed for 172.17.0.1
> 
> 11:26:59.801 [INFO ] o.a.j.p.n.BasicChannelUpstreamHandler - Connection closed for 172.17.0.1
> 
> 11:26:59.801 [INFO ] o.a.j.p.n.BasicChannelUpstreamHandler - Channel closed before we could send in flight messages to the users (ClosedChannelException): null
> 

---

# And for the second configuration:
> 1:27:52.961 [INFO ] o.a.j.i.n.ImapChannelUpstreamHandler - Connection established from 172.17.0.1

> 11:27:52.961 [INFO ] o.a.j.p.n.BasicChannelUpstreamHandler - Connection established from 172.17.0.1
> 
> 11:27:52.962 [INFO ] o.a.j.p.n.BasicChannelUpstreamHandler - Channel closed before we could send in flight messages to the users (ClosedChannelException): null
> 
> 11:27:52.961 [WARN ] o.a.j.i.n.ImapChannelUpstreamHandler - Error while processing imap request
> 
> javax.net.ssl.SSLHandshakeException: Received fatal alert: bad_certificate
> 
> at java.base/sun.security.ssl.Alert.createSSLException(Unknown Source)
> 
> at java.base/sun.security.ssl.Alert.createSSLException(Unknown Source)
> 
> at java.base/sun.security.ssl.TransportContext.fatal(Unknown Source)
> 
> at java.base/sun.security.ssl.Alert$AlertConsumer.consume(Unknown Source)
> 
> at java.base/sun.security.ssl.TransportContext.dispatch(Unknown Source)
> 
> at java.base/sun.security.ssl.SSLTransport.decode(Unknown Source)
> 
> at java.base/sun.security.ssl.SSLEngineImpl.decode(Unknown Source)
> 
> at java.base/sun.security.ssl.SSLEngineImpl.readRecord(Unknown Source)
> 
> at java.base/sun.security.ssl.SSLEngineImpl.unwrap(Unknown Source)
> 
> at java.base/sun.security.ssl.SSLEngineImpl.unwrap(Unknown Source)
> 
> at java.base/javax.net.ssl.SSLEngine.unwrap(Unknown Source)
> 
> at org.jboss.netty.handler.ssl.SslHandler.unwrap(SslHandler.java:1219)
> 
> at org.jboss.netty.handler.ssl.SslHandler.decode(SslHandler.java:852)
> 
> at org.jboss.netty.handler.codec.frame.FrameDecoder.callDecode(FrameDecoder.java:425)
> 
> at org.jboss.netty.handler.codec.frame.FrameDecoder.messageReceived(FrameDecoder.java:303)
> 
> at org.jboss.netty.channel.SimpleChannelUpstreamHandler.handleUpstream(SimpleChannelUpstreamHandler.java:70)
> 
> at org.jboss.netty.channel.DefaultChannelPipeline.sendUpstream(DefaultChannelPipeline.java:564)
> 
> at org.jboss.netty.channel.DefaultChannelPipeline.sendUpstream(DefaultChannelPipeline.java:559)
> 
> at org.jboss.netty.channel.Channels.fireMessageReceived(Channels.java:268)
> 
> at org.jboss.netty.channel.Channels.fireMessageReceived(Channels.java:255)
> 
> at org.jboss.netty.channel.socket.nio.NioWorker.read(NioWorker.java:88)
> 
> at org.jboss.netty.channel.socket.nio.AbstractNioWorker.process(AbstractNioWorker.java:108)
> 
> at org.jboss.netty.channel.socket.nio.AbstractNioSelector.run(AbstractNioSelector.java:337)
> 
> at org.jboss.netty.channel.socket.nio.AbstractNioWorker.run(AbstractNioWorker.java:89)
> 
> at org.jboss.netty.channel.socket.nio.NioWorker.run(NioWorker.java:178)
> 
> at org.jboss.netty.util.ThreadRenamingRunnable.run(ThreadRenamingRunnable.java:108)
> 
> at org.jboss.netty.util.internal.DeadLockProofWorker$1.run(DeadLockProofWorker.java:42)
> 
> at java.base/java.util.concurrent.ThreadPoolExecutor.runWorker(Unknown Source)
> 
> at java.base/java.util.concurrent.ThreadPoolExecutor$Worker.run(Unknown Source)
> 
> at java.base/java.lang.Thread.run(Unknown Source)
> 
> 11:27:52.961 [ERROR] o.a.j.p.n.BasicChannelUpstreamHandler - Unable to process request
> 
> javax.net.ssl.SSLHandshakeException: Received fatal alert: bad_certificate
> 
> at java.base/sun.security.ssl.Alert.createSSLException(Unknown Source)
> 
> at java.base/sun.security.ssl.Alert.createSSLException(Unknown Source)
> 
> at java.base/sun.security.ssl.TransportContext.fatal(Unknown Source)
> 
> at java.base/sun.security.ssl.Alert$AlertConsumer.consume(Unknown Source)
> 
> at java.base/sun.security.ssl.TransportContext.dispatch(Unknown Source)
> 
> at java.base/sun.security.ssl.SSLTransport.decode(Unknown Source)
> 
> at java.base/sun.security.ssl.SSLEngineImpl.decode(Unknown Source)
> 
> at java.base/sun.security.ssl.SSLEngineImpl.readRecord(Unknown Source)
> 
> at java.base/sun.security.ssl.SSLEngineImpl.unwrap(Unknown Source)
> 
> at java.base/sun.security.ssl.SSLEngineImpl.unwrap(Unknown Source)
> 
> at java.base/javax.net.ssl.SSLEngine.unwrap(Unknown Source)
> 
> at org.jboss.netty.handler.ssl.SslHandler.unwrap(SslHandler.java:1219)
> 
> at org.jboss.netty.handler.ssl.SslHandler.decode(SslHandler.java:852)
> 
> at org.jboss.netty.handler.codec.frame.FrameDecoder.callDecode(FrameDecoder.java:425)
> 
> at org.jboss.netty.handler.codec.frame.FrameDecoder.messageReceived(FrameDecoder.java:303)
> 
> at org.jboss.netty.channel.SimpleChannelUpstreamHandler.handleUpstream(SimpleChannelUpstreamHandler.java:70)
> 
> at org.jboss.netty.channel.DefaultChannelPipeline.sendUpstream(DefaultChannelPipeline.java:564)
> 
> at org.jboss.netty.channel.DefaultChannelPipeline.sendUpstream(DefaultChannelPipeline.java:559)
> 
> at org.jboss.netty.channel.Channels.fireMessageReceived(Channels.java:268)
> 
> at org.jboss.netty.channel.Channels.fireMessageReceived(Channels.java:255)
> 
> at org.jboss.netty.channel.socket.nio.NioWorker.read(NioWorker.java:88)
> 
> at org.jboss.netty.channel.socket.nio.AbstractNioWorker.process(AbstractNioWorker.java:108)
> 
> at org.jboss.netty.channel.socket.nio.AbstractNioSelector.run(AbstractNioSelector.java:337)
> 
> at org.jboss.netty.channel.socket.nio.AbstractNioWorker.run(AbstractNioWorker.java:89)
> 
> at org.jboss.netty.channel.socket.nio.NioWorker.run(NioWorker.java:178)
> 
> at org.jboss.netty.util.ThreadRenamingRunnable.run(ThreadRenamingRunnable.java:108)
> 
> at org.jboss.netty.util.internal.DeadLockProofWorker$1.run(DeadLockProofWorker.java:42)
> 
> at java.base/java.util.concurrent.ThreadPoolExecutor.runWorker(Unknown Source)
> 
> at java.base/java.util.concurrent.ThreadPoolExecutor$Worker.run(Unknown Source)
> 
> at java.base/java.lang.Thread.run(Unknown Source)
> 
> 11:27:52.964 [INFO ] o.a.j.i.n.ImapChannelUpstreamHandler - Connection closed for 172.17.0.1
> 
> 11:27:52.964 [INFO ] o.a.j.p.n.BasicChannelUpstreamHandler - Connection closed for 172.17.0.1

---

I understand, that the logs for the second configuration indicate, that there is a problem with the SSL-certificate, but I dont know the reason why. Furthermore I dont understand, why the first configuration is not working. 

Because I dont know how to continue, I am seeking for your help. Thanks in advance!

I expected, that the Thunderbird configurations would work an that I could test sending mails locally with Thunderbird. After this, I want to connect a Spring Boot backend to the mailserver and test sending mails wie Spring Mail.