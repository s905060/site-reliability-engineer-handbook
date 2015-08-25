# Useful Bash Shell Built-in Commands

Bash has several commands that comes with the shell (i.e built inside the bash shell).

When you execute a built-in command, bash shell executes it immediately, without invoking any other program.

Bash shell built-in commands are faster than external commands, because external commands usually fork a process to execute it.

In this article let us review some useful bash shell builtins with examples.

1. Bash Export Command Example

export command is used to export a variable or function to the environment of all the child processes running in the current shell.
```
export varname=value
export -f functionname # exports a function in the current shell.
```

It exports a variable or function with a value. “env” command lists all the environment variables. In the following example, you can see that env displays the exported variable.
```
$ export country=India

$ env
SESSIONNAME=Console
country=India
_=/usr/bin/env
```

“export -p” command also displays all the exported variable in the current shell.

2. Bash eval Command Example

eval command combines all the given arguments and evaluates the combined expression and executes it, and returns the exit status of the executed command.
```
$ cat evalex.sh
if [ ! -z $1 ]
then
proccomm="ps -e -o pcpu,cpu,nice,state,cputime,args --sort pcpu | grep $1"
else
proccomm="ps -e -o pcpu,cpu,nice,state,cputime,args --sort pcpu"
fi
eval $procomm
```

The above code snippet accepts an argument, which is the pattern for a grep command. This lists the processes in the order of cpu usage and greps for a particular pattern given in the command line.

Note: This article is part of our on-going Bash Tutorial Series.

3. Bash hash Command Example

hash command maintains a hash table, which has the used command’s path names. When you execute a command, it searches for a command in the variable $PATH.
But if the command is available in the hash table, it picks up from there and executes it. Hash table maintains the number of hits encountered for each commands used so far in that shell.
```
$ hash
hits    command
   1    /usr/bin/cat
   2    /usr/bin/ps
   4    /usr/bin/ls
You can delete a particular command from a hash table using -d option, and -r option to reset the complete hash table.

$ hash -d cat
$ hash
hits    command
   2    /usr/bin/ps
   4    /usr/bin/ls
```

4. Bash pwd Command Example

pwd is a shell built-in command to print the current working directory. It basically returns the value of built in variable ${PWD}
```
$pwd
/home/sasikala/bash/exercises

$echo $PWD
/home/sasikala/bash/exercises
```

5. Bash readonly Command Example

readonly command is used to mark a variable or function as read-only, which can not be changed further.
```
$ cat readonly_ex.sh
#!/bin/bash
# Restricting an array as a readonly
readonly -a shells=("ksh" "bash" "sh" "csh" );
echo ${#shells[@]}

# Trying to  modify an array, it throws an error
shells[0]="gnu-bash"

echo ${shells[@]}

$ ./readonly_ex.sh
4
readonly_ex.sh: line 9: shells: readonly variable
```

6. Bash shift Command Example

shift command is used to shift the positional parameters left by N number of times and renames the variable accordingly after shifting.
```
$ cat shift.sh
#! /bin/bash

while [ $# -gt 0 ]
do
        case "$1" in

        -l) echo "List command"
            ls
    	    shift
            ;;
	-p) echo "Process command"
    	    ps -a
    	    shift
    	    ;;
	-t) echo "Hash Table command"
    	    hash
    	    shift
    	    ;;
	-h) echo "Help command"
    	    help
    	    shift
	    ;;
	esac
done

$./shift.sh -l -t
List command analysis  break  testing t1.sh temp Hash Table command
hits    command
   1    /usr/bin/ls
```

7. Bash test Command Example

test command evaluates the conditional expression and returns zero or one based on the evaluation. Refer the manual page of bash, for more test operators.
```
#! /bin/bash

if test -z $1
then
        echo "The positional parameter \$1 is empty"
fi
```

8. Bash getopts Command Example

getopts command is used to parse the given command line arguments. We can define the rules for options i.e which option accepts arguments and which does not. In getopts command, if an option is followed by a colon, then it expects an argument for that option.

