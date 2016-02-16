# Load Python code dynamically

The normal way to load Python code is through the import statement:
```python
import pprint
pprint.pprint('Hello, world.')
```

But what do you do if you want to dynamically load a module? A classic example of where you’d like to do this is adding ‘extensions’ to your application. Your application has no way of knowing the exact name of the module that it’s going to use; it only knows the filename(s). The way to do this is the imp module:

```python
import md5
import os.path
import imp
import traceback

def load_module(code_path):
    try:
        try:
            code_dir = os.path.dirname(code_path)
            code_file = os.path.basename(code_path)

            fin = open(code_path, 'rb')

            return  imp.load_source(md5.new(code_path).hexdigest(), code_path, fin)
        finally:
            try: fin.close()
            except: pass
    except ImportError, x:
        traceback.print_exc(file = sys.stderr)
        raise
    except:
        traceback.print_exc(file = sys.stderr)
        raise
```