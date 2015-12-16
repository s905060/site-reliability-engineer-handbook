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

To illustrate that you can skip parts of the statements, I’ll show you a few code snippets. All of the below are valid statements.

Partial code: for loop snippets
```
//prints increasing value of i infinitely, since the middle check statement does not exist
for i := 0; ; i++ {
    fmt.Println("Value of i is now:", i)
}

//prints "value of i is: 0" infinitely since the incrementing part is not there and i can never reach 3
for i := 0; i < 3;  {
    fmt.Println("Value of i:", i)
}

//here both initialization and incrementing is missing in the for statement, but it is managed outside of it
s := ""
for ; s != "aaaaa";  {
    fmt.Println("Value of s:", s)
    s = s + "a"
}
```

Within the Go for loop you can use the multiple assignment to initialize many variables. Similarly to increment or update multiple variables, you will have to use the multiple assignment paradigm (i, j = i+2, j+2). For those used to certain other languages where you can separate statements within the for with commas, please note that this does not exist in Go.

Full program
```
package main

import "fmt"

func main() {
    //multiple initialization; a consolidated bool expression with && and ||; multiple ‘incrementation’
    for i, j, s := 0, 5, "a"; i < 3 && j < 100 && s != "aaaaa"; i, j, s = i+1, j+1, s + "a"  {
        fmt.Println("Value of i, j, s:", i, j, s)
    }
}
```
```
Value of i, j, s: 0 5 a
Value of i, j, s: 1 6 aa
Value of i, j, s: 2 7 aaa
```

###break keyword
The break keyword allows you to terminate a loop at that point and continue execution at the statement following the end of the for loop block. In the simplistic example below, we are making use of the break keyword to end a loop when a particular condition is met. Since the for loop has no conditions and checks as part of its declaration, it will run for ever unless we terminate the loop explicitly. Note that this is not terminating the program entirely; it is merely jumping out of the loop, and the program continues at the statement following the ending }. 
