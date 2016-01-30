# Data Structures I

### Introduction
Data structures provide a way to organize the data for your program in a way that is efficient and easy to use. For example, in an air combat game, there would likely be a data structure keeping track of the thirty missiles your plane has fired, the six other planes in your squadron, and the fifty alien ships that you are trying to shoot down. There are many different data structures that might be used to keep track of these objects, each of which is suited to organizing the data differently. This article is the first in a series that is intended to be a guide to using data structures in games, and should help you decide which data structure is best suited for a task. The articles are designed to be at an introductory level. However, the series also contains examples of applications of data structures to games, so programmers who are strong in other areas, but with little game experience, may benefit as well.

At the most basic level, data structures allow you to perform three basic operations: adding data, accessing data, and modifying data. By analyzing the most frequent ways that your data is used, you can decide which data structure is appropriate. Each data structure is designed to be good at one kind of usage pattern, often at the expense of another. For example, a data structure might excel at accessing data in order, using a simple for loop. The same data structure might be much slower when asked to access a random element, however. Some data structures are geared towards easily adding new data, while others are fixed size. One of the arts in software development is learning to match data structures to how their data will be used. Poor data structure choice is often a significant factor in game performance problems.

The decision of which data structure to use, then, boils down to an analysis of how the data is likely to be used. To use the previous example of an air combat game, how often will you need to add new missiles? Will they be added every frame, or is the number of missiles a constant? How will you examine the data that makes up a missile? Will you always access missiles one after another, or will you need access to any missile at any time? Once you have a sense of how your data is likely to be used, you can decide which data structure is appropriate.

One quick disclaimer: any time this article discusses the internal workings of .NET Framework or XNA Framework classes, remember that private implementation details are always subject to change from version to version.

###Big O
There are a couple of things to explain before we can begin discussing the data structures themselves. First, in order to talk about the performance of different data structures, we're going to need to have some kind of framework for analyzing performance. If I tried to write this whole article by calling everything fast, slow, slowish, or not-that-great-but-faster-than-the-other-guy, I'd go crazy.

To that end, I’m going to briefly explain something called Big O notation. Big O notation gives us a sense of how many steps it will take to complete a task or algorithm. Say, for example, I have the following function:
```
float Double(float x)
{
    return x * 2;
}
```

Examining this function, we can see that it will always take one step to complete, no matter what the input to the function is. This means the function will run in what’s called "constant time," expressed in Big O notation as O(1). Another function might be one that computes n factorial (n!):
```
int NFactorial(int n)
{
    int result = 1;
    for (int i = n; i > 0; i--)
    {
        result *= i;
    }
    return result;
}
```

If we assume that the code that handles the assignment, comparison, and return statements is trivial and can be ignored, we see that the performance of this function will be dominated by the line result *= i;. Since that line of code is in a loop that will be executed n times, this function is O(n), "linear time."

Let’s look at one more function.
```
int Badness(int n)
{
    int result = 1;
    for (int i = n; i > 0; i--)
    {
        for (int j = n; j > 0; j--)
        {
            result *= j;
        }
    }
    return result;
}
```

The function may seem to be a bit contrived—what's the point of it, after all? However, it's worth mentioning the double loop that this function uses. This is something that will pop up frequently in games programs, particularly in collision code, as we'll see later. So, what's so bad about this function? There's still only one multiplication happening. Again, the multiplication is inside of a loop, so it will happen n times, but the loop is also inside a loop, so it too will happen n times. All told, our one lonely multiplication will actually happen n2 times, giving us a Big O of O(n2), which is called "quadratic time."

Watch out for functions like these: they don't look that bad from glancing at the code, and with small values of n they are not that bad. As n increases, though, you will quickly find you have a monster on your hands. Any time you're writing an algorithm, and you find yourself thinking "I'll just check everything against everything else," stop and ask yourself if there is a better way you could do it.

Here are several common running times and their Big O notation, from fastest to slowest:

