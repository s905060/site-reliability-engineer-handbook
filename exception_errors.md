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