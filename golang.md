###Why Go?
```
 “Go is like a better C, from the guys that didn’t bring you C++” — Ikai Lan
```

###Features of Go:

* Syntax and environment adopting patterns.
* Fast compilation time.
* Remote package management.
* Online package documentation.
* Built-in concurrency primitives.
* Interfaces to replace virtual inheritance.
* Type embedding to replace non-virtual inheritance.
* Compiled into statically linked native binaries without external dependencies.
* Simple language specifications.
* Large built-in library.
* Light testing framework.

###The Go Language

TYPES
###Integers

Integers are numbers without a decimal component. Unlike us, the computers use a base-2 representation called binary system. Go’s integer types are : **uint8, int8, uint16, int16, uint32, int32, uint64, int64.**

8, 16, 32 and 64 tell us how many bits each of the types use.

Also, a **byte** is an alias for **uint8** and a rune for **int32**.

There are also 3 machine dependent integer types: **uint, int and uintptr**. They are machine dependent because their size depends on the type of architecture you are using.

```
var x int = 404
var y uint32 = 234242
```

###Floating Point Numbers

Floating Point Numbers can be defined as numbers with a decimal component or more generally known as Real Numbers.

Go has two floating point types : **float32 and float64**

Go also has two complex number types : **complex64 and complex128**

```
var x float64 = 34.23
var y complex64 = 3 + 2i
```

###Strings

A string is a sequence of characters of a definite length for textual representation.

String literals in Go can be created using either double quotes “Hello World” or back quotes `Hello World`.

Double quotes cannot contain newlines but allow special escape sequences like \n and \t.

Strings can be concatenated using the + symbol.

The length of a string can be obtained using the len(<string>) function.

Using indices, individual character of a string can be obtained.

```
var weird string = "weird string"
a := `Hello to all`
```

###Boolean

Booleans are a special 1 bit integer type used to indicate True or False.

The logical operators which can be used on booleans are :

```
and &&
or ||
not !
```

###Variables

Variables in Go are initialized using the var keyword and then followed by the variable name and variable type.
```
var x int
var y string = "Hello world"
```

You can also initialize variables in a shorter way.

Here the data type is internally assigned.
```
x := 8
y := "Hello world"
```

You can also declare constants using the assignment operator, Numeric constants in Go are high-precision values.
```
const y = "Hello world"
const Pi = 3.14
```

###Loops and Control Structures

**For**

For loops are used just as in C and Java.

```
for i := 0; i < 10; i++ {
	fmt.Println("The value of i : ", i)
}   
```