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

You will be asked for the reason that it is being revoked. You can choose any of the available options, but since this is being done ahead of time, you won't have the specifics.

You will then be offered to supply a comment and finally, to confirm the selections.

Afterwards, a revocation certificate will be generated to the screen. Copy and paste this to a secure location, or print it for later use:

```
Revocation certificate created.

Please move it to a medium which you can hide away; if Mallory gets
access to this certificate he can use it to make your key unusable.
It is smart to print this certificate and store it away, just in case
your media become unreadable.  But have some caution:  The print system of
your machine might store the data and make it available to others!
-----BEGIN PGP PUBLIC KEY BLOCK-----
Version: GnuPG v1.4.11 (GNU/Linux)
Comment: A revocation certificate should follow

iQIfBCABAgAJBQJSTxNSAh0AAAoJEIKHahUxGx+E15EP/1BL2pCTqSG9IYbz4CMN
bCW9HgeNpb24BK9u6fAuyH8aieLVD7It80LnSg/+PgG9t4KlzUky5sOoo54Qc3rD
H+JClu4oaRpq25vWd7+Vb2oOwwd/27Y1KRt6TODwK61z20XkGPU2NJ/ATPn9yIR9
4B10QxqqQSpQeB7rr2+Ahsyl5jefswwXmduDziZlZqf+g4lv8lZlJ8C3+GKv06fB
FJwE6XO4Y69LNAeL+tzSE9y5lARKVMfqor/wS7lNBdFzo3BE0w68HN6iD+nDbo8r
xCdQ9E2ui9os/5yf9Y3Uzky1GTLmBhTqPnl8AOyHHLTqqOT47arpwRXXDeNd4B7C
DiE0p1yevG6uZGfhVAkisNfi4VrprTx73NGwyahCc3gO/5e2GnKokCde/NhOknci
Wl4oSL/7a3Wx8h/XKeNvkiurInuZugFnZVKbW5kvIbHDWJOanEQnLJp3Q2tvebrr
BBHyiVeQiEwOpFRvBuZW3znifoGrIc7KMmuEUPvA243xFcRTO3G1D1X9B3TTSlc/
o8jOlv6y2pcdBfp4aUkFtunE4GfXmIfCF5Vn3TkCyBV/Y2aW/fpA3Y+nUy5hPhSt
tprTYmxyjzSvaIw5tjsgylMZ48+qp/Awe34UWL9AWk3DvmydAerAxLdiK/80KJp0
88qdrRRgEuw3qfBJbNZ7oM/o
=isbs
-----END PGP PUBLIC KEY BLOCK-----
```

###How To Import Other Users' Public Keys
GPG would be pretty useless if you could not accept other public keys from people you wished to communicate with.

You can import someone's public key in a variety of ways. If you've obtained a public key from someone in a text file, GPG can import it with the following command:
```
gpg --import name_of_pub_key_file
```

There is also the possibility that the person you are wishing to communicate with has uploaded their key to a public key server. These key servers are used to house people's public keys from all over the world.

A popular key server that syncs its information with a variety of other servers is the MIT public key server. You can search for people by their name or email address by going here in your web browser:

```
http://pgp.mit.edu/
```

You can also search the key server from within GPG by typing the following:
```
gpg --keyserver pgp.mit.edu  --search-keys search_parameters
```

###How To Verify and Sign Keys
While you can freely distribute your generated public key file and people can use this to contact you in an encrypted way, there is still an issue of trust in the initial public key transmission.

###Verify the Other Person's Identity

How do you know that the person giving you the public key is who they say they are? In some cases, this may be simple. You may be sitting right next to the person with your laptops both open and exchanging keys. This should be a pretty secure way of identifying that you are receiving the correct, legitimate key.

But there are many other circumstances where such personal contact is not possible. You may not know the other party personally, or you may be separated by physical distance. If you never want to communicate over insecure channels, verification of the public key could be problematic.

Luckily, instead of verifying the entire public keys of both parties, you can simply compare the "fingerprint" derived from these keys. This will give you a reasonable assurance that you both are using the same public key information.

You can get the fingerprint of a public key by typing:
```
gpg --fingerprint your_email@address.com
```
```
pub   4096R/311B1F84 2013-10-04
      Key fingerprint = CB9E C70F 2421 AF06 7D72  F980 8287 6A15 311B 1F84
uid                  Test User <test.user@address.com>
sub   4096R/8822A56A 2013-10-04
```

