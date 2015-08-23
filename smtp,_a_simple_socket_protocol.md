# SMTP, a simple socket protocol

Example 5.7 is an example transaction in SMTP (Simple Mail Transfer Protocol), which is described by
RFC 2821. In the example below, C: lines are sent by a mail transport agent (MTA) sending mail, and S: lines are returned by the MTA receiving it. Text emphasized like this is comments, not part of the actual transaction.

Example 5.7. An SMTP session example
```
C: <client connects to service port 25>
C: HELO snark.thyrsus.com sending host identifies self S: 250 OK Hello snark, glad to meet you receiver acknowledges
C: MAIL FROM <esr@thyrsus.com>
S: 250 <esr@thyrsus.com>... Sender ok
C: RCPT TO: cor@cpmy.com
S: 250 root... Recipient ok
C: DATA
S: 354 Enter mail, end with "." on a line by itself
C: Scratch called. He wants to share
C: a room with us at Balticon.
C: . end of multiline send S: 250 WAA01865 Message accepted for delivery
C: QUIT sender signs off
S: 221 cpmy.com closing connection receiver disconnects 
C: <client hangs up>
```
This is how mail is passed among Internet machines. Note the following features: command-argument format of the requests, responses consisting of a status code followed by an informational message, the fact that the payload of the DATA command is terminated by a line consisting of a single dot.

SMTP is one of the two or three oldest application protocols still in use on the Internet. It is simple, effective, and has withstood the test of time. The traits we have called out here are tropes that recur frequently in other Internet protocols. If there is any single archetype of what a well-designed Internet application protocol looks like, SMTP is it.