# How To Kerberos

System security and integrity within a network can be unwieldy. It can occupy the time of several administrators just to keep track of what services are being run on a network and the manner in which these services are used.

Further, authenticating users to network services can prove dangerous when the method used by the protocol is inherently insecure, as evidenced by the transfer of unencrypted passwords over a network using the traditional FTP and Telnet protocols.

Kerberos is a way to eliminate the need for protocols that allow unsafe methods of authentication, thereby enhancing overall network security.

###What is Kerberos?

Kerberos is a network authentication protocol created by MIT, and uses symmetric-key cryptography[18] to authenticate users to network services, which means passwords are never actually sent over the network.

Consequently, when users authenticate to network services using Kerberos, unauthorized users attempting to gather passwords by monitoring network traffic are effectively thwarted.

###Advantages of Kerberos

Most conventional network services use password-based authentication schemes. Such schemes require a user to authenticate to a given network server by supplying their username and password. Unfortunately, the transmission of authentication information for many services is unencrypted. For such a scheme to be secure, the network has to be inaccessible to outsiders, and all computers and users on the network must be trusted and trustworthy.

Even if this is the case, a network that is connected to the Internet can no longer be assumed to be secure. Any attacker who gains access to the network can use a simple packet analyzer, also known as a packet sniffer, to intercept usernames and passwords, compromising user accounts and the integrity of the entire security infrastructure.

The primary design goal of Kerberos is to eliminate the transmission of unencrypted passwords across the network. If used properly, Kerberos effectively eliminates the threat that packet sniffers would otherwise pose on a network.

###Disadvantages of Kerberos

Although Kerberos removes a common and severe security threat, it may be difficult to implement for a variety of reasons:

* Migrating user passwords from a standard UNIX password database, such as /etc/passwd or /etc/shadow, to a Kerberos password database can be tedious, as there is no automated mechanism to perform this task. Refer to Question 2.23 in the online Kerberos FAQ:

  http://www.nrl.navy.mil/CCS/people/kenh/kerberos-faq.html

* Kerberos has only partial compatibility with the Pluggable Authentication Modules (PAM) system used by most Red Hat Enterprise Linux servers. Refer to Section 42.6.4, “Kerberos and PAM” for more information about this issue.

* Kerberos assumes that each user is trusted but is using an untrusted host on an untrusted network. Its primary goal is to prevent unencrypted passwords from being transmitted across that network. However, if anyone other than the proper user has access to the one host that issues tickets used for authentication — called the key distribution center (KDC) — the entire Kerberos authentication system is at risk.

* For an application to use Kerberos, its source must be modified to make the appropriate calls into the Kerberos libraries. Applications modified in this way are considered to be Kerberos-aware, or kerberized. For some applications, this can be quite problematic due to the size of the application or its design. For other incompatible applications, changes must be made to the way in which the server and client communicate. Again, this may require extensive programming. Closed-source applications that do not have Kerberos support by default are often the most problematic.

* Kerberos is an all-or-nothing solution. If Kerberos is used on the network, any unencrypted passwords transferred to a non-Kerberos aware service is at risk. Thus, the network gains no benefit from the use of Kerberos. To secure a network with Kerberos, one must either use Kerberos-aware versions of all client/server applications that transmit passwords unencrypted, or not use any such client/server applications at all.

###Kerberos Terminology

Kerberos has its own terminology to define various aspects of the service. Before learning how Kerberos works, it is important to learn the following terms.

**authentication server (AS)**
 > A server that issues tickets for a desired service which are in turn given to users for access to the service. The AS responds to requests from clients who do not have or do not send credentials with a request. It is usually used to gain access to the ticket-granting server (TGS) service by issuing a ticket-granting ticket (TGT). The AS usually runs on the same host as the key distribution center (KDC).

**ciphertext**
>Encrypted data.

**client**
>An entity on the network (a user, a host, or an application) that can receive a ticket from Kerberos.

**credentials**
>A temporary set of electronic credentials that verify the identity of a client for a particular service. Also called a ticket.

**credential cache or ticket file**
>A file which contains the keys for encrypting communications between a user and various network services. Kerberos 5 supports a framework for using other cache types, such as shared memory, but files are more thoroughly supported.

**crypt hash**
>A one-way hash used to authenticate users. These are more secure than using unencrypted data, but they are still relatively easy to decrypt for an experienced cracker.

