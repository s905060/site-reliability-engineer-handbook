# Puppet Master/Agent Setup

###Puppet configuration

`/etc/puppet/puppet.conf` is your standard config file. There are 3 sections to take note of.

`[main]` - global configuration section

`[master]` - master specific configuration

`[agent]` - agent specific configuration


###SSL Certificate

When the Puppet Master is first started, besides initializing the environment, it also creates a local Certificate Authority along with the certificates and keys for the Master. It will also open the necessary ports so that agents can connect.

Puppet's ssl info and certificates are located at `/var/lib/puppet/ssl`

As briefly touched on before, Puppet uses SSL certificates to authenticate between the master and agent. When you first run:

`sudo puppet agent --test --server=puppet.example.com`