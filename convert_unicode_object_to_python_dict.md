# Convert Unicode Object to Python Dict

```
import ast

ast.literal_eval(u"{u'city': u'new-york', u'name': u'Home', u'display_value': u'2 Main Street'}")
{u'city': u'new-york', u'name': u'Home', u'display_value': u'2 Main Street'}
```