getopts provides two variables $OPTIND and $OPTARG which has index of the next parameter and option arguments respectively.
```
$ cat options.sh
#! /bin/bash

while getopts :h:r:l: OPTION
do
         case $OPTION in
          h) echo "help of $OPTARG"
             help "$OPTARG"
             ;;
          r) echo "Going to remove a file $OPTARG"
             rm -f "$OPTARG"
            ;;
         esac
done

$ ./options.sh -h jobs
help of jobs
jobs: jobs [-lnprs] [jobspec ...] or jobs -x command [args]
    Lists the active jobs.  The -l option lists process id's in addition
    to the normal information; the -p option lists process id's only.
```

9. Bash logout Command

```Logout``` built in used to exit a current shell.

10. Bash umask Command Example

umask command sets a file mode creation mask for a current process. When an user creates a file, its default permission is based on the value set in umask. Default permission for file is 666, and it will be masked with the umask bits when user creates a file.

For more details please refer our article File and Directory permissions.

When user creates a file 666 is masked with 022, so default file permission would be 644.
```
$ umask
0022

$ > temporary

$ ls -l temporary
-rw-r--r-- 1 root root 4 Jul 26 07:48 temporary
```

11. Bash set Command Examples

set is a shell built-in command, which is used to set and modify the internal variables of the shell. set command without argument lists all the variables and it’s values. set command is also used to set the values for the positional parameters.
```
$ set +o history # To disable the history storing.
+o disables the given options.

$ set -o history
-o enables the history

$ cat set.sh
var="Welcome to thegeekstuff"
set -- $var
echo "\$1=" $1
echo "\$2=" $2
echo "\$3=" $3

$ ./set.sh
$1=Welcome
$2=to
$3=thegeekstuff
```

12. Bash unset Command Examples

unset built-in is used to set the shell variable to null. unset also used to delete an element of an array and
to delete complete array.

For more details on Bash array, refer our earlier article 15 Bash Array Operations
```
$ cat unset.sh
#!/bin/bash
#Assign values and print it
var="welcome to thegeekstuff"
echo $var

#unset the variable
unset var
echo $var

$ ./unset.sh
welcome to thegeekstuff
```
In the above example, after unset the variable “var” will be assigned with null string.

13. Bash let Command Example

let commands is used to perform arithmetic operations on shell variables.
```
$ cat arith.sh
#! /bin/bash

let arg1=12
let arg2=11

let add=$arg1+$arg2
let sub=$arg1-$arg2
let mul=$arg1*$arg2
let div=$arg1/$arg2
echo $add $sub $mul $div

$ ./arith.sh
23 1 132 1
```

14. Bash shopt Command Example

shopt built in command is used to set and unset a shell options. Using this command, you can make use of shell intelligence.
```
$cat shopt.sh
#! /bin/bash

## Before enabling xpg_echo
echo "WELCOME\n"
echo "GEEKSTUF\n"
shopt -s  xpg_echo
## After enabling xpg_echo
echo "WELCOME\n"
echo "GEEKSTUF\n"

# Before disabling aliases
alias l.='ls -l .'
l.

# After disabling aliases
shopt -u expand_aliases
l.

$ ./shopt.sh
WELCOME\n
GEEKSTUF\n
WELCOME

GEEKSTUF

total 3300
-rw------- 1 root root    1112 Jan 23  2009 anaconda-ks.cfg
-r-xr-xr-x 1 root root 3252304 Jul  1 08:25 backup
drwxr-xr-x 2 root root    4096 Jan 26  2009 Desktop
shopt.sh: line 17: l.: command not found
```

Before enabling xpg_echo option, the echo statement didn’t expand escape sequences. “l.” is aliased to ls -l of current directory. After disabling expand_aliases option in shell, it didn’t expand aliases, you could notice an error l. command not found.

15. Bash printf Command Example

Similar to printf in C language, bash printf built-in is used to format print operations.

In example 13, the script does arithmetic operation on two inputs. In that script instead of echo statement, you can use printf statement to print formatted output as shown below.

In arith.sh, replace the echo statement with this printf statement.
```
printf "Addition=%d\nSubtraction=%d\nMultiplication=%d\nDivision=%f\n" $add $sub $mul $div

$ ./arith.sh
Addition=23
Subtraction=1
Multiplication=132
Division=1.000000
```