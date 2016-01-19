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

or
```
for i != 0 {
	fmt.Println("The value of i :", i)
}
```

The **range** keyword can be used to range over a list of values also
```
a := [ ]int{4, 3, 1, 7, 3, 9, 2}
for _, i := range a {
	sum = sum + i
}
```

###If

If statements are also similar to those in C and Java.
```
if num % 2 == 0 {
	fmt.Println("Even")
} else {
	fmt.Println("Odd")
}
```

###Switch

Switch statements starts with the keyword switch and follows with multiple cases.
```
switch i {
	case 0 : fmt.Println("Zero")
	case 1 : fmt.Println("One")
	case 2 : fmt.Println("Two")
	case 3 : fmt.Println("Three")
	case 4 : fmt.Println("Four")
	case 5 : fmt.Println("Five")
	case 6 : fmt.Println("Six")
	case 7 : fmt.Println("Seven")
	case 8 : fmt.Println("Eight")
	case 9 : fmt.Println("Nine")
	default : fmt.Println("Unknown Number")
}
```

###ARRAYS, SLICES AND MAPS
###Arrays

The type [n]T is an array of size n of type T.

```
var a [10]int 
x := [5]float64 {
	34,
	63,
	56,
	84,
	23,
}
```

###Slices

A slice is a segment of an array. The length of a slice is not fixed. Creation of slice is via the make command.
```
x := make([]int, 5)
```

###Maps

A map is an unordered collection of key-value pairs. Also known as an associative array, a hash table or a dictionary, maps are used to look up a value by its associated key.
```
var x map[string]int
x["Hello"] = 1
```

###Functions

Functions are defined using the keyword func followed by the function name and arguments and the return values.
```
func say_hello() {
	fmt.Println("Hello From the function")
}
```

If the result parameters are named, a return statement without arguments return the current values of the variables

```
func details(a []int) (x, y int) {
	x := len(a)
	y := cap(a)
}
```

###Multiple return types

Go supports multiple return types.
```
func calculate(a, b int) (sum, prod int) {
	sum := a + b
	prod := a * b
	return sum, prod
}

func main() {
	a, b := calculate(3, 4)
}
```

###Variadic Function

Functions can take 0 to undefined variables of a particular data type.
```
func total(args ...int) (sum int) {
	sum := 0
	for _, i := range args {
		sum = sum + i
	}
	return sum
}
```

###Closure

Consider the following example
```
func main() {
dog := func() {
		fmt.Println("Woof!")
}
	dog()
}
```

Here we are actually assigning the function to a variable.

Similarly.

```
func increm() func() int {
	x := 0
	return func() int {
		x = x + 1
		return x
	}
}

func main() {
	y := increm()
	fmt.Println(y)
	fmt.Println(y)
}

Output :
1
2
```

Here, the func increm returns a func of type func() int which still refers to the variable x which is outside the body of the func. This is known as a closure func.

###Pointers

Pointers, yes pointers! Go has pointers, but no pointer arithmetic.
```
x := 5
y := &x
fmt.Println(*y)

Output :
5
```

y points to x and the value of y is obtained using a * operator. The & operator is used to get the address of a variable.

The new operator is used to allocate memory to a pointer variable.

```
x := new(int)
*x = 9
fmt.Println(*x)

Output :
9
```

###STRUCTURES, METHODS AND INTERFACES
###Structures 
A structure is a user defined data type which contains named fields. A structure is basically an object in Go which has data types and methods on it.

```
type dog struct {
	breed string
	age int
	owner string
}

type cow struct {
	age int
	farm string
}

rocky := dog { "Lab", 5, "Roy" }
bigcow := cow { age : 3, farm : "Donalds" }
fmt.Println("Dogs age is : ", rocky.age )
fmt.Println("Cows age is : ", bigcow.age )
```

We declare structures using the type and struct keyword. Individual elements of the structure are accessed using the dot operator.

