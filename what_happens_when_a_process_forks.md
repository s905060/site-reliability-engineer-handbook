#What happens when a process forks?

The child gets a copy of the stack & program, and there are two processes running. You differentiate the child from the parent by fork's ending status. If zero, you're in the child. If not, you're in the parent and the ending status is the PID of the child.