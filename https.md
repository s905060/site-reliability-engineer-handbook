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