# Beginners’ Guide To PGP

If you are new to Bitcoin it’s likely you’ve heard some terms thrown around by Bitcoiners that you have no idea what they mean―PGP, Tor, VPN, OTR, etc. In most cases these are referring to various technologies that people use to protect their data and communications.

This is the first installment of what will likely be a series of articles aimed at introducing new Bitcoiners to these and other technologies that you can use to enhance your privacy and keep sensitive information away from the prying eyes of governments and data thieves.

We’re going to start off this series by introducing you to PGP, which is by far the most widely used encryption software available and a critical component to online privacy. Whether you’re purchasing drugs from Silk Road or just sending emails to friends and family, it’s something with which even casual internet users should familiarize themselves.

###What is PGP?

PGP stands for Pretty Good Privacy. At it’s core, it is an internet standard (called OpenPGP) used for data encryption and digital signatures. Software that employs this standard is available in both a free, open source version produced by the Free Software Foundation called the GNU Privacy Guard (or GPG for short) as well as a low-cost commercial version.

Let’s take a moment to understand some of the basics of how it works. In conventional encryption, a secret key is used to transform plaintext (the unencrypted data) into unreadable ciphertext. The same key is also used to decrypt the ciphertext and reveal the plaintext. While this process works well for encrypting data stored on your hard drive, it has its drawbacks for use in communication. For one, you need to somehow communicate the secret key to the other party. But how to do this securely? After all, the reason you are using encryption is because you don’t believe your communication channel is secure. You could meet in person and exchange the secret key offline, but that isn’t very convenient. Protocols have been developed to allow for secure exchange of keys across insecure communication channels, but they tend to work better for real-time chat than, say, sending encrypted emails.

PGP makes use of public-key encryption. One key (a public key) is used to encrypt the data and a separate key (the private key) is used to decrypt it.

![](public-key-cryptography.jpg)

As a new user, you will generate a new public-private key pair. Just like the names suggest, you’ll share your public key with others so that they can send you encrypted messages or files, while keeping your private key secret so that you can decrypt the data. The process by which the key pair is generated makes it impossible (given current technology and knowledge of mathematics) for an attacker to derive your private key from the public key.

###Use Cases

The most obvious use case for this type of encryption is email. Anyone who has your public key can send you encrypted emails which only you can view. Likewise, you can send encrypted emails to your contacts by first downloading their public keys. In a future post we’ll provide a more thorough tutorial demonstrating how to set up an email client to work with PGP. What you need to keep in mind, however, is only the body of the email will be encrypted. The subject and metadata (to, from, cc, and timestamp) will still be visible to anyone snooping on your emails.

You aren’t limited to just encrypting emails either. Buyers at anonymous marketplaces like Silk Road frequently download their merchant’s public key and use it to encrypt their shipping address so that only the merchant view it. Edward Snowden persuaded journalist Glenn Greenwald to set up PGP prior to leaking the top secret classified documents that revealed the depths of the NSA’s spying operation. You can encrypt whole folders and files with your own public key to protect them from attackers who may gain access to your hard drive. In other words, PGP can be used in just about every conceivable case where strong encryption is needed.

###Digital Signatures

Another feature of public-key cryptography is it allows for the creation of something called digital signatures. Much like your real life signature, a digital signature can be used to authenticate data but with the added benefit of being completely unforgeable (again given the current state of cryptography).

A digital signature is created by a mathematical algorithm which combines your private key with data you wish to “sign”. The validity of the signature can by verified by anyone simply by checking it with your public key.

![](digital-signature.jpg)

In the above diagram you see that the plaintext is run through a hash function to produce a message digest which is then signed with your private key. What this process ensures is that a signed document cannot be altered without invalidating the signature, allowing people to not only check the document’s authenticity but also the integrity of the data. Just to give an example, suppose you sign a 10,000 word document. If someone were change even a single punctuation in that document, the signature would show as invalid. To see why digital signatures are useful let’s consider a few examples:

Returning to Edward Snowden, suppose the NSA had intercepted the classified documents before they reached Glenn Greenwald. The NSA could have removed the sensitive data, replaced it with disinformation, then forwarded it along to Greenwald. The reason this didn’t happen is because Snowden signed the data with his private key before sending it along. This allowed Greenwald to use Snowden’s public key to verify the files were unaltered. If the NSA tried to switch out some information, the signature would have shown as invalid.

Digital signatures are also extremely useful in verifying the integrity of software. A great example here would be Bitcoin wallets. Given the security implications, you want to be able to trust that the wallet you download is legitimate and wont leak information that would allow someone to steal your bitcoins. While all Bitcoin wallets are open source, unless you check and compile the source code yourself, you will most likely download a pre-compiled version that could contain malicious lines of code. Software developers will typically sign the software and provide a link to download the public key used for signing. With Bitcoin-Qt, lead developer Gavin Andresen signs new versions with his PGP key. Simply by checking the signature with his public key you can guarantee you’ve downloaded a legitimate copy.

###How Secure Is It?

If all of this is new to you, you’re likely wondering how secure is the encryption used in PGP. Can we really trust it to protect us from from the NSA and its $52.9 billion black budget? All I can really say is that the cryptographic algorithms used in PGP are all part of the public domain have been heavily vetted by the community of experts. At this point in time there are no feasible attacks known to the general public or academia. It’s certainly possible that the NSA has access to highly advanced math that isn’t publicly known, but even there the best attacks typically don’t reveal the plaintext, rather they just make the keys slightly easier to brute force. The fact that the NSA has pressured Google, Microsoft, Apple etc. into giving them backdoors into their systems seems to be prima facie evidence that they can’t break commercial cryptographic algorithms.

###Getting Started

The first thing you need to do to get started is download and install GPG. If you use the Ubuntu operating system you’re in luck, you already have it. It can be found in the apps menu as “Passwords and Keys”.

Windows users can download Gpg4win here.

And Mac users should download the GPG Suite for OS X from here.

![](Selection_005.png)
![](gnupg-kleopatra-home.png)
![](gka-key-list.1375965203.png)

In each of these operating systems you can access GPG as well as a number of advanced options from the command line, but as a new user, you’re better off learning to use the GUI for now.

###Generating A New Certificate

In PGP a “certificate” is essentially a public key with extra data attached to help others verify that the key really belongs to you. In practice this is usually your name, email address and one or more digital signatures from others (more on that later).

Depending on your operating system, you’ll generate a new certificate by clicking “New”, “New Certificate”, or “New PGP Key”.

At minimum you will have to enter your name, email address, and a strong password that you will use for decrypting and signing data. In the advanced options menu you can select your encryption algorithm (RSA, DSA/ElGamal), key size (in bits), and an expiration date if you want your certificate to expire. The defaults here should suffice for our purposes. The differences are technical and unlikely to affect your overall security (just don’t reduce to the key size).

Once this process is complete you will have generated a new certificate and private key. You can click on “export” to save your public key to a .asc file for distributing to others, or you can copy the text of the key block and share it with people that way. A typical public key block will look like this:

```
-----BEGIN PGP PUBLIC KEY BLOCK-----
Version: PGP Universal 2.9.1 (Build 347)

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
2vOMWRC5GQUM8TU1YkCeAuhsxSj3ew==
=dgnf
-----END PGP PUBLIC KEY BLOCK-----
```

