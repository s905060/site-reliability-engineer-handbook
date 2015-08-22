### COMMON SIGNAL NAMES AND NUMBERS

POSIX (/ˈpɒzɪks/ poz-iks), an acronym for Portable Operating System Interface, is a family of standards specified by the IEEE Computer Society for maintaining compatibility between operating systems. POSIX defines the application programming interface (API), along with command line shells and utility interfaces, for software compatibility with variants of Unix and other operating systems.

| Number | Name | Description | Used for
| --| -- | -- | --
| 0 |SIGNULL |Null | Check access to pid
| 1 |SIGHUP |Hangup | Terminate; can be trapped
| 2 |SIGINT |Interrupt | Terminate; can be trapped
| 3 |SIGQUIT |Quit| Terminate with core dump; can be
| 9 |SIGKILL |Kill | Forced termination; cannot be trapped
| 15 |SIGTERM |Terminate | Terminate; can be trapped
| 24 |SIGSTOP |Stop | Pause the process; cannot be trapped
| 25 |SIGTSTP |Terminal | stop Pause the process; can be
| 26 |SIGCONT |Continue | Run a stopped process