**GSS-API**
>The Generic Security Service Application Program Interface (defined in RFC-2743 published by The Internet Engineering Task Force) is a set of functions which provide security services. This API is used by clients and services to authenticate to each other without either program having specific knowledge of the underlying mechanism. If a network service (such as cyrus-IMAP) uses GSS-API, it can authenticate using Kerberos.

**hash**
>Also known as a hash value. A value generated by passing a string through a hash function. These values are typically used to ensure that transmitted data has not been tampered with.

**hash function**
>A way of generating a digital "fingerprint" from input data. These functions rearrange, transpose or otherwise alter data to produce a hash value.

**key**
>Data used when encrypting or decrypting other data. Encrypted data cannot be decrypted without the proper key or extremely good fortune on the part of the cracker.

**key distribution center (KDC)**
>A service that issues Kerberos tickets, and which usually run on the same host as the ticket-granting server (TGS).

**keytab (or key table)**
>A file that includes an unencrypted list of principals and their keys. Servers retrieve the keys they need from keytab files instead of using kinit. The default keytab file is /etc/krb5.keytab. The KDC administration server, /usr/kerberos/sbin/kadmind, is the only service that uses any other file (it uses /var/kerberos/krb5kdc/kadm5.keytab).

**kinit**
>The kinit command allows a principal who has already logged in to obtain and cache the initial ticket-granting ticket (TGT). Refer to the kinit man page for more information.

**principal (or principal name)**
>The principal is the unique name of a user or service allowed to authenticate using Kerberos. A principal follows the form root[/instance]@REALM. For a typical user, the root is the same as their login ID. The instance is optional. If the principal has an instance, it is separated from the root with a forward slash ("/"). An empty string ("") is considered a valid instance (which differs from the default NULL instance), but using it can be confusing. All principals in a realm have their own key, which for users is derived from a password or is randomly set for services.

**realm**
>A network that uses Kerberos, composed of one or more servers called KDCs and a potentially large number of clients.

**service**
>A program accessed over the network.

**ticket**
>A temporary set of electronic credentials that verify the identity of a client for a particular service. Also called credentials.

**ticket-granting server (TGS)**
>A server that issues tickets for a desired service which are in turn given to users for access to the service. The TGS usually runs on the same host as the KDC.

**ticket-granting ticket (TGT)**
>A special ticket that allows the client to obtain additional tickets without applying for them from the KDC.

**unencrypted password**
>A plain text, human-readable password.

###How Kerberos Works

Kerberos differs from username/password authentication methods. Instead of authenticating each user to each network service, Kerberos uses symmetric encryption and a trusted third party (a KDC), to authenticate users to a suite of network services. When a user authenticates to the KDC, the KDC sends a ticket specific to that session back to the user's machine, and any Kerberos-aware services look for the ticket on the user's machine rather than requiring the user to authenticate using a password.

When a user on a Kerberos-aware network logs in to their workstation, their principal is sent to the KDC as part of a request for a TGT from the Authentication Server. This request can be sent by the log-in program so that it is transparent to the user, or can be sent by the kinit program after the user logs in.

The KDC then checks for the principal in its database. If the principal is found, the KDC creates a TGT, which is encrypted using the user's key and returned to that user.

The login or kinit program on the client then decrypts the TGT using the user's key, which it computes from the user's password. The user's key is used only on the client machine and is not transmitted over the network.

The TGT is set to expire after a certain period of time (usually ten to twenty-four hours) and is stored in the client machine's credentials cache. An expiration time is set so that a compromised TGT is of use to an attacker for only a short period of time. After the TGT has been issued, the user does not have to re-enter their password until the TGT expires or until they log out and log in again.

Whenever the user needs access to a network service, the client software uses the TGT to request a new ticket for that specific service from the TGS. The service ticket is then used to authenticate the user to that service transparently.


---
Warning
The Kerberos system can be compromised if a user on the network authenticates against a non-Kerberos aware service by transmitting a password in plain text. The use of non-Kerberos aware services is highly discouraged. Such services include Telnet and FTP. The use of other encrypted protocols, such as SSH or SSL-secured services, however, is preferred, although not ideal.

---

