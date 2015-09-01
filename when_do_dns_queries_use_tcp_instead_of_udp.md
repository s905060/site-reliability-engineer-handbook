# When do DNS queries use TCP instead of UDP?

DNS goes over TCP when the size of the request or the response is greater than a single packet such as with responses that have many records or many IPv6 responses or most DNSSEC responses.

The maximum size was originally 512 bytes but there is an extension to the DNS protocol that allows clients to indicate that they can handle UDP responses of up to 4096 bytes.

DNSSEC responses are usually larger than the maximum UDP size.

Transfer requests are usually larger than the maximum UDP size and hence will also be done over TCP.

