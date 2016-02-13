# Lambda, filter, reduce and map

###Lambda Operator

Ring als Symbol der for-Schleife
Some like it, others hate it and many are afraid of the lambda operator. We are confident that you will like it, when you have finished with this chapter of our tutorial. If not, you can learn all about "List Comprehensions", Guido van Rossums preferred way to do it, because he doesn't like Lambda, map, filter and reduce either. 

because The lambda operator or lambda function is a way to create small anonymous functions, i.e. functions without a name. These functions are throw-away functions, i.e. they are just needed where they have been created. Lambda functions are mainly used in combination with the functions filter(), map() and reduce(). The lambda feature was added to Python due to the demand from Lisp programmers. 

The general syntax of a lambda function is quite simple:
lambda argument_list: expression 
The argument list consists of a comma separated list of arguments and the expression is an arithmetic expression using these arguments. You can assign the function to a variable to give it a name. 
The following example of a lambda function returns the sum of its two arguments:
```
>>> f = lambda x, y : x + y
>>> f(1,1)
2
```