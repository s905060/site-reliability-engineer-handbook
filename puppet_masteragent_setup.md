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

The Agent creates a Certificate Signing Request (CSR) and a private key to secure our request. The agent sends the request to the Master/Certificate Authority to sign and return the certificate. The agent will need to be rerun to check if the CA has signed the certificate.

To check the list of certificates waiting to be signed by the Master/CA:

`puppet ca list`

To sign the certicate:

```
# Per host
puppet cert sign [hostname]

# Sign all
puppet cert sign --all
```

Rather than signing each certificate manually you can set puppet to autosign certificates from a certain range.

https://docs.puppetlabs.com/puppet/latest/reference/subsystem_agent_master_comm.html

###Manifests dir

Manifests is where Puppet puts files containing configuration information.

Usually this dir contains a file called site.pp which tells Puppet where and which configurations to load for agents.

You can also override the name and location of the manifests directory and `site.pp` file using the `manifestdir` and `manifest` configuration options, respectively. these options are set in the `puppet.conf` configuration file in the [master] section. See http://docs.puppetlabs.com/references/stable/configuration.html

###Master

Puppet’s agent/master mode is pull-based. Usually, agents are configured to periodically fetch a catalog and apply it, and the master controls what goes into that catalog.

On redhat or Debian systems the Master is started via an init script with the service command:
```
# Output can be viewed in /var/log/syslog
sudo service puppetmaster start
```

For testing purposes you can do the following:
```
# --verbose      = gives detailed logging
# --no-daemonize = keeps the output in the foreground
# --debug        = this will produce detailed debug output

sudo puppet master --verbose --no-daemonize
```

###Agent

What is an Agent and how it works with the Puppet Master

On redhat or Debian systems the Agent is started via an init script with the service command:
```
# Output can be viewed in /var/log/syslog
sudo service puppet start
```

For testing purposes you can do the following:
```
# --verbose = gives detailed logging
# --test    = run the puppet agent, outputs to standard out and exitssudo

# If you don't specify the server, the agent will look for a host called `puppet` (/etc/hosts)
sudo puppet agent --test --server=puppet.server.com
```
