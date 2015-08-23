# IMAP, the Internet Message Access Protocol

To complete our triptych of Internet application protocol examples, we'll look at IMAP, another post office protocol designed in a slightly different style. See Example 5.9; as before, C: lines are sent by the client, and S: lines by the mail-server. Text emphasized like this is comments, not part of the actual transaction.

Example 5.9. An IMAP session example
```
C: <client connects to service port 143>
S: * OK example.com IMAP4rev1 v12.264 server ready
C: A0001 USER "frobozz" "xyzzy"
S: * OK User frobozz authenticated
C: A0002 SELECT INBOX
S: * 1 EXISTS
S: * 1 RECENT
S: * FLAGS (\Answered \Flagged \Deleted \Draft \Seen)
S: * OK [UNSEEN 1] first unseen message in /var/spool/mail/esr S: A0002 OK [READ-WRITE] SELECT completed
C: A0003 FETCH 1 RFC822.SIZE
S: * 1 FETCH (RFC822.SIZE 2545)
S: A0003 OK FETCH completed
C: A0004 FETCH 1 BODY[HEADER]
S: * 1 FETCH (RFC822.HEADER {1425}
<server sends 1425 octets of message payload>
S: )
S: A0004 OK FETCH completed
C: A0005 FETCH 1 BODY[TEXT]
S: * 1 FETCH (BODY[TEXT] {1120}
<server sends 1120 octets of message payload>
S: )
S: * 1 FETCH (FLAGS (\Recent \Seen))
S: A0005 OK FETCH completed
C: A0006 LOGOUT
S: * BYE example.com IMAP4rev1 server terminating connection S: A0006 OK LOGOUT completed
C: <client hangs up>
```
IMAP delimits payloads in a slightly different way. Instead of ending the payload with a dot, the payload length is sent just before it. This increases the burden on the server a little bit (messages have to be composed ahead of time, they can't just be streamed up after the send initiation) but makes life easier for the client, which can tell in advance how much storage it will need to allocate to buffer the message for processing as a whole.
Also, notice that each response is tagged with a sequence label supplied by the request; in this example they have the form A000n, but the client could have generated any token into that slot. This feature makes it possible for IMAP commands to be streamed to the server without waiting for the responses; a state machine in the client can then simply interpret the responses and payloads as they come back. This technique cuts down on latency.
IMAP (which was designed to replace POP3) is an excellent example of a mature and powerful Internet