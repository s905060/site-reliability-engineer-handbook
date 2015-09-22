# Go exec

Package exec runs external commands. It wraps os.StartProcess to make it easier to remap stdin and stdout, connect I/O with pipes, and do other adjustments.

### func LookPath

>func LookPath(file string) (string, error)

LookPath searches for an executable binary named file in the directories named by the PATH environment variable. If file contains a slash, it is tried directly and the PATH is not consulted. The result may be an absolute path or a path relative to the current directory.

```
package main

import (
	"fmt"
	"log"
	"os/exec"
)

func main() {
	path, err := exec.LookPath("fortune")
	if err != nil {
		log.Fatal("installing fortune is in your future")
	}
	fmt.Printf("fortune is available at %s\n", path)
}
```