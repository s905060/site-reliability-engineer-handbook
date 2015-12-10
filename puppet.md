# Puppet

Puppet CLI

* Bootstrap client
```
puppet agent -t --server <puppet master> [<options>]
```

* Display facts:
```
facter	# :
facter -y	# YAML
facter -j	# JSON
facter memoryfree
facter is_virtual processor0
```

* Find out effective classes on a node
```
cat /var/lib/puppet/classes.txt
```

* Find out when which file was modified
```
cd /var/lib/puppet
for i in $(find clientbucket/ -name paths); do
	echo "$(stat -c %y $i | sed 's/\..*//')       $(cat $i)";
done | sort -n
```

* Puppet Dry Run:
```
puppet agent --noop --verbose
```

* Disable agent
```
puppet agent --disable
puppet agent --disable <info message>   # Only recent versions
puppet agent --enable
```

* Executing selective classes
```
puppet agent --tags Some::Class
Managing Certificates (on master)
puppet cert list
puppet cert list --all
puppet cert sign <name>
puppet cert clean <name>   # removes cert
```

* Managing Nodes
```
puppet node clean <name>   # removes node + cert
```

* Managing Modules
```
puppet module list
puppet module install <name>
puppet module uninstall <name>
puppet module upgrade <name>
puppet module search <name>
```