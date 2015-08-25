# Exception Errors

Below is some common exceptions errors in Python:

###IOError
If the file cannot be opened.

###ImportError
If python cannot find the module

###ValueError
Raised when a built-in operation or function receives an argument that has the
right type but an inappropriate value

###KeyboardInterrupt
Raised when the user hits the interrupt key (normally Control-C or Delete)

###EOFError
Raised when one of the built-in functions (input() or raw_input()) hits an
end-of-file condition (EOF) without reading any data

---

Exception Errors Examples

Now, when we know what some of the exception errors means, let's see some
examples:

### except IOError:
    print('An error occurred trying to read the file.')

### except ValueError:
    print('Non-numeric data found in the file.')

### except ImportError:
    print "NO module found"

### except EOFError:
    print('Why did you do an EOF on me?')

### except KeyboardInterrupt:
    print('You cancelled the operation.')

### except:
    print('An error occurred.')
    
---

Set up exception handling blocks
To use exception handling in Python, you first need to have a catch-all except
clause. 

The words "try" and "except" are Python keywords and are used to catch exceptions.

try-except [exception-name] (see above for examples) blocks

The code within the try clause will be executed statement by statement.

If an exception occurs, the rest of the try block will be skipped and the
except clause will be executed.
```
try:
    some statements here
except:
    exception handling
```

Let's see a short example on how to do this:

```
try:
    print 1/0

except ZeroDivisionError:
    print "You can't divide by zero, you're silly."
```

How does it work?
The error handling is done through the use of exceptions that are caught in try
blocks and handled in except blocks. If an error is encountered, a try block
code execution is stopped and transferred down to the except block. 

In addition to using an except block after the try block, you can also use the
finally block. 

The code in the finally block will be executed regardless of whether an exception
occurs.

### EXAMPLE:
```
import sys

print "Lets fix the previous code with exception handling"

try:
    number = int(raw_input("Enter a number between 1 - 10 \n"))

except ValueError:
    print "Err.. numbers only"
    sys.exit()

print "you entered number \n", number
```

Try ... except ... else clause

The else clause in a try , except statement must follow all except clauses, and is useful for code that must be executed if the try clause does not raise an exception.
```
try:
    data = something_that_can_go_wrong

except IOError:
    handle_the_exception_error

else:
    doing_different_exception_handling
```

Exceptions in the else clause are not handled by the preceding except clauses. 

Make sure that the else clause is run before the finally block.