|  |  |
|--|--|
|Constant time	|O(1)
|Log n time	    |O(Log(n))
|Linear time	    |O(n)
|Quadratic time	|O(n2)
|Factorial time	|O(n!)

When expressing a function's running time in Big O, only the most significant term of the function is written. For example, say we've analyzed another function that computes factorials, and we've seen that it takes n2 + n + 5 steps to complete. Once n grows very large, the n2 term in n2 + n + 5 will become the most significant term in the function: it will contribute the most to the result. For clarity when comparing different functions' run times, only the most significant term is written. Therefore, our function n2 + n + 5 has a Big O of O(n2). Simplifying like this allows us to easily compare the running times of various functions, jumping right to the "meat" of the functions' performance. Constant time functions are always written as O(1).

This "most significant term only" simplification obviously breaks down, however, when n is not large. For example, say we have two functions with running times of n + 100 and n2. The Big O of the first is O(n), and the second is O(n2). If the two functions accomplish the same goal, a naïve approach would be to simply always use the first function, since O(n) is faster than O(n2). However, notice that for all values of n less than 10, the second function, O(n2), is actually faster.

Situations like this are fairly common. In many cases, there are two ways to accomplish a certain task: the brute force approach, and the clever approach. The clever approach is often faster, but requires some setup work beforehand. With small data sets, it's often faster to just use the brute force algorithm, rather than pay the setup cost of the cleverer one. It is worthwhile to consider these "hidden costs" when determining which algorithm to use.

For a tabular view of Big O notation and the pros and cons of each algorithm, see Cheat Sheet: Speed of Common Operations.

Generics
The last thing I need to cover before we can start discussing data structures themselves is a technology called generics. Generics were added to C# in version 2.0, and provide a way to reuse code while still having type safety. For example, let’s say I have a class called a Holder. Its only purpose is to hold on to one piece of data, so that different callers can share data between themselves without needing to know who else they might be sharing with. If I want the Holder to hold integers, it might look like this:

class IntHolder
{
    private int heldValue;
    public int HeldValue
    {
        get { return heldValue; }
        set { heldValue = value; }
    }
}
Now imagine I want a Holder that holds floats, or Booleans, or anything. I have to make a new class for every single kind of thing I might want to hold. This is a pretty trivial class, so it might not be that much code, but it clearly demonstrates the need for a better solution.

Alternatively, the Holder could hold objects, so that one class can hold anything, but this isn’t ideal either. When someone accesses Holder's HeldValue, the first thing they’ll do is cast it to what they expect it to be so they can use it. For example, suppose person A puts an int value into the Holder. Then person B comes along and tries to get at the value in the Holder. The problem is B thinks it’s supposed to be a string, and the minute he tries to cast it that way, things will go downhill fast. Obviously, the type safety that we had with our separate IntHolder and StringHolder structures was quite useful.

C# 2.0 addressed this issue by adding generics, which are extremely powerful and useful. Their syntax is similar to that of C++’s templates (meaning that it’s confusing). Changing our Holder to a generic would make it look like this:

class Holder<T>
{
    private T heldValue;
    public T HeldValue
    {
        get { return heldValue; }
        set { heldValue = value; }
    }
}
The <T> bit means our Holder is a generic class, specialized on some type that will be called T. Then, when someone comes along to use our Holder, they replace the T with the type they want, like so:

Holder<int> holder = new Holder<int>();
holder.HeldValue = 10;

Holder<string> stringHolder = new Holder<string>();
stringHolder.HeldValue = "Camelus Maximus";
Now we’ve got the best of both worlds: code reuse and type safety. Most of the data structures we’re going to examine will be generics, so it’s important to be comfortable working with them.

Conclusion
Now that we’ve laid the groundwork for examining data structures, in Part II we can begin to discuss the actual data structures. I realize that this first part may be a bit of a tease (a data structures article that doesn’t actually talk about any data structures?), but understanding Big O and generics is crucial to the rest of these articles, so it’s important to discuss them early.