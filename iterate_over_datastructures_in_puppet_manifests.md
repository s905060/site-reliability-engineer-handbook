# Iterate over datastructures in Puppet manifests

###Iterate over Arrays

The easiest way to iterate over arrays is to use the array directly on the Class or Defined Type:
```puppet
define arrayDebug {
  notify { "Item ${name}": }
}
 
class array {
  $array = [ 'item1', 'item2', 'item3' ]
  arrayDebug { $array: }
}
```

This calls arrayDebug 3 times with the array values as parameter.

Output of the example:

```bash
Item item1
Item item2
Item item3
```

###Iterate over Arrays with split

If you need more values per iteration than just one, we could use the split function to create a pseudo Hash:
```puppet
define arrayDebug {
  $value = split($name,',')
  notify { "Item ${value[0]} has value ${value[1]}": }
}
 
class array {
  $array = [ 'item1,value1', 'item2,value2', 'item3,value3' ]
  arrayDebug { $array: }
}
```
Output of the example:
```bash
Item item1 has value value1
Item item2 has value value2
Item item3 has value value3
```

###Iterate over Hashes

The above method with the pseudo Hash functionality does it’s job, but not very nice. It would be much nicer to directly iterate over a Hash. To achieve this, there is this nice function called create_resources:
```puppet
define hashDebug($value, $othervalue, $somevalue) {
  notify { "Item ${name} has value ${value}, othervalue ${othervalue} and somevalue ${somevalue}": }
}
 
class array {
  $hash = {
    item1 => { value      => '1',
               othervalue => 'a',
               somevalue  => 'jjj' },
    item2 => { value      => '2',
               othervalue => 'b' },
  }
 
  $hashDefaults = {
    $somevalue => 'zzz'
  }
 
  create_resources(hashDebug, $hash, $hashDefaults)
}
```
This example calls hashDebug two times, passing the Hash item as arguments. The variable $hashDefaults defines default values for missing parameters in the Hash.

Output of the example:
```bash
Item item1 has value 1, othervalue a and somevalue jjj
Item item1 has value 2, othervalue b and somevalue zzz
```