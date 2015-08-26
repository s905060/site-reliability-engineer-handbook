# The standard filedescriptors

Once Initialized, every normal UNIX®-program has at least 3 open files:

#### 0 stdin: standard input
#### 1 stdout: standard output
#### 2 stderr: standard error output

Usually, they're all connected to your terminal, stdin as input file (keyboard), stdout and stderr as output files (screen). When calling such a program, the invoking shell can change these file descriptor connections away from the terminal to any other file (see redirection). Why two different output file descriptors? It's convention to send error messages and warnings to stderr and only program output to stdout. This enables the user to decide if they want to see nothing, only the data, only the errors, or both - and where they want to see them.

When you write a script:

* always read user-input from stdin
* always write diagnostic/error/warning messages to stderr