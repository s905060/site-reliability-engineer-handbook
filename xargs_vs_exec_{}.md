# xargs vs. exec {}

There is a bit of a debate in some circles about using xargs vs. the -exec {} option that’s built into find itself. To me, however, it’s not much of a debate; -exec isn’t nearly as good as xargs for what I use find for. I tend to use it to perform tasks involving many files. “Move all these files there”, “copy all those directories there”, “Delete these links.”, etc.

This is where -exec breaks down and xargs shows its superiority. When you use -exec to do the work you run a separate instance of the called program for each element of input. So if find comes up with 10,000 results, you run exec 10,000 times. With xargs, you build up the input into bundles and run them through the command as few times as possible, which is often just once. When dealing with hundreds or thousands of elements this is a big win for xargs.
That’s all nice and stuff, but you probably want to see it in action, right? Let’s run some numbers. Below is a listing of 1,668 .jpg files on my OS X system using both -exec and xargs:

```
time find . -name "*.jpg" -exec ls {} \;

real    0m6.618s
user    0m1.465s
sys     0m4.396s

```

Hmm, that’s not bad — seven seconds for over around 1,600 files, right? Let’s try it with xargs.

```
# time find . -name "*.jpg" -print0 | xargs -0 ls

real    0m1.120s
user    0m0.594s
sys     0m0.527s
```
That’s one (1) second vs seven (7) seconds. Seriously; xargs is the way to go.
