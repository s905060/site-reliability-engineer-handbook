# Java

### Java Heap Memory vs Stack Memory Difference

#### #Java Heap Memory

Heap memory is used by java runtime to allocate memory to Objects and JRE classes. Whenever we create any object, it’s always created in the Heap space. Garbage Collection runs on the heap memory to free the memory used by objects that doesn’t have any reference. Any object created in the heap space has global access and can be referenced from anywhere of the application.

#### Java Stack Memory

Java Stack memory is used for execution of a thread. They contain method specific values that are short-lived and references to other objects in the heap that are getting referred from the method. Stack memory is always referenced in LIFO (Last-In-First-Out) order. Whenever a method is invoked, a new block is created in the stack memory for the method to hold local primitive values and reference to other objects in the method. As soon as method ends, the block becomes unused and become available for next method.
Stack memory size is very less compared to Heap memory.

Let’s understand the Heap and Stack memory usage with a simple program.

Memory.java
```
package com.journaldev.test;
 
public class Memory {
 
    public static void main(String[] args) { // Line 1
        int i=1; // Line 2
        Object obj = new Object(); // Line 3
        Memory mem = new Memory(); // Line 4
        mem.foo(obj); // Line 5
    } // Line 9
 
    private void foo(Object param) { // Line 6
        String str = param.toString(); //// Line 7
        System.out.println(str);
    } // Line 8
 
}
```
Below image shows the Stack and Heap memory with reference to above program and how they are being used to store primitive, Objects and reference variables.

![](Java-Heap-Stack-Memory-450x254.png)


Let’s go through the steps of execution of the program.

* As soon as we run the program, it loads all the Runtime classes into the Heap space. When main() method is found at line 1, Java Runtime creates stack memory to be used by main() method thread.
* We are creating primitive local variable at line 2, so it’s created and stored in the stack memory of main() method.
* Since we are creating an Object in line 3, it’s created in Heap memory and stack memory contains the reference for it. Similar process occurs when we create Memory object in line 4.
* Now when we call foo() method in line 5, a block in the top of the stack is created to be used by foo() method. Since Java is pass by value, a new reference to Object is created in the foo() stack block in line 6.
* A string is created in line 7, it goes in the String Pool in the heap space and a reference is created in the foo() stack space for it.
* foo() method is terminated in line 8, at this time memory block allocated for foo() in stack becomes free.
* In line 9, main() method terminates and the stack memory created for main() method is destroyed. Also the program ends at this line, hence Java Runtime frees all the memory and end the execution of the program.

#### Difference between Heap and Stack Memory

Based on the above explanations, we can easily conclude following differences between Heap and Stack memory.

1. Heap memory is used by all the parts of the application whereas stack memory is used only by one thread of execution.
2. Whenever an object is created, it’s always stored in the Heap space and stack memory contains the reference to it. Stack memory only contains local primitive variables and reference variables to objects in heap space.
3. Objects stored in the heap are globally accessible whereas stack memory can’t be accessed by other threads.
4. Memory management in stack is done in LIFO manner whereas it’s more complex in Heap memory because it’s used globally. Heap memory is divided into Young-Generation, Old-Generation etc, more details at Java Garbage Collection.
5. Stack memory is short-lived whereas heap memory lives from the start till the end of application execution.
6. We can use -Xms and -Xmx JVM option to define the startup size and maximum size of heap memory. We can use -Xss to define the stack memory size.
7. When stack memory is full, Java runtime throws java.lang.StackOverFlowError whereas if heap memory is full, it throws java.lang.OutOfMemoryError: Java Heap Space error.
8. Stack memory size is very less when compared to Heap memory. Because of simplicity in memory allocation (LIFO), stack memory is very fast when compared to heap memory.
That’s all for Stack vs Heap Memory in terms of java application, I hope it will clear your doubts regarding memory allocation when any java program is executed.

### How do threads interact with the stack and the heap? How do the stack and heap work in multithreading?

