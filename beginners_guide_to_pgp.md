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