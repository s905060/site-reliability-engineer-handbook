# Kerberos

###In a nutshell
Basically, Kerberos comes down to just this:

* a protocol for authentication
* uses tickets to authenticate
*avoids storing passwords locally or sending them over the internet
* involves a trusted 3rd-party
* built on symmetric-key cryptography
* 
You have a ticket – your proof of identity encrypted with a secret key for the particular service requested – on your local machine (creation of a ticket is described below); so long as it’s valid, you can access the requested service that is within a Kerberos realm.

Typically, this is used within corporate/internal environments. Perhaps you want to access your internal payroll site to review what little bonus your boss has given you. Rather than re-entering your user/password credentials, your ticket (cached on your system) is used to authenticate allowing for single sign-on.

Your ticket is refreshed when you sign on to your computer, or when you kinit USER within your terminal.