##### Note
Kerberos depends on the following network services to function correctly.

  * Approximate clock synchronization between the machines on the network.
  * A clock synchronization program should be set up for the network, such as ntpd. Refer to /usr/share/doc/ntp-<version-number>/index.html for details on setting up Network Time Protocol servers (where <version-number> is the version number of the ntp package installed on your system).
  * Domain Name Service (DNS).
  * You should ensure that the DNS entries and hosts on the network are all properly configured. Refer to the Kerberos V5 System Administrator's Guide in /usr/share/doc/krb5-server-<version-number> for more information (where <version-number> is the version number of the krb5-server package installed on your system).

###Kerberos and PAM

Kerberos-aware services do not currently make use of Pluggable Authentication Modules (PAM) — these services bypass PAM completely. However, applications that use PAM can make use of Kerberos for authentication if the pam_krb5 module (provided in the pam_krb5 package) is installed. The pam_krb5 package contains sample configuration files that allow services such as login and gdm to authenticate users as well as obtain initial credentials using their passwords. If access to network servers is always performed using Kerberos-aware services or services that use GSS-API, such as IMAP, the network can be considered reasonably safe.


---

#####Tip
Administrators should be careful not to allow users to authenticate to most network services using Kerberos passwords. Many protocols used by these services do not encrypt the password before sending it over the network, destroying the benefits of the Kerberos system. For example, users should not be allowed to authenticate to Telnet services with the same password they use for Kerberos authentication.

---

###Configuring a Kerberos 5 Server

When setting up Kerberos, install the KDC first. If it is necessary to set up slave servers, install the master first.

To configure the first Kerberos KDC, follow these steps:

1. Ensure that time synchronization and DNS are functioning correctly on all client and server machines before configuring Kerberos. Pay particular attention to time synchronization between the Kerberos server and its clients. If the time difference between the server and client is greater than five minutes (this is configurable in Kerberos 5), Kerberos clients can not authenticate to the server. This time synchronization is necessary to prevent an attacker from using an old Kerberos ticket to masquerade as a valid user.

 It is advisable to set up a Network Time Protocol (NTP) compatible client/server network even if Kerberos is not being used. Red Hat Enterprise Linux includes the ntp package for this purpose. Refer to /usr/share/doc/ntp-<version-number>/index.html (where <version-number> is the version number of the ntp package installed on your system) for details about how to set up Network Time Protocol servers, and http://www.ntp.org for more information about NTP.

2. Install the krb5-libs, krb5-server, and krb5-workstation packages on the dedicated machine which runs the KDC. This machine needs to be very secure — if possible, it should not run any services other than the KDC.

3. Edit the /etc/krb5.conf and /var/kerberos/krb5kdc/kdc.conf configuration files to reflect the realm name and domain-to-realm mappings. A simple realm can be constructed by replacing instances of EXAMPLE.COM and example.com with the correct domain name — being certain to keep uppercase and lowercase names in the correct format — and by changing the KDC from kerberos.example.com to the name of the Kerberos server. By convention, all realm names are uppercase and all DNS hostnames and domain names are lowercase. For full details about the formats of these configuration files, refer to their respective man pages.

4. Create the database using the kdb5_util utility from a shell prompt:

 `/usr/kerberos/sbin/kdb5_util create -s`
The create command creates the database that stores keys for the Kerberos realm. The -s switch forces creation of a stash file in which the master server key is stored. If no stash file is present from which to read the key, the Kerberos server (krb5kdc) prompts the user for the master server password (which can be used to regenerate the key) every time it starts.

5. Edit the /var/kerberos/krb5kdc/kadm5.acl file. This file is used by kadmind to determine which principals have administrative access to the Kerberos database and their level of access. Most organizations can get by with a single line:

 `*/admin@EXAMPLE.COM  *`
 
 Most users are represented in the database by a single principal (with a NULL, or empty, instance, such as joe@EXAMPLE.COM). In this configuration, users with a second principal with an instance of admin (for example, joe/admin@EXAMPLE.COM) are able to wield full power over the realm's Kerberos database.

 After kadmind has been started on the server, any user can access its services by running kadmin on any of the clients or servers in the realm. However, only users listed in the kadm5.acl file can modify the database in any way, except for changing their own passwords.
 
 Type the following kadmin.local command at the KDC terminal to create the first principal:

 `/usr/kerberos/sbin/kadmin.local -q "addprinc username/admin"`

 ---

 ######Note
 The kadmin utility communicates with the kadmind server over the network, and uses Kerberos to handle authentication. Consequently, the first principal must already exist before connecting to the server over the network to administer it. Create the first principal with the kadmin.local command, which is specifically designed to be used on the same host as the KDC and does not use Kerberos for authentication.

 ---

