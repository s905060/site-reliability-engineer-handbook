# Beginners’ Guide To PGP

If you are new to Bitcoin it’s likely you’ve heard some terms thrown around by Bitcoiners that you have no idea what they mean―PGP, Tor, VPN, OTR, etc. In most cases these are referring to various technologies that people use to protect their data and communications.

This is the first installment of what will likely be a series of articles aimed at introducing new Bitcoiners to these and other technologies that you can use to enhance your privacy and keep sensitive information away from the prying eyes of governments and data thieves.

We’re going to start off this series by introducing you to PGP, which is by far the most widely used encryption software available and a critical component to online privacy. Whether you’re purchasing drugs from Silk Road or just sending emails to friends and family, it’s something with which even casual internet users should familiarize themselves.

###What is PGP?

PGP stands for Pretty Good Privacy. At it’s core, it is an internet standard (called OpenPGP) used for data encryption and digital signatures. Software that employs this standard is available in both a free, open source version produced by the Free Software Foundation called the GNU Privacy Guard (or GPG for short) as well as a low-cost commercial version.

Let’s take a moment to understand some of the basics of how it works. In conventional encryption, a secret key is used to transform plaintext (the unencrypted data) into unreadable ciphertext. The same key is also used to decrypt the ciphertext and reveal the plaintext. While this process works well for encrypting data stored on your hard drive, it has its drawbacks for use in communication. For one, you need to somehow communicate the secret key to the other party. But how to do this securely? After all, the reason you are using encryption is because you don’t believe your communication channel is secure. You could meet in person and exchange the secret key offline, but that isn’t very convenient. Protocols have been developed to allow for secure exchange of keys across insecure communication channels, but they tend to work better for real-time chat than, say, sending encrypted emails.

PGP makes use of public-key encryption. One key (a public key) is used to encrypt the data and a separate key (the private key) is used to decrypt it.