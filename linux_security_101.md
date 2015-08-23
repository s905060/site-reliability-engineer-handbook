# Linux Security 101

By Kelley Spoon, mars@loeffel.txdirect.net

You can jump down to the section on tcpd 
Or take a peek at the other stuff you need to keep an eye on.

Ok. You've got Linux beat. You finally got AfterStep set up the way you want it, you've managed to set up ip masquerading for your home LAN, you've managed to set up a cool issue for people to see when they log in, you managed to convert a couple over to the One True OS, and chicks really dig you because, as we all know, Linux geeks are sexy.

One night as you're peeking at /var/adm/messages, you notice that someone from some place you've never heard of before tried to make 5 ftp connections, 6 telnets, and even an nntp connection. What's up with that?

Well, Linux (and all Unix type OS's in general) were designed to be a programmer's paradise. The same qualities that makes Linux such a wonderful networking and hacking operating system also expose a few security holes. There are a few programs that you probably rely on or use daily that can be used to gain root access (which is a Bad Thing). What's worse, the commercial distributions that many Linux users depend on have these programs with security holes inside packages that are installed as part of the base system.

That's the bad news. The good news is that we can make it tougher for the Bad Guys to do their dirty deeds. By checking the Linux ALERTS page, you can find out what the holes we know about are, and how to temporarily plug them up or even fix them up for good. There is also a nice little tool that is probably on your system that we can use to keep them from even having access to our machine.

And that's what I'm going to focus on. My belief here is that if we can keep the Remote Bad Guys (people who don't have legitimate access to our machines) out, then we only have to worry about the Local Bad Guys (if any). Plus it gives us a chance to fix anything on our machine that is a security hole the RBG's can use.

# tcpd

There's a daemon that's probably been installed on your machine that you don't know about. Or at least, you're not aware of what it can do. It's called tcpd, and it's how we shut off access to some of the basic services that the Bad Guys can use to get on our system.

Since tcpd can be pretty complex, I'm not going to go into all the details and tell you how to do the fancy stuff. The goal here is to keep the mischievous gibbons from knocking down what it took so long for use to set up.

tcpd is called into action from another daemon, inetd, whenever someone tries to access a service like in.telnetd, wu.ftpd, in.fingerd, in.rshd, etc. tcpd's job is to look at two files and determine if the person who is trying to access the service has permission or not.

The files are /etc/hosts.allow and /etc/hosts.deny. Here's how it all works:

1. Someone tries to use a service that tcpd is monitoring.
2. tcpd wakes up, and makes a note of the attempt to the 2. syslog.
2. tcpd then looks hosts.allow
if it finds a match, tcpd goes back to sleep and lets the user access the service.
3. tcpd now takes a look at hosts.deny
if it finds a match, tcpd closes the user's connection
4. If it can't find a match in either file, or if both files are empty, tcpd shrugs, guesses it's OK to let the user on, and goes back to sleep.

Now, there are a couple of things to note here. First, if you haven't edited hosts.allow or hosts.deny since you installed Linux, then tcpd assumes that you want to let everyone have access to your machine. The second thing to note is that if tcpd finds a match in hosts.allow, it stops looking. In other words, we can put an entry in hosts.deny and deny access to all services from all machines, and then list ``friendly'' machines in the hosts.allow file.

Let's take a look at the man page. You'll find the info you need by typing man 5 hosts_access (don't forget the 5 and the underscore).
```
       daemon_list : client_list

       daemon_list is a list of one or more daemon process  names
         or wildcards

       client_list  is  a  list  of  one or more host names, host
         addresses, patterns or wildcards  that will  be matched
         against the remote host name or address. 
       
       List elements should be separated by blanks and/or commas.
```

Now, if you go take a look at the man page, you'll notice that I didn't show you everything that was in there. The reason for that is because the extra option (the shell_command) can be used to do some neat stuff, but *most Linux distributions have not enabled the use of this option in their tcpd binaries*. We'll save how to do this for an article on tcpd itself.

If you absolutely have to have this option, get the source from here and recompile.

Back to business. What the above section from the hosts_access man page was trying to say is that the format of hosts.[allow|deny] is made up of a list of services and a list of host name patterns, separated by a ``:''

You'll find the name of the services you can use by looking in your /etc/inetd.conf...they'll be the ones with /usr/sbin/tcpd set as the server path.

The rules for determining host patterns are pretty simple, too:

* if you want to match all hosts in a domain, put a ``.'' at the front.
Ex: .bar.com will match "foo.bar.com", "sailors.bar.com", "blue.oyster.bar.com", etc.

* if you want to match all IPs in a domain, put a "." at the end.
Ex: 192.168.1. will match "192.168.1.1", "192.168.1.2", "192.168.1.3", etc.

And finally, there are some wildcards you can use:

*  ALL matches everything. If in daemon_list, matches all daemons; if in client_list, it matches all host names.
Ex: ALL : ALL would match any machine trying to get to any service.

* LOCAL matches host names that don't have a dot in them.
Ex: ALL : LOCAL would match any machine that is inside the domain or search aliases given in your /etc/resolv.conf

* except isn't really a wildcard, but it comes in useful. It excludes a pattern from the list.
Ex: ALL : ALL except .leetin-haxor.org would match all services to anyone who is not from ``*.leetin-haxor.org''
Ok. Enough technical stuff. Let's get to some examples.

Let's pretend we have a home LAN, and a computer for each member of the family.

Our home network looks like this:
```
    linux.home.net      192.168.1.1
    dad.home.net	    192.168.1.2
    mom.home.net	    192.168.1.3
    sis.home.net	    192.168.1.4
    bro.home.net        192.168.1.5
```

Now, since no one in the family is likely to try and ``hack root,'' we can assume they're all friendly. But....we're not so sure about the rest of the people on the Internet. Here's how we go about setting things up so people on home.net have full access to our machine, but no one else does.

In /etc/hosts.allow:
```
# /etc/hosts.allow for linux.home.net

ALL: .home.net
And in /etc/hosts.deny
```
```
# /etc/hosts.deny for linux.home.net

ALL : ALL
```
Since tcpd looks at hosts.allow first, we can safely deny access to all services for everybody. If tcpd can't match the machine sending the request to ``*.home.net'', the connection gets refused.

Now, let's pretend that Mom has been reading up on how Unix stuff works, and she's started doing some unfriendly stuff on our machine. In order to deny her access to our machine, we simply change the line in hosts.allow to:

ALL: .home.net except mom.home.net
Now, let's pretend a friend from....uh....friend.com wants to get something off our ftp server. No problem, just edit hosts.allow again:
```
# /etc/hosts.allow for linux.home.net

ALL: .home.net except mom.home.net
wu.ftpd: .friend.com
```
Things are looking good.
The only problem is that the name server for home.net is sometimes down, and the only way we can identify someone as being on home.net is through their IP address. Not a problem:
```
# /etc/hosts.allow for linux.home.net

ALL: .home.net except mom.home.net
ALL: 192.168.1. except 192.168.1.3
ALL: .friend.com
And so on....
```
I have found that's it's easier to deny everybody access, and list your friends in hosts.allow than it is to allow everybody access, and deny only the people who you know are RBG's. If you are running a private machine, this won't really be a problem, and you can rest easy.

However, if you're trying to run a public service (like an ftp archive of Tetris games for different OS's) and you can't afford to be this paranoid, then you need shouldn't put anything in hosts.allow, and just put all of the people you don't want touching your machine in hosts.deny

You might also want to take a look at the next section.

# Other things to keep in mind

###Security holes in software

Like I said earlier, a lot of the software that comes standard in CD-ROMs have security holes in them which could let local or even remote users execute commands as root on your system. Keep an eye on Linux ALERTS to find out about problems we know about and how to fix them.

###What services you offer

Check to make sure that the services you have running on your machine are what you really want to offer. For example, most of us don't have a need to run in.nntpd, yet it's got an entry in /etc/inetd.conf. Do you really want everyone on the Internet to have access to in.fingerd? Do you really need to let everyone on the Internet have access to your ftp server?

Find what you don't need (or don't want to offer to any passing stranger who might happen across your machine) and either shut it down or deny outside access to it.

###Passwords

Yeah, yeah, yeah. Everyone's heard the speech about passwords, but they are pretty important. Especially if you're not restricting access to your machine. Remember, if they can get to your machine, they can get on your machine. And if they can get on your machine, they can get root access.

In case you haven't heard the speech, here's the condensed version:

Make the passwords at least 6 characters long.
Mix the case of the passwords.
Use at least one numeral.
Use at least one non-alphanumeric character.
Change the passwords on a regular basis. About once every
two months should do for the casual user.
I have found that using "k-rad" or "leet-speak" helps when you need to make up a password. For example, instead of using the password "foobar", try using "f00b4R!".
Also, get and install shadow passwords. You might have to recompile a few services, but it's worth the extra protection.

Finally, it is important to note that only the first 8 characters of the password get used under Linux's login. In other words, if you have a password that looks like abcdefghijklmnopqrstuvwxyz, you will only need to enter abcdefhg in order to gain access to the account. This holds true whether you are using shadowed passwords or not.

[ Thanks to Olav Wölfelschneider for pointing that out. ]

### File Permissions

Many of the security holes that exist are because the files are "setuid". That means that a non-root user can execute the files as root. Remove this permission from any files that don't need it. Like mount. It really isn't that much of a hassle to keep one of your virtual consoles logged in as root, and flip over to it when you need to get something done.

Also, if you have stuff sitting somewhere that you don't want anyone else to see, don't give them world rwx permission on the dir.

### Keep an eye on the syslog

At least once a day, you need to go check the syslog and see what's been happening. You can find it /var/adm/syslog, and I'd also recommend taking a peek at /var/adm/messages. You'll want to look for multiple connections coming from places you don't know in a short period of time. If they look suspicious, then don't hesitate to slap an entry for the domain into /etc/hosts.deny

### Who you trust

This is just common sense. It's not a wise idea to give out your root password to someone you just met on IRC 5 minutes ago who claims they can get Apache up and running on your system if you just tell them the root password.

Set up a guest account with limited read, write, execute abilities and let them use that.

It's also not wise to let people just log in and fiddle around on your machine. Despite common belief, it is possible to create Unix ``viruses,'' and all you really need is the knowledge, the will, and an opportunity. For more information, see the paper on The Plausibility of Unix Virus Attacks

# Final Word

To be completely honest with you, the only way to be 100% sure your machine can't be compromised is to physically deny access to it. That means get rid of the modem and ethernet card, fill up any hole in the computer's case with cement, and buy a big, mean pit bull to guard it while you are asleep.

Well, maybe that's going a bit far. But the point is, if they can't get to your machine, they can't do anything to it. If you think your machine has been compromised, disconnect it from the network, look through the syslog, try to find out how it was compromised, fix the problem, set all new passwords for your accounts, and then reconnect it.

We might not be able to make the machine 100% secure, but we can make it hard for the Bad Guys to do their thing.

