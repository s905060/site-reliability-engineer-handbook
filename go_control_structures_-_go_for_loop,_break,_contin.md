# Go Control structures - Go for loop, break, continue, range

The for statement is the only available looping statement in Go. The generic statement definition is:

```
for "initialization statements"; "bool expression that has to evaluate to true"; "statements run prior to every loop except the first" {
//code to be executed if the boolean expression evaluates to true
}
```

Any of the parts of the for statement can be left out - but the semicolons have to be there (unless all three parts are empty, in which case the semi colons can also be left out). The best way to understand this, again, would be with a few examples. 

Full program
```
package main

import "fmt"

func main() {
    // initialize i to 0; check each time that i is less than 5; increment i for each loop after the first
    for i := 0; i < 5; i++ {
        fmt.Println("Value of i is now:", i)
    }
}
```
```
Value of i is now: 0
Value of i is now: 1
Value of i is now: 2
Value of i is now: 3
Value of i is now: 4
```
