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

