# Kerberos

###In a nutshell
Basically, Kerberos comes down to just this:

* a protocol for authentication
* uses tickets to authenticate
*avoids storing passwords locally or sending them over the internet
* involves a trusted 3rd-party
* built on symmetric-key cryptography

You have a ticket – your proof of identity encrypted with a secret key for the particular service requested – on your local machine (creation of a ticket is described below); so long as it’s valid, you can access the requested service that is within a Kerberos realm.

Typically, this is used within corporate/internal environments. Perhaps you want to access your internal payroll site to review what little bonus your boss has given you. Rather than re-entering your user/password credentials, your ticket (cached on your system) is used to authenticate allowing for single sign-on.

Your ticket is refreshed when you sign on to your computer, or when you kinit USER within your terminal.

For the trivia-loving folks, Kerberos’ name comes from Greek mythology, the three-headed guard dog of Hades. It’s pretty fitting since it takes a third-party (a Key Distribution Center) to authenticate between a client and a service or host machine.

###Kerberos Realm
Admins create realms – Kerberos realms – that will encompass all that is available to access. Granted, you may not have access to certain services or host machines that is defined within the policy management – developers should not access anything finance related, stuff like that. But a realm defines what Kerberos manages in terms of who can access what.

Your machine, the Client, lives within this realm, as well as the service or host you want to request and the Key Distribution Center, KDC (no, not the KGB, although I always think of that, too). In the following example, I separate out the Authentication Server and the Ticket Granting Server, but both are within the KDC.

![](Kerb.001.jpg)

###To keep in the back of your mind
You may want to come back up here after you read through the gritty details on how the example works.

When requesting access to a service or host, three interactions take place between you and:

* the Authentication Server
* the Ticket Granting Server
* the Service or host machine that you’re wanting access to.
Other important points: