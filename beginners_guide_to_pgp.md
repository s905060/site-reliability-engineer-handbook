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

