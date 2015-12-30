# Visual Index

This page can help you find syntax elements when you can’t remember their names.
```
    file {'ntp.conf':
      path    => '/etc/ntp.conf',
      ensure  => file,
      content => template('ntp/ntp.conf'),
      owner   => 'root',
      mode    => '0644',
    }
```

A resource declaration.

* file: The resource type
* ntp.conf: The title
* path: An attribute
'/etc/ntp.conf': A value; in this case, a string
* template('ntp/ntp.conf'): A function call that returns a value; in this case, the template function, with the name of a template in a module as its argument

```
 package {'ntp':
      ensure => installed,
      before => File['ntp.conf'],
    }
    service {'ntpd':
      ensure    => running,
      subscribe => File['ntp.conf'],
    }
```

Two resources using the before and subscribe relationship metaparameters (which accept resource references).
```
    Package['ntp'] -> File['ntp.conf'] ~> Service['ntpd']
```

 Chaining arrows forming relationships between three resources, using resource references.
```
    $package_list = ['ntp', 'apache2', 'vim-nox', 'wget']
```

A variable being assigned an array value.
```
    $myhash = { key => { subkey => 'b' }}
```

A variable being assigned a hash value.
```
    ...
    content => "Managed by puppet master version ${serverversion}"
```

A master-provided built-in variable being interpolated into a double-quoted string (with optional curly braces).
```
    class ntp {
      package {'ntp':
        ...
      }
      ...
    }
```

A class definition, which makes a class avaliable for later use.
```
    include ntp
    require ntp
    class {'ntp':}
```

 Declaring a class in three different ways: with the include function, with the require function, and with the resource-like syntax. Declaring a class causes the resources in it to be managed.
```
    define apache::vhost ($port, $docroot, $servername = $title, $vhost_name = '*') {
      include apache
      include apache::params
      $vhost_dir = $apache::params::vhost_dir
      file { "${vhost_dir}/${servername}.conf":
          content => template('apache/vhost-default.conf.erb'),
          owner   => 'www',
          group   => 'www',
          mode    => '644',
          require => Package['httpd'],
          notify  => Service['httpd'],
      }
    }
```

A defined type, which makes a new resource type available. Note that the name of the type has two namespace segments.
```
    apache::vhost {'homepages':
      port    => 8081,
      docroot => '/var/www-testhost',
    }
```

Declaring a defined resource (or “instance”) of the type defined above.
```
    Apache::Vhost['homepages']
```

A resource reference to the defined resource declared above. Note that every namespace segment must be capitalized.
```
    node 'www1.example.com' {
      include common
      include apache
      include squid
    }
```

A node definition.
```
    node /^www\d+$/ {
      include common
    }
```

A regular expression node definition.
```
    import nodes/*.pp
```

An import statement. Should be avoided in all but a few circumstances.
```
    # comment
    /* comment */
```

Two comments.
```
    if str2bool("$is_virtual") {
      warning( 'Tried to include class ntp on virtual machine; this node may be misclassified.' )
    }
    elsif $operatingsystem == 'Darwin' {
      warning( 'This NTP module does not yet work on our Mac laptops.' )
    else {
      include ntp
    }
```