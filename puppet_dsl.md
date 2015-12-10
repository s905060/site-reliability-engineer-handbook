# Puppet DSL

* Snippets
```
 notify { 'message': loglevel => 'err' }
```

* Check for file
```
if file_exists('somefile.txt') == 1 { }
```

* Execute commands (evil!)
```
exec { "mkdir -p $dir":
    command => "/bin/mkdir -p $dir",
    creates => $dir
}
```

* Merging Arrays
```
 $result = split(inline_template("<%= (array1+array2).join(',') %>"),',')
```

* Exceptions
```
 fail('This is a parser time error')
```

* Conditions

```
 if $var == 'value' {
}

case $::lsbdistcodename {
	'squeeze': {
        }
        'wheezy', 'jessie': {
        }
        default {
        }
}
```