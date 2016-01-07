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

