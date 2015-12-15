# BC

```
$ echo "5*4" | bc
20
$ echo "5+4" | bc
9
$ echo "5-4" | bc
1
```

```
$ x=`echo "5-4" | bc`
$ echo $x
1
```

```
$ echo "sqrt(10)" | bc
3
$ echo "scale=1;sqrt(10)" | bc
3.1
$ echo "scale=10;sqrt(10)" | bc
3.1622776601
```

```
$ echo 'scale=2; 90 * 100 /13' | bc
692.30
```