6. Start Kerberos using the following commands:
 ```
 /sbin/service krb5kdc start
 /sbin/service kadmin start
 /sbin/service krb524 start
 ```

7. Add principals for the users using the addprinc command within kadmin. kadmin and kadmin.local are command line interfaces to the KDC. As such, many commands — such as addprinc — are available after launching the kadmin program. Refer to the kadmin man page for more information.

8. Verify that the KDC is issuing tickets. First, run kinit to obtain a ticket and store it in a credential cache file. Next, use klist to view the list of credentials in the cache and use kdestroy to destroy the cache and the credentials it contains.

  ---

 ######Note
 By default, kinit attempts to authenticate using the same system login username (not the Kerberos server). If that username does not correspond to a principal in the Kerberos database, kinit issues an error message. If that happens, supply kinit with the name of the correct principal as an argument on the command line (kinit <principal>).

 ---
 
Once these steps are completed, the Kerberos server should be up and running.
 
###Configuring a Kerberos 5 Client

Setting up a Kerberos 5 client is less involved than setting up a server. At a minimum, install the client packages and provide each client with a valid krb5.conf configuration file. While ssh and slogin are the preferred method of remotely logging in to client systems, Kerberized versions of rsh and rlogin are still available, though deploying them requires that a few more configuration changes be made.

1. Be sure that time synchronization is in place between the Kerberos client and the KDC. Refer to Section 42.6.5, “Configuring a Kerberos 5 Server” for more information. In addition, verify that DNS is working properly on the Kerberos client before configuring the Kerberos client programs.

2. Install the krb5-libs and krb5-workstation packages on all of the client machines. Supply a valid /etc/krb5.conf file for each client (usually this can be the same krb5.conf file used by the KDC).

3. Before a workstation in the realm can use Kerberos to authenticate users who connect using ssh or Kerberized rsh or rlogin, it must have its own host principal in the Kerberos database. The sshd, kshd, and klogind server programs all need access to the keys for the host service's principal. Additionally, in order to use the kerberized rsh and rlogin services, that workstation must have the xinetd package installed.

 Using kadmin, add a host principal for the workstation on the KDC. The instance in this case is the hostname of the workstation. Use the -randkey option for the kadmin's addprinc command to create the principal and assign it a random key:

 `addprinc -randkey host/blah.example.com`

 Now that the principal has been created, keys can be extracted for the workstation by running kadmin on the workstation itself, and using the ktadd command within kadmin:

 `ktadd -k /etc/krb5.keytab host/blah.example.com`

4. To use other kerberized network services, they must first be started. Below is a list of some common kerberized services and instructions about enabling them:

 * ssh — OpenSSH uses GSS-API to authenticate users to servers if the client's and server's configuration both have GSSAPIAuthentication enabled. If the client also has GSSAPIDelegateCredentials enabled, the user's credentials are made available on the remote system.

 * rsh and rlogin — To use the kerberized versions of rsh and rlogin, enable klogin, eklogin, and kshell.

 * Telnet — To use kerberized Telnet, krb5-telnet must be enabled.

 * FTP — To provide FTP access, create and extract a key for the principal with a root of ftp. Be certain to set the instance to the fully qualified hostname of the FTP server, then enable gssftp.

 * IMAP — To use a kerberized IMAP server, the cyrus-imap package uses Kerberos 5 if it also has the cyrus-sasl-gssapi package installed. The cyrus-sasl-gssapi package contains the Cyrus SASL plugins which support GSS-API authentication. Cyrus IMAP should function properly with Kerberos as long as the cyrus user is able to find the proper key in /etc/krb5.keytab, and the root for the principal is set to imap (created with kadmin).

 * An alternative to cyrus-imap can be found in the dovecot package, which is also included in Red Hat Enterprise Linux. This package contains an IMAP server but does not, to date, support GSS-API and Kerberos.

 * CVS — To use a kerberized CVS server, gserver uses a principal with a root of cvs and is otherwise identical to the CVS pserver.

### Domain-to-Realm Mapping

When a client attempts to access a service running on a particular server, it knows the name of the service (host) and the name of the server (foo.example.com), but because more than one realm may be deployed on your network, it must guess at the name of the realm in which the service resides.

