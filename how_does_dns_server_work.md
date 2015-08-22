# How does DNS server work

* **Step1**: the client types www.example.com in his browser

* **Step2**: the operating system looks at /etc/host file,first for the ip address of www.example.com(this can be changed from /etc/nsswitch), then looks /etc/resolv.conf for the DNS server IP for that machine

* **Step3**: the dns server will search its database for the name www.example.com, if it finds it will give that back, if not it will query the root server(.) for the information.

* **Step4**: root server will return a referral to the .com TLD name server(these TLD name servers knows the address of name servers of all SLD's).In our case we searched for www.example.com so root server will give us referral to .com TLD servers.

If it was www.example.net then root server will give, .net TLD servers refferal.

* **Step5**: Now One of the TLD servers of .com will give us the referral to the DNS server resposible for example.com domain.

* **Step6**: the dns server for example.com domain will now give the client the ip address of www host(www is the host name.)

Now lets practically have a look at how this process works.

```
[root@myvm1 ~]# dig +trace www.google.com

; <<>> DiG 9.3.4-P1 <<>> +trace www.google.com
;; global options:  printcmd
.                       5       IN      NS      a.root-servers.net.
.                       5       IN      NS      b.root-servers.net.
.                       5       IN      NS      c.root-servers.net.
.                       5       IN      NS      d.root-servers.net.
.                       5       IN      NS      e.root-servers.net.
.                       5       IN      NS      f.root-servers.net.
.                       5       IN      NS      g.root-servers.net.
.                       5       IN      NS      h.root-servers.net.
.                       5       IN      NS      i.root-servers.net.
.                       5       IN      NS      j.root-servers.net.
.                       5       IN      NS      k.root-servers.net.
.                       5       IN      NS      l.root-servers.net.
.                       5       IN      NS      m.root-servers.net.
;; Received 228 bytes from 192.168.159.2#53(192.168.159.2) in 49 ms

com.                    172800  IN      NS      a.gtld-servers.net.
com.                    172800  IN      NS      b.gtld-servers.net.
com.                    172800  IN      NS      c.gtld-servers.net.
com.                    172800  IN      NS      d.gtld-servers.net.
com.                    172800  IN      NS      e.gtld-servers.net.
com.                    172800  IN      NS      f.gtld-servers.net.
com.                    172800  IN      NS      g.gtld-servers.net.
com.                    172800  IN      NS      h.gtld-servers.net.
com.                    172800  IN      NS      i.gtld-servers.net.
com.                    172800  IN      NS      j.gtld-servers.net.
com.                    172800  IN      NS      k.gtld-servers.net.
com.                    172800  IN      NS      l.gtld-servers.net.
com.                    172800  IN      NS      m.gtld-servers.net.
;; Received 504 bytes from 198.41.0.4#53(a.root-servers.net) in 153 ms

google.com.             172800  IN      NS      ns2.google.com.
google.com.             172800  IN      NS      ns1.google.com.
google.com.             172800  IN      NS      ns3.google.com.
google.com.             172800  IN      NS      ns4.google.com.
;; Received 168 bytes from 192.33.14.30#53(b.gtld-servers.net) in 12 ms

www.google.com.         300     IN      A       74.125.236.48
www.google.com.         300     IN      A       74.125.236.50
www.google.com.         300     IN      A       74.125.236.51
www.google.com.         300     IN      A       74.125.236.49
www.google.com.         300     IN      A       74.125.236.52
;; Received 112 bytes from 216.239.34.10#53(ns2.google.com) in 108 ms
```

Now you can clearly see from the dig with trace output that, the request first went to root servers. a.root-servers.net replied me with the addresses of all .com gtld servers, and b.gtld-servers.net gave me the name servers for google.com and finally ns2.google.com replied me with the ip address of www.google.com