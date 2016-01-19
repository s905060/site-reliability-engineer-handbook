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