This will produce a much more manageable string of numbers to compare. You can compare this string with the person themselves, or someone else who has access to that person.

###Sign Their Key

Signing a key tells your software that you trust the key that you have been provided with and that you have verified that it is associated with the person in question.

To sign a key that you've imported, simply type:
```
gpg --sign-key email@example.com
```

After you've signed the key, it means you verify that you trust the person is who he/she claims to be. This can help other people decide whether to trust that person too. If someone trusts you, and they see that you've signed this person's key, they may be more likely to trust their identity too.

You should allow the person whose key you are signing the advantages of your trusted relationship by sending them back the signed key. You can do this by typing:
```
gpg --export --armor email@example.com
```

You'll have to type in your passphrase again. Afterwards, their public key, signed by you, will be spit out on the screen. Send them this, so that they can benefit from gaining your "stamp of approval" when interacting with others.

When they receive this new, signed key, they can import it, adding on the signing information you've generated, into their GPG database. They can do this by typing:
```
gpg --import file_name
```

###How To Make Your Public Key Highly Available
There is not really anything malicious that can happen if unknown people have your public key.

Because of this, it may be beneficial to make your public key easily available. People can then easily find your information to send you secure messages, from the very first communication.

You can send anyone your public key by requesting it from the GPG system:
```
gpg --armor --export your_email@address.com
```
```
-----BEGIN PGP PUBLIC KEY BLOCK-----
Version: GnuPG v1.4.11 (GNU/Linux)

mQINBFJPCuABEACiog/sInjg0O2SqgmG1T8n9FroSTdN74uGsRMHHAOuAmGLsTse
9oxeLQpN+r75Ko39RVE88dRcW710fPY0+fjSXBKhpN+raRMUKJp4AX9BJd00YA/4
EpD+8cDK4DuLlLdn1x0q41VUsznXrnMpQedRmAL9f9bL6pbLTJhaKeorTokTvdn6
5VT3pb2o+jr6NETaUxd99ZG/osPar9tNThVLIIzG1nDabcTFbMB+w7wOJuhXyTLQ
JBU9xmavTM71PfV6Pkh4j1pfWImXc1D8dS+jcvKeXInBfm2XZsfOCesk12YnK3Nc
u1Xe1lxzSt7Cegum4S/YuxmYoh462oGZ7FA4Cr2lvAPVpO9zmgQ8JITXiqYg2wB3
. . .
```

You can then copy and paste this or send this in an appropriate medium.

If you want to publish your key to a key server, you can do it manually through the forms available on most of the server sites.

Another option is to do this through the GPG interface.

Look up your key ID by typing:
```
gpg --list-keys your_email@address.com
```

The highlighted portion is your key ID. It is a short way to reference the key to the internal software.
```
pub   4096R/311B1F84 2013-10-04
uid                  Test User <test.user@address.com>
sub   4096R/8822A56A 2013-10-04
```

To upload your key to a certain key server, you can then use this syntax:
```
gpg --send-keys --keyserver pgp.mit.edu key_id
```

###Encrypt and Decrypt Messages with GPG
You can easily encrypt and decrypt messages after you have configured your keys with the other party.


###Encrypt Messages

You can encrypt messages using the "--encrypt" flag for GPG. The basic syntax would be:
```
gpg --encrypt --sign --armor -r person@email.com name_of_file
```

The parameters basically encrypt the email, sign it with your private key to guarantee that it is coming from you, and generates the message in a text format instead of raw bytes.

You should also include a second "-r" recipient with your own email address if you want to be able to read the message ever. This is because the message will be encrypted with each person's public key, and will only be able to be decrypted with the associated private key.

So if it was only encrypted with the other party's public key, you would not be able to view the message again, unless you somehow obtained their private key. Adding yourself as a second recipient encrypts the message two separate times, one for each recipient.

###Decrypt Messages

When you receive a message, simply call GPG on the message file:
```
gpg file_name
```

The software will prompt you as necessary.

If you have the message as a raw text stream, you can copy and paste it after you just typing gpg without any arguments. You can press "CTRL-D" to signify the end of the message and GPG will decrypt it for you.

