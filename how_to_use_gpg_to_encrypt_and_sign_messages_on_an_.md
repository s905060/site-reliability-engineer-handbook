# How To Use GPG to Encrypt and Sign Messages on an Ubuntu 12.04 VPS

###Introduction

GPG, or GNU Privacy Guard, is a public key cryptography implementation. This allows for the secure transmission of information between parties and can be used to verify that the origin of a message is genuine.

In this guide, we will discuss how GPG works and how to implement it. We will be using an Ubuntu 12.04 VPS for this demonstration, but the tools are available on any modern Linux distribution.

###How Public Key Encryption Works
A problem that many users face is how to communicate securely and validate the identity of the party they are talking to. Many schemes that attempt to answer this question require, at least at some point, the transfer of a password or other identifying credentials, over an insecure medium.

###Ensure That Only the Intended Party Can Read

To get around this issue, GPG relies on a security concept known as public key encryption. The idea is that you can split the encrypting and decrypting stages of the transmission into two separate pieces. That way, you can freely distribute the encrypting portion, as long as you secure the decrypting portion.

This would allow for a one-way message transfer that can be created and encrypted by anyone, but only be decrypted by the designated user (the one with the private decrypting key). If both of the parties create public/private key pairs and give each other their public encrypting keys, they can both encrypt messages to each other.

So in this scenario, each party has their own private key and the other user's public key.

###Validate the Identity of the Sender

Another benefit of this system is that the sender of a message can "sign" the message with their private key. The public key that the receiver has can be used to verify that the signature is actually being sent by the indicated user.

This can prevent a third-party from "spoofing" the identity of someone. It also helps to ensure that the message was transmitted in-full, without damage or file corruption.

###Set Up GPG Keys
GPG should be installed by default on Ubuntu 12.04. If it is not, you can install it with:

```
sudo apt-get install gnupg
```

To begin using GPG to encrypt your communications, you need to create a key pair. You can do this by issuing the following command:

```
gpg --gen-key
```

This will take you through a few questions that will configure your keys.

* Please select what kind of key you want: (1) RSA and RSA (default)

* What keysize do you want? 4096

* Key is valid for? 0

* Is this correct? y

* Real name: your real name here

* Email address: your_email@address.com

* Comment: Optional comment that will be visible in your signature

* Change (N)ame, (C)omment, (E)mail or (O)kay/(Q)uit? O

* Enter passphrase: Enter a secure passphrase here (upper & lower case, digits, symbols)

At this point, it will need to generate the keys using entropy. This is basically a term to describe the amount of unpredictability that exists in a system. GPG uses this entropy to generate a random set of keys.

It is best to open a new terminal and ssh into the VPS while this runs. Install some software, do some work, and just use the machine as much as possible to let it generate the needed entropy.

This process may take a long time, depending on how active you can make your system. There is an article here about how to generate additional entropy with haveged, which may be of use.

###Create a Revocation Certificate

You need to have a way of invalidating your key pair in case there is a security breach, or in case you lose your secret key. There is an easy way of doing this with the GPG software.

This should be done as soon as you make the key pair, not when you need it. This revocation key must be generated ahead of time and kept in a secure, separate location in case your computer is compromised or inoperable. Type:

```
gpg --gen-revoke your_email@address.com
```