In a multi-threaded application, each thread will have its own stack. But, all the different threads will share the heap. Because the different threads share the heap in a multi-threaded application, this also means that there has to be some coordination between the threads so that they don’t try to access and manipulate the same piece(s) of memory in the heap at the same time.

### Can an object be stored on the stack instead of the heap?

Yes, an object can be stored on the stack. If you create an object inside a function without using the “new” operator then this will create and store the object on the stack, and not on the heap. Suppose we have a C++ class called Member, for which we want to create an object. We also have a function called somefunction( ). Here is what the code would look like:

### Code to create an object on the stack:

```
void somefunction( )
{
/* create an object "m" of class Member
    this will be put on the stack since the 
    "new" keyword is not used, and we are 
   creating the object inside a function
*/
  
  Member m;

} //the object "m" is destroyed once the function ends
```

So, the object “m” is destroyed once the function has run to completion – or, in other words, when it “goes out of scope”. The memory being used for the object “m” on the stack will be removed once the function is done running.

If we want to create an object on the heap inside a function, then this is what the code would look like:

### Code to create an object on the heap:

```
void somefunction( )
{
/* create an object "m" of class Member
    this will be put on the heap since the 
    "new" keyword is used, and we are 
   creating the object inside a function
*/
  
  Member* m = new Member( ) ;
  
  /* the object "m" must be deleted
      otherwise a memory leak occurs
  */

  delete m; 
} 
```

In the code above, you can see that the “m” object is created inside a function using the “new” keyword. This means that “m” will be created on the heap. But, since “m” is created using the “new” keyword, that also means that we must delete the “m” object on our own as well – otherwise we will end up with a memory leak.

### How long does memory on the stack last versus memory on the heap?

Once a function call runs to completion, any data on the stack created specifically for that function call will automatically be deleted. Any data on the heap will remain there until it’s manually deleted by the programmer.

### Can the stack grow in size? Can the heap grow in size?

The stack is set to a fixed size, and can not grow past it’s fixed size (although some languages have extensions that do allow this). So, if there is not enough room on the stack to handle the memory being assigned to it, a stack overflow occurs. This often happens when a lot of nested functions are being called, or if there is an infinite recursive call.

If the current size of the heap is too small to accommodate new memory, then more memory can be added to the heap by the operating system. This is one of the big differences between the heap and the stack.

### How are the stack and heap implemented?

The implementation really depends on the language, compiler, and run-time – the small details of the implementation for a stack and a heap will always be different depending on what language and compiler are being used. But, in the big picture, the stacks and heaps in one language are used to accomplish the same things as stacks and heaps in another language.

### Which is faster – the stack or the heap? And why?

The stack is much faster than the heap. This is because of the way that memory is allocated on the stack. Allocating memory on the stack is as simple as moving the stack pointer up.

### How is memory deallocated on the stack and heap?

Data on the stack is automatically deallocated when variables go out of scope. However, in languages like C and C++, data stored on the heap has to be deleted manually by the programmer using one of the built in keywords like free, delete, or delete[ ]. Other languages like Java and .NET use garbage collection to automatically delete memory from the heap, without the programmer having to do anything..


### What can go wrong with the stack and the heap?

If the stack runs out of memory, then this is called a stack overflow – and could cause the program to crash. The heap could have the problem of fragmentation, which occurs when the available memory on the heap is being stored as noncontiguous (or disconnected) blocks – because used blocks of memory are in between the unused memory blocks. When excessive fragmentation occurs, allocating new memory may be impossible because of the fact that even though there is enough memory for the desired allocation, there may not be enough memory in one big block for the desired amount of memory.

### Which one should I use – the stack or the heap?

For people new to programming, it’s probably a good idea to use the stack since it’s easier.
Because the stack is small, you would want to use it when you know exactly how much memory you will need for your data, or if you know the size of your data is very small. It’s better to use the heap when you know that you will need a lot of memory for your data, or you just are not sure how much memory you will need (like with a dynamic array).