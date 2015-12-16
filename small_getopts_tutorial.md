# Small getopts tutorial

Description
When you want to parse command line arguments in a professional way, getopts is the tool of choice. Unlike its older brother getopt (note the missing s!), it's a shell builtin command. The advantages are:

* No need to pass the positional parameters through to an external program.
* Being a builtin, getopts can set shell variables to use for parsing (impossible for an external process!)
* There's no need to argue with several getopt implementations which had buggy concepts in the past (whitespace, …)
* getopts is defined in POSIX®.

Some other methods to parse positional parameters (without getopt(s)) are described in: How to handle positional parameters.

Note that getopts is not able to parse GNU-style long options (--myoption) or XF86-style long options (-myoption)!

###Terminology
It's useful to know what we're talking about here, so let's see… Consider the following command line:

`mybackup -x -f /etc/mybackup.conf -r ./foo.txt ./bar.txt`

These are all positional parameters, but they can be divided into several logical groups:

* -x is an option (aka flag or switch). It consists of a dash (-) followed by one character.
* -f is also an option, but this option has an associated option argument (an argument to the option -f): /etc/mybackup.conf. The option argument is usually the argument following the option itself, but that isn't mandatory. Joining the option and option argument into a single argument -f/etc/mybackup.conf is valid.
* -r depends on the configuration. In this example, -r doesn't take arguments so it's a standalone option like -x.
* ./foo.txt and ./bar.txt are remaining arguments without any associated options. These are often used as mass-arguments. For example, the filenames specified for cp(1), or arguments that don't need an option to be recognized because of the intended behavior of the program. POSIX® calls them operands.
To give you an idea about why getopts is useful, The above command line is equivalent to:

`mybackup -xrf /etc/mybackup.conf ./foo.txt ./bar.txt`

which is complex to parse without the help of getopts.

The option flags can be upper- and lowercase characters, or digits. It may recognize other characters, but that's not recommended (usability and maybe problems with special characters).

###How it works
In general you need to call getopts several times. Each time it will use the next positional parameter and a possible argument, if parsable, and provide it to you. getopts will not change the set of positional parameters. If you want to shift them, it must be done manually:
```
shift $((OPTIND-1))
# now do something with $@
```

Since getopts sets an exit status of FALSE when there's nothing left to parse, it's easy to use in a while-loop:
```
while getopts ...; do
  ...
done
```

getopts will parse options and their possible arguments. It will stop parsing on the first non-option argument (a string that doesn't begin with a hyphen (-) that isn't an argument for any option in front of it). It will also stop parsing when it sees the -- (double-hyphen), which means end of options.