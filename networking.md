#  POP3, the Post Office Protocol

Another one of the classic Internet protocols is POP3, the Post Office Protocol. It is also used for mail transport, but where SMTP is a ‘push’ protocol with transactions initiated by the mail sender, POP3 is a ‘pull’ protocol with transactions initiated by the mail receiver. Internet users with intermittent access (like dial-up connections) can let their mail pile up on a mail-drop machine, then use a POP3 connection to pull mail up the wire to their personal machines.

Example 5.8 is an example POP3 session. In the example below, C: lines are sent by the client, and S: lines by the mail server. Observe the many similarities with SMTP. This protocol is also textual and line-oriented, sends payload message sections terminated by a line consisting of a single dot followed by line terminator, and even uses the same exit command, QUIT. Like SMTP, each client operation is acknowledged by a reply line that begins with a status code and includes an informational message meant for human eyes.

Example 5.8. A POP3 example session

```
C: <client connects to service port 110>
S: +OK POP3 server ready <1896.697170952@mailgate.dobbs.org> C: USER bob
S: +OK bob
C: PASS redqueen
S: +OK bob's maildrop has 2 messages (320 octets)
C: STAT
S: +OK 2 320
C: LIST
S: +OK 2 messages (320 octets)
S: 1 120
S: 2 200
S: .
C: RETR 1
S: +OK 120 octets
S: <the POP3 server sends the text of message 1>
S: .
C: DELE 1
S: +OK message 1 deleted
C: RETR 2
S: +OK 200 octets
S: <the POP3 server sends the text of message 2>
S: .
C: DELE 2
S: +OK message 2 deleted
C: QUIT
S: +OK dewey POP3 server signing off (maildrop empty)
C: <client hangs up>
```

There are a few differences. The most obvious one is that POP3 uses status tokens rather than SMTP's 3- digit status codes. Of course the requests have different semantics. But the family resemblance (one we'll have more to say about when we discuss the generic Internet metaprotocol later in this chapter) is clear.