By default, the name of the realm is taken to be the DNS domain name of the server, upper-cased.

```
foo.example.org → EXAMPLE.ORG
foo.example.com → EXAMPLE.COM
foo.hq.example.com → HQ.EXAMPLE.COM
```

In some configurations, this will be sufficient, but in others, the realm name which is derived will be the name of a non-existant realm. In these cases, the mapping from the server's DNS domain name to the name of its realm must be specified in the domain_realm section of the client system's krb5.conf. For example:

```
[domain_realm]
.example.com = EXAMPLE.COM
example.com = EXAMPLE.COM
```

The above configuration specifies two mappings. The first mapping specifies that any system in the "example.com" DNS domain belongs to the EXAMPLE.COM realm. The second specifies that a system with the exact name "example.com" is also in the realm. (The distinction between a domain and a specific host is marked by the presence or lack of an initial ".".) The mapping can also be stored directly in DNS.

###Setting Up Secondary KDCs

For a number of reasons, you may choose to run multiple KDCs for a given realm. In this scenario, one KDC (the master KDC) keeps a writable copy of the realm database and runs kadmind (it is also your realm's admin server), and one or more KDCs (slave KDCs) keep read-only copies of the database and run kpropd.

The master-slave propagation procedure entails the master KDC dumping its database to a temporary dump file and then transmitting that file to each of its slaves, which then overwrite their previously-received read-only copies of the database with the contents of the dump file.

To set up a slave KDC, first ensure that the master KDC's krb5.conf and kdc.conf files are copied to the slave KDC.

Start kadmin.local from a root shell on the master KDC and use its add_principal command to create a new entry for the master KDC's host service, and then use its ktadd command to simultaneously set a random key for the service and store the random key in the master's default keytab file. This key will be used by the kprop command to authenticate to the slave servers. You will only need to do this once, regardless of how many slave servers you install.

```
# kadmin.local -r EXAMPLE.COM
Authenticating as principal root/admin@EXAMPLE.COM with password.
kadmin: add_principal -randkey host/masterkdc.example.com
Principal "host/host/masterkdc.example.com@EXAMPLE.COM" created.
kadmin: ktadd host/masterkdc.example.com
Entry for principal host/masterkdc.example.com with kvno 3, encryption type Triple DES cbc mode with \
HMAC/sha1 added to keytab WRFILE:/etc/krb5.keytab.
Entry for principal host/masterkdc.example.com with kvno 3, encryption type ArcFour with HMAC/md5 \
added to keytab WRFILE:/etc/krb5.keytab.
Entry for principal host/masterkdc.example.com with kvno 3, encryption type DES with HMAC/sha1 added \
to keytab WRFILE:/etc/krb5.keytab.
Entry for principal host/masterkdc.example.com with kvno 3, encryption type DES cbc mode with RSA-MD5 \
added to keytab WRFILE:/etc/krb5.keytab.
kadmin: quit
```

Start kadmin from a root shell on the slave KDC and use its add_principal command to create a new entry for the slave KDC's host service, and then use kadmin's ktadd command to simultaneously set a random key for the service and store the random key in the slave's default keytab file. This key is used by the kpropd service when authenticating clients.

```
# kadmin -p jimbo/admin@EXAMPLE.COM -r EXAMPLE.COM
Authenticating as principal jimbo/admin@EXAMPLE.COM with password.
Password for jimbo/admin@EXAMPLE.COM: 
kadmin: add_principal -randkey host/slavekdc.example.com
Principal "host/slavekdc.example.com@EXAMPLE.COM" created.
kadmin: ktadd host/slavekdc.example.com@EXAMPLE.COM
Entry for principal host/slavekdc.example.com with kvno 3, encryption type Triple DES cbc mode with \
HMAC/sha1 added to keytab WRFILE:/etc/krb5.keytab.
Entry for principal host/slavekdc.example.com with kvno 3, encryption type ArcFour with HMAC/md5 added \
to keytab WRFILE:/etc/krb5.keytab.
Entry for principal host/slavekdc.example.com with kvno 3, encryption type DES with HMAC/sha1 added \
to keytab WRFILE:/etc/krb5.keytab.
Entry for principal host/slavekdc.example.com with kvno 3, encryption type DES cbc mode with RSA-MD5 added \
to keytab WRFILE:/etc/krb5.keytab.
kadmin: quit
```

With its service key, the slave KDC could authenticate any client which would connect to it. Obviously, not all of them should be allowed to provide the slave's kprop service with a new realm database. To restrict access, the kprop service on the slave KDC will only accept updates from clients whose principal names are listed in /var/kerberos/krb5kdc/kpropd.acl. Add the master KDC's host service's name to that file.

`# echo host/masterkdc.example.com@EXAMPLE.COM > /var/kerberos/krb5kdc/kpropd.acl`

Once the slave KDC has obtained a copy of the database, it will also need the master key which was used to encrypt it. If your KDC database's master key is stored in a stash file on the master KDC (typically named /var/kerberos/krb5kdc/.k5.REALM, either copy it to the slave KDC using any available secure method, or create a dummy database and identical stash file on the slave KDC by running kdb5_util create -s (the dummy database will be overwritten by the first successful database propagation) and supplying the same password.

Ensure that the slave KDC's firewall allows the master KDC to contact it using TCP on port 754 (krb5_prop), and start the kprop service. Then, double-check that the kadmin service is disabled.

Now perform a manual database propagation test by dumping the realm database, on the master KDC, to the default data file which the kprop command will read (/var/kerberos/krb5kdc/slave_datatrans), and then use the kprop command to transmit its contents to the slave KDC.

```
# /usr/kerberos/sbin/kdb5_util dump /var/kerberos/krb5kdc/slave_datatrans
# kprop slavekdc.example.com
```

Using kinit, verify that a client system whose krb5.conf lists only the slave KDC in its list of KDCs for your realm is now correctly able to obtain initial credentials from the slave KDC.

That done, simply create a script which dumps the realm database and runs the kprop command to transmit the database to each slave KDC in turn, and configure the cron service to run the script periodically.

###Setting Up Cross Realm Authentication

Cross-realm authentication is the term which is used to describe situations in which clients (typically users) of one realm use Kerberos to authenticate to services (typically server processes running on a particular server system) which belong to a realm other than their own.

For the simplest case, in order for a client of a realm named A.EXAMPLE.COM to access a service in the B.EXAMPLE.COM realm, both realms must share a key for a principal named krbtgt/B.EXAMPLE.COM@A.EXAMPLE.COM, and both keys must have the same key version number associated with them.

To accomplish this, select a very strong password or passphrase, and create an entry for the principal in both realms using kadmin.

```
# kadmin -r A.EXAMPLE.COM
kadmin: add_principal krbtgt/B.EXAMPLE.COM@A.EXAMPLE.COM
Enter password for principal "krbtgt/B.EXAMPLE.COM@A.EXAMPLE.COM":
Re-enter password for principal "krbtgt/B.EXAMPLE.COM@A.EXAMPLE.COM":
Principal "krbtgt/B.EXAMPLE.COM@A.EXAMPLE.COM" created.
quit
# kadmin -r B.EXAMPLE.COM
kadmin: add_principal krbtgt/B.EXAMPLE.COM@A.EXAMPLE.COM
Enter password for principal "krbtgt/B.EXAMPLE.COM@A.EXAMPLE.COM":
Re-enter password for principal "krbtgt/B.EXAMPLE.COM@A.EXAMPLE.COM":
Principal "krbtgt/B.EXAMPLE.COM@A.EXAMPLE.COM" created.
quit
```

Use the get_principal command to verify that both entries have matching key version numbers (kvno values) and encryption types.

---

####Dumping the Database Doesn't Do It
Security-conscious administrators may attempt to use the add_principal command's -randkey option to assign a random key instead of a password, dump the new entry from the database of the first realm, and import it into the second. This will not work unless the master keys for the realm databases are identical, as the keys contained in a database dump are themselves encrypted using the master key.

---

Clients in the A.EXAMPLE.COM realm are now able to authenticate to services in the B.EXAMPLE.COM realm. Put another way, the B.EXAMPLE.COM realm now trusts the A.EXAMPLE.COM realm, or phrased even more simply, B.EXAMPLE.COM now trusts A.EXAMPLE.COM.

This brings us to an important point: cross-realm trust is unidirectional by default. The KDC for the B.EXAMPLE.COM realm may trust clients from the A.EXAMPLE.COM to authenticate to services in the B.EXAMPLE.COM realm, but the fact that it does has no effect on whether or not clients in the B.EXAMPLE.COM realm are trusted to authenticate to services in the A.EXAMPLE.COM realm. To establish trust in the other direction, both realms would need to share keys for the krbtgt/A.EXAMPLE.COM@B.EXAMPLE.COM service (take note of the reversed in order of the two realms compared to the example above).

If direct trust relationships were the only method for providing trust between realms, networks which contain multiple realms would be very difficult to set up. Luckily, cross-realm trust is transitive. If clients from A.EXAMPLE.COM can authenticate to services in B.EXAMPLE.COM, and clients from B.EXAMPLE.COM can authenticate to services in C.EXAMPLE.COM, then clients in A.EXAMPLE.COM can also authenticate to services in C.EXAMPLE.COM, even if C.EXAMPLE.COM doesn't directly trust A.EXAMPLE.COM. This means that, on a network with multiple realms which all need to trust each other, making good choices about which trust relationships to set up can greatly reduce the amount of effort required.

Now you face the more conventional problems: the client's system must be configured so that it can properly deduce the realm to which a particular service belongs, and it must be able to determine how to obtain credentials for services in that realm.

First things first: the principal name for a service provided from a specific server system in a given realm typically looks like this:

service/server.example.com@EXAMPLE.COM

In this example, service is typically either the name of the protocol in use (other common values include ldap, imap, cvs, and HTTP) or host, server.example.com is the fully-qualified domain name of the system which runs the service, and EXAMPLE.COM is the name of the realm.

To deduce the realm to which the service belongs, clients will most often consult DNS or the domain_realm section of /etc/krb5.conf to map either a hostname (server.example.com) or a DNS domain name (.example.com) to the name of a realm (EXAMPLE.COM).

Having determined which to which realm a service belongs, a client then has to determine the set of realms which it needs to contact, and in which order it must contact them, to obtain credentials for use in authenticating to the service.

This can be done in one of two ways.

The default method, which requires no explicit configuration, is to give the realms names within a shared hierarchy. For an example, assume realms named A.EXAMPLE.COM, B.EXAMPLE.COM, and EXAMPLE.COM. When a client in the A.EXAMPLE.COM realm attempts to authenticate to a service in B.EXAMPLE.COM, it will, by default, first attempt to get credentials for the EXAMPLE.COM realm, and then to use those credentials to obtain credentials for use in the B.EXAMPLE.COM realm.

The client in this scenario treats the realm name as one might treat a DNS name. It repeatedly strips off the components of its own realm's name to generate the names of realms which are "above" it in the hierarchy until it reaches a point which is also "above" the service's realm. At that point it begins prepending components of the service's realm name until it reaches the service's realm. Each realm which is involved in the process is another "hop".

For example, using credentials in A.EXAMPLE.COM, authenticating to a service in B.EXAMPLE.COM:

* A.EXAMPLE.COM → EXAMPLE.COM → B.EXAMPLE.COM
A.EXAMPLE.COM and EXAMPLE.COM share a key for krbtgt/EXAMPLE.COM@A.EXAMPLE.COM

* EXAMPLE.COM and B.EXAMPLE.COM share a key for krbtgt/B.EXAMPLE.COM@EXAMPLE.COM

Another example, using credentials in SITE1.SALES.EXAMPLE.COM, authenticating to a service in EVERYWHERE.EXAMPLE.COM:

SITE1.SALES.EXAMPLE.COM → SALES.EXAMPLE.COM → EXAMPLE.COM → EVERYWHERE.EXAMPLE.COM

* SITE1.SALES.EXAMPLE.COM and SALES.EXAMPLE.COM share a key for krbtgt/SALES.EXAMPLE.COM@SITE1.SALES.EXAMPLE.COM

* SALES.EXAMPLE.COM and EXAMPLE.COM share a key for krbtgt/EXAMPLE.COM@SALES.EXAMPLE.COM

* EXAMPLE.COM and EVERYWHERE.EXAMPLE.COM share a key for krbtgt/EVERYWHERE.EXAMPLE.COM@EXAMPLE.COM

Another example, this time using realm names whose names share no common suffix (DEVEL.EXAMPLE.COM and PROD.EXAMPLE.ORG):

DEVEL.EXAMPLE.COM → EXAMPLE.COM → COM → ORG → EXAMPLE.ORG → PROD.EXAMPLE.ORG

* DEVEL.EXAMPLE.COM and EXAMPLE.COM share a key for krbtgt/EXAMPLE.COM@DEVEL.EXAMPLE.COM

* EXAMPLE.COM and COM share a key for krbtgt/COM@EXAMPLE.COM

* COM and ORG share a key for krbtgt/ORG@COM

* ORG and EXAMPLE.ORG share a key for krbtgt/EXAMPLE.ORG@ORG

* EXAMPLE.ORG and PROD.EXAMPLE.ORG share a key for krbtgt/PROD.EXAMPLE.ORG@EXAMPLE.ORG

The more complicated, but also more flexible, method involves configuring the capaths section of /etc/krb5.conf, so that clients which have credentials for one realm will be able to look up which realm is next in the chain which will eventually lead to the being able to authenticate to servers.

The format of the capaths section is relatively straightforward: each entry in the section is named after a realm in which a client might exist. Inside of that subsection, the set of intermediate realms from which the client must obtain credentials is listed as values of the key which corresponds to the realm in which a service might reside. If there are no intermediate realms, the value "." is used.

Here's an example:

```
[capaths]
A.EXAMPLE.COM = {
B.EXAMPLE.COM = .
C.EXAMPLE.COM = B.EXAMPLE.COM
D.EXAMPLE.COM = B.EXAMPLE.COM
D.EXAMPLE.COM = C.EXAMPLE.COM
}
```

In this example, clients in the A.EXAMPLE.COM realm can obtain cross-realm credentials for B.EXAMPLE.COM directly from the A.EXAMPLE.COM KDC.

If those clients wish to contact a service in theC.EXAMPLE.COM realm, they will first need to obtain necessary credentials from the B.EXAMPLE.COM realm (this requires that krbtgt/B.EXAMPLE.COM@A.EXAMPLE.COM exist), and then use those credentials to obtain credentials for use in the C.EXAMPLE.COM realm (using krbtgt/C.EXAMPLE.COM@B.EXAMPLE.COM).

If those clients wish to contact a service in the D.EXAMPLE.COM realm, they will first need to obtain necessary credentials from the B.EXAMPLE.COM realm, and then credentials from the C.EXAMPLE.COM realm, before finally obtaining credentials for use with the D.EXAMPLE.COM realm.

---
####Note
Without a capath entry indicating otherwise, Kerberos assumes that cross-realm trust relationships form a hierarchy.
Clients in the A.EXAMPLE.COM realm can obtain cross-realm credentials from B.EXAMPLE.COM realm directly. Without the "." indicating this, the client would instead attempt to use a hierarchical path, in this case:

A.EXAMPLE.COM → EXAMPLE.COM → B.EXAMPLE.COM

---

###Additional Resources

For more information about Kerberos, refer to the following resources.

###Installed Documentation

The Kerberos V5 Installation Guide and the Kerberos V5 System Administrator's Guide in PostScript and HTML formats. These can be found in the /usr/share/doc/krb5-server-<version-number>/ directory (where <version-number> is the version number of the krb5-server package installed on your system).

The Kerberos V5 UNIX User's Guide in PostScript and HTML formats. These can be found in the /usr/share/doc/krb5-workstation-<version-number>/ directory (where <version-number> is the version number of the krb5-workstation package installed on your system).

Kerberos man pages — There are a number of man pages for the various applications and configuration files involved with a Kerberos implementation. The following is a list of some of the more important man pages.

####Client Applications
man kerberos — An introduction to the Kerberos system which describes how credentials work and provides recommendations for obtaining and destroying Kerberos tickets. The bottom of the man page references a number of related man pages.

  * man kinit — Describes how to use this command to obtain and cache a ticket-granting ticket.

  * man kdestroy — Describes how to use this command to destroy Kerberos credentials.

  * man klist — Describes how to use this command to list cached Kerberos credentials.

####Administrative Applications
  * man kadmin — Describes how to use this command to administer the Kerberos V5 database.

  * man kdb5_util — Describes how to use this command to create and perform low-level administrative functions on the Kerberos V5 database.

####Server Applications
  * man krb5kdc — Describes available command line options for the Kerberos V5 KDC.

  * man kadmind — Describes available command line options for the Kerberos V5 administration server.

####Configuration Files
  * man krb5.conf — Describes the format and options available within the configuration file for the Kerberos V5 library.

  * man kdc.conf — Describes the format and options available within the configuration file for the Kerberos V5 AS and KDC.