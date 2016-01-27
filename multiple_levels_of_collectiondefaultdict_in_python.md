# Multiple levels of 'collection.defaultdict' in Python

```python
class AutoVivification(dict):
    """Implementation of perl's autovivification feature."""
    def __getitem__(self, item):
        try:
            return dict.__getitem__(self, item)
        except KeyError:
            value = self[item] = type(self)()
            return value
Testing:

a = AutoVivification()

a[1][2][3] = 4
a[1][3][3] = 5
a[1][2]['test'] = 6

print a
Output:

{1: {2: {'test': 6, 3: 4}, 3: {3: 5}}}
```

```python
def rec_dd():
    return defaultdict(rec_dd)

>>> x = rec_dd()
>>> x['a']['b']['c']['d']
defaultdict(<function rec_dd at 0x7f0dcef81500>, {})
>>> print json.dumps(x)
{"a": {"b": {"c": {"d": {}}}}}
```

```python
tree = lambda: defaultdict(tree)
Then you can create your x with x = tree().
```