# What is SSL and what are Certificates?

### 1.2. What is SSL and what are Certificates?

The Secure Socket Layer protocol was created by Netscape to ensure secure transactions between web servers and browsers. The protocol uses a third party, a Certificate Authority (CA), to identify one end or both end of the transactions. This is in short how it works.

A browser requests a secure page (usually https://).

The web server sends its public key with its certificate.

The browser checks that the certificate was issued by a trusted party (usually a trusted root CA), that the certificate is still valid and that the certificate is related to the site contacted.

The browser then uses the public key, to encrypt a random symmetric encryption key and sends it to the server with the encrypted URL required as well as other encrypted http data.

The web server decrypts the symmetric encryption key using its private key and uses the symmetric key to decrypt the URL and http data.

The web server sends back the requested html document and http data encrypted with the symmetric key.

The browser decrypts the http data and html document using the symmetric key and displays the information.

Several concepts have to be understood here.

###  1.2.1. Private Key/Public Key:

The encryption using a private key/public key pair ensures that the data can be encrypted by one key but can only be decrypted by the other key pair. This is sometime hard to understand, but believe me it works. The keys are similar in nature and can be used alternatively: what one key encrypts, the other key pair can decrypt. The key pair is based on prime numbers and their length in terms of bits ensures the difficulty of being able to decrypt the message without the key pairs. The trick in a key pair is to keep one key secret (the private key) and to distribute the other key (the public key) to everybody. Anybody can send you an encrypted message, that only you will be able to decrypt. You are the only one to have the other key pair, right? In the opposite , you can certify that a message is only coming from you, because you have encrypted it with you private key, and only the associated public key will decrypt it correctly. Beware, in this case the message is not secured you have only signed it. Everybody has the public key, remember!

One of the problem left is to know the public key of your correspondent. Usually you will ask him to send you a non confidential signed message that will contains his publick key as well as a certificate.

Message-->[Public Key]-->Encrypted Message-->[Private Key]-->Message
1.2.2. The Certificate:

How do you know that you are dealing with the right person or rather the right web site. Well, someone has taken great length (if they are serious) to ensure that the web site owners are who they claim to be. This someone, you have to implicitly trust: you have his/her certificate loaded in your browser (a root Certificate). A certificate, contains information about the owner of the certificate, like e-mail address, owner's name, certificate usage, duration of validity, resource location or Distinguished Name (DN) which includes the Common Name (CN) (web site address or e-mail address depending of the usage) and the certificate ID of the person who certifies (signs) this information. It contains also the public key and finally a hash to ensure that the certificate has not been tampered with. As you made the choice to trust the person who signs this certificate, therefore you also trust this certificate. This is a certificate trust tree or certificate path. Usually your browser or application has already loaded the root certificate of well known Certification Authorities (CA) or root CA Certificates. The CA maintains a list of all signed certificates as well as a list of revoked certificates. A certificate is insecure until it is signed, as only a signed certificate cannot be modified. You can sign a certificate using itself, it is called a self signed certificate. All root CA certificates are self signed.
```
Certificate: 
    Data: 
        Version: 3 (0x2) 
        Serial Number: 1 (0x1) 
        Signature Algorithm: md5WithRSAEncryption 
        Issuer: C=FJ, ST=Fiji, L=Suva, O=SOPAC, OU=ICT, CN=SOPAC Root CA/Email=administrator@sopac.org 
        Validity 
            Not Before: Nov 20 05:47:44 2001 GMT 
            Not After : Nov 20 05:47:44 2002 GMT 
        Subject: C=FJ, ST=Fiji, L=Suva, O=SOPAC, OU=ICT, CN=www.sopac.org/Email=administrator@sopac.org 
        Subject Public Key Info: 
            Public Key Algorithm: rsaEncryption  
            RSA Public Key: (1024 bit) 
                Modulus (1024 bit): 
                    00:ba:54:2c:ab:88:74:aa:6b:35:a5:a9:c1:d0:5a: 
                    9b:fb:6b:b5:71:bc:ef:d3:ab:15:cc:5b:75:73:36: 
                    b8:01:d1:59:3f:c1:88:c0:33:91:04:f1:bf:1a:b4: 
                    7a:c8:39:c2:89:1f:87:0f:91:19:81:09:46:0c:86: 
                    08:d8:75:c4:6f:5a:98:4a:f9:f8:f7:38:24:fc:bd: 
                    94:24:37:ab:f1:1c:d8:91:ee:fb:1b:9f:88:ba:25: 
                    da:f6:21:7f:04:32:35:17:3d:36:1c:fb:b7:32:9e: 
                    42:af:77:b6:25:1c:59:69:af:be:00:a1:f8:b0:1a: 
                    6c:14:e2:ae:62:e7:6b:30:e9 
                Exponent: 65537 (0x10001) 
         X509v3 extensions: 
             X509v3 Basic Constraints: 
                 CA:FALSE 
             Netscape Comment: 
                 OpenSSL Generated Certificate
             X509v3 Subject Key Identifier:
                 FE:04:46:ED:A0:15:BE:C1:4B:59:03:F8:2D:0D:ED:2A:E0:ED:F9:2F 
             X509v3 Authority Key Identifier:
                 keyid:E6:12:7C:3D:A1:02:E5:BA:1F:DA:9E:37:BE:E3:45:3E:9B:AE:E5:A6 
                 DirName:/C=FJ/ST=Fiji/L=Suva/O=SOPAC/OU=ICT/CN=SOPAC Root CA/Email=administrator@sopac.org 
                 serial:00
    Signature Algorithm: md5WithRSAEncryption
        34:8d:fb:65:0b:85:5b:e2:44:09:f0:55:31:3b:29:2b:f4:fd: 
        aa:5f:db:b8:11:1a:c6:ab:33:67:59:c1:04:de:34:df:08:57: 
        2e:c6:60:dc:f7:d4:e2:f1:73:97:57:23:50:02:63:fc:78:96: 
        34:b3:ca:c4:1b:c5:4c:c8:16:69:bb:9c:4a:7e:00:19:48:62: 
        e2:51:ab:3a:fa:fd:88:cd:e0:9d:ef:67:50:da:fe:4b:13:c5: 
        0c:8c:fc:ad:6e:b5:ee:40:e3:fd:34:10:9f:ad:34:bd:db:06: 
        ed:09:3d:f2:a6:81:22:63:16:dc:ae:33:0c:70:fd:0a:6c:af:
        bc:5a 
-----BEGIN CERTIFICATE----- 
MIIDoTCCAwqgAwIBAgIBATANBgkqhkiG9w0BAQQFADCBiTELMAkGA1UEBhMCRkox 
DTALBgNVBAgTBEZpamkxDTALBgNVBAcTBFN1dmExDjAMBgNVBAoTBVNPUEFDMQww 
CgYDVQQLEwNJQ1QxFjAUBgNVBAMTDVNPUEFDIFJvb3QgQ0ExJjAkBgkqhkiG9w0B 
CQEWF2FkbWluaXN0cmF0b3JAc29wYWMub3JnMB4XDTAxMTEyMDA1NDc0NFoXDTAy 
MTEyMDA1NDc0NFowgYkxCzAJBgNVBAYTAkZKMQ0wCwYDVQQIEwRGaWppMQ0wCwYD 
VQQHEwRTdXZhMQ4wDAYDVQQKEwVTT1BBQzEMMAoGA1UECxMDSUNUMRYwFAYDVQQD 
Ew13d3cuc29wYWMub3JnMSYwJAYJKoZIhvcNAQkBFhdhZG1pbmlzdHJhdG9yQHNv 
cGFjLm9yZzCBnzANBgkqhkiG9w0BAQEFAAOBjQAwgYkCgYEAulQsq4h0qms1panB 
0Fqb+2u1cbzv06sVzFt1cza4AdFZP8GIwDORBPG/GrR6yDnCiR+HD5EZgQlGDIYI 
2HXEb1qYSvn49zgk/L2UJDer8RzYke77G5+IuiXa9iF/BDI1Fz02HPu3Mp5Cr3e2 
JRxZaa++AKH4sBpsFOKuYudrMOkCAwEAAaOCARUwggERMAkGA1UdEwQCMAAwLAYJ 
YIZIAYb4QgENBB8WHU9wZW5TU0wgR2VuZXJhdGVkIENlcnRpZmljYXRlMB0GA1Ud
DgQWBBT+BEbtoBW+wUtZA/gtDe0q4O35LzCBtgYDVR0jBIGuMIGrgBTmEnw9oQLl 
uh/anje+40U+m67lpqGBj6SBjDCBiTELMAkGA1UEBhMCRkoxDTALBgNVBAgTBEZp 
amkxDTALBgNVBAcTBFN1dmExDjAMBgNVBAoTBVNPUEFDMQwwCgYDVQQLEwNJQ1Qx 
FjAUBgNVBAMTDVNPUEFDIFJvb3QgQ0ExJjAkBgkqhkiG9w0BCQEWF2FkbWluaXN0 
cmF0b3JAc29wYWMub3JnggEAMA0GCSqGSIb3DQEBBAUAA4GBADSN+2ULhVviRAnw 
VTE7KSv0/apf27gRGsarM2dZwQTeNN8IVy7GYNz31OLxc5dXI1ACY/x4ljSzysQb 
xUzIFmm7nEp+ABlIYuJRqzr6/YjN4J3vZ1Da/ksTxQyM/K1ute5A4/00EJ+tNL3b 
Bu0JPfKmgSJjFtyuMwxw/Qpsr7xa
-----END CERTIFICATE-----
```

As You may have noticed, the certificate contains the reference to the issuer, the public key of the owner of this certificate, the dates of validity of this certificate and the signature of the certificate to ensure this certificate hasen't been tampered with. The certificate does not contain the private key as it should never be transmitted in any form whatsoever. This certificate has all the elements to send an encrypted message to the owner (using the public key) or to verify a message signed by the author of this certificate.

###  1.2.3. The Symmetric key:

Well, Private Key/Public Key encryption algorithms are great, but they are not usually practical. It is asymmetric because you need the other key pair to decrypt. You can't use the same key to encrypt and decrypt. An algorithm using the same key to decrypt and encrypt is deemed to have a symmetric key. A symmetric algorithm is much faster in doing its job than an asymmetric algorithm. But a symmetric key is potentially highly insecure. If the enemy gets hold of the key then you have no more secret information. You must therefore transmit the key to the other party without the enemy getting its hands on it. As you know, nothing is secure on the Internet. The solution is to encapsulate the symmetric key inside a message encrypted with an asymmetric algorithm. You have never transmitted your private key to anybody, then the message encrypted with the public key is secure (relatively secure, nothing is certain except death and taxes). The symmetric key is also chosen randomly, so that if the symmetric secret key is discovered then the next transaction will be totally different.

```
Symetric Key-->[Public Key]-->Encrypted Symetric Key-->[Private Key]-->Symetric Key
1.2.4. Encryption algorithm:
```

There are several encryption algorithms available, using symmetric or asymmetric methods, with keys of various lengths. Usually, algorithms cannot be patented, if Henri Poincare had patented his algorithms, then he would have been able to sue Albert Einstein... So algorithms cannot be patented except mainly in USA. OpenSSL is developed in a country where algorithms cannot be patented and where encryption technology is not reserved to state agencies like military and secret services. During the negotiation between browser and web server, the applications will indicate to each other a list of algorithms that can be understood ranked by order of preference. The common preferred algorithm is then chosen. OpenSSL can be compiled with or without certain algorithms, so that it can be used in many countries where restrictions apply.

### 1.2.5. The Hash:

A hash is a number given by a hash function from a message. This is a one way function, it means that it is impossible to get the original message knowing the hash. However the hash will drastically change even for the slightest modification in the message. It is therefore extremely difficult to modify a message while keeping its original hash. It is also called a message digest. Hash functions are used in password mechanisms, in certifying that applications are original (MD5 sum), and in general in ensuring that any message has not been tampered with. It seems that the Internet Enginering Task Force (IETF) prefers SHA1 over MD5 for a number of technical reasons (Cf RFC2459 7.1.2 and 7.1.3).

### 1.2.6. Signing:

Signing a message, means authentifying that you have yourself assured the authenticity of the message (most of the time it means you are the author, but not neccesarily). The message can be a text message, or someone else's certificate. To sign a message, you create its hash, and then encrypt the hash with your private key, you then add the encrypted hash and your signed certificate with the message. The recipient will recreate the message hash, decrypts the encrypted hash using your well known public key stored in your signed certificate, check that both hash are equals and finally check the certificate.

The other advantage of signing your messages is that you transmit your public key and certificate automatically to all your recipients.

There are usually 2 ways to sign, encapsulating the text message inside the signature (with delimiters), or encoding the message altogether with the signature. This later form is a very simple encryption form as any software can decrypt it if it can read the embedded public key. The advantage of the first form is that the message is human readable allowing any non complaint client to pass the message as is for the user to read, while the second form does not even allow to read part of the message if it has been tampered with.

### 1.2.7. PassPhrase:

“A passprase is like a password except it is longer”. In the early days passwords on Unix system were limited to 8 characters, so the term passphrase for longer passwords. Longer is the password harder it is to guess. Nowadays Unix systems use MD5 hashes which have no limitation in length of the password.

### 1.2.8. Public Key Infrastructure

The Public Key Infrastructure (PKI) is the software management system and database system that allows to sign certifcate, keep a list of revoked certificates, distribute public key,... You can usually access it via a website and/or ldap server. There will be also some people checking that you are who you are... For securing individual applications, you can use any well known commercial PKI as their root CA certificate is most likely to be inside your browser/application. The problem is for securing e-mail, either you get a generic type certificate for your e-mail or you must pay about USD100 a year per certificate/e-mail address. There is also no way to find someone's public key if you have never received a prior e-mail with his certificate (including his public key).