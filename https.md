# HTTPS

###How does HTTPS actually work?

HTTPS is simply your standard HTTP protocol slathered with a generous layer of delicious SSL/TLS encryption goodness. Unless something goes horribly wrong (and it can), it prevents people like the infamous Eve from viewing or modifying the requests that make up your browsing experience; it’s what keeps your passwords, communications and credit card details safe on the wire between your computer and the servers you want to send this data to. Whilst the little green padlock and the letters “https” in your address bar don’t mean that there isn’t still ample rope for both you and the website you are viewing to hang yourselves elsewhere, they do at least help you communicate securely whilst you do so.

###1. What is HTTPS and what does it do?

HTTPS takes the well-known and understood HTTP protocol, and simply layers a SSL/TLS (hereafter referred to simply as “SSL”) encryption layer on top of it. Servers and clients still speak exactly the same HTTP to each other, but over a secure SSL connection that encrypts and decrypts their requests and responses. The SSL layer has 2 main purposes:

* Verifying that you are talking directly to the server that you think you are talking to
* Ensuring that only the server can read what you send it and only you can read what it sends back

The really, really clever part is that anyone can intercept every single one of the messages you exchange with a server, including the ones where you are agreeing on the key and encryption strategy to use, and still not be able to read any of the actual data you send.

###2. How an SSL connection is established

An SSL connection between a client and server is set up by a handshake, the goals of which are:

* To satisfy the client that it is talking to the right server (and optionally visa versa)
* For the parties to have agreed on a “cipher suite”, which includes which encryption algorithm they will use to exchange data
* For the parties to have agreed on any necessary keys for this algorithm

Once the connection is established, both parties can use the agreed algorithm and keys to securely send messages to each other. We will break the handshake up into 3 main phases - Hello, Certificate Exchange and Key Exchange.

* *Hello* - The handshake begins with the client sending a ClientHello message. This contains all the information the server needs in order to connect to the client via SSL, including the various cipher suites and maximum SSL version that it supports. The server responds with a ServerHello, which contains similar information required by the client, including a decision based on the client’s preferences about which cipher suite and version of SSL will be used.

* *Certificate Exchange* - Now that contact has been established, the server has to prove its identity to the client. This is achieved using its SSL certificate, which is a very tiny bit like its passport. An SSL certificate contains various pieces of data, including the name of the owner, the property (eg. domain) it is attached to, the certificate’s public key, the digital signature and information about the certificate’s validity dates. The client checks that it either implicitly trusts the certificate, or that it is verified and trusted by one of several Certificate Authorities (CAs) that it also implicitly trusts. Much more about this shortly. Note that the server is also allowed to require a certificate to prove the client’s identity, but this typically only happens in very sensitive applications.

* *Key Exchange* - The encryption of the actual message data exchanged by the client and server will be done using a symmetric algorithm, the exact nature of which was already agreed during the Hello phase. A symmetric algorithm uses a single key for both encryption and decryption, in contrast to asymmetric algorithms that require a public/private key pair. Both parties need to agree on this single, symmetric key, a process that is accomplished securely using asymmetric encryption and the server’s public/private keys.

The client generates a random key to be used for the main, symmetric algorithm. It encrypts it using an algorithm also agreed upon during the Hello phase, and the server’s public key (found on its SSL certificate). It sends this encrypted key to the server, where it is decrypted using the server’s private key, and the interesting parts of the handshake are complete. The parties are sufficiently happy that they are talking to the right person, and have secretly agreed on a key to symmetrically encrypt the data that they are about to send each other. HTTP requests and responses can now be sent by forming a plaintext message and then encrypting and sending it. The other party is the only one who knows how to decrypt this message, and so Man In The Middle Attackers are unable to read or modify any requests that they may intercept.

###3. Certificates

####3.1 Trust

At its most basic level, an SSL certificate is simply a text file, and anyone with a text editor can create one. You can in fact trivially create a certificate claiming that you are Google Inc. and that you control the domain gmail.com. If this were the whole story then SSL would be a joke; identity verification would essentially be the client asking the server “are you Google?”, the server replying “er, yeah totally, here’s a piece of paper with ‘I am Google’ written on it” and the client saying “OK great, here’s all my data.” The magic that prevents this farce is in the digital signature, which allows a party to verify that another party’s piece of paper really is legit.

There are 2 sensible reasons why you might trust a certificate:

* If it’s on a list of certificates that you trust implicitly
* If it’s able to prove that it is trusted by the controller of one of the certificates on the above list