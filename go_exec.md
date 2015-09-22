# Go exec

Package exec runs external commands. It wraps os.StartProcess to make it easier to remap stdin and stdout, connect I/O with pipes, and do other adjustments.

### func LookPath

> func LookPath(file string) (string, error)

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

### func (*Cmd) Output

> func (c *Cmd) Output() ([]byte, error)

Output runs the command and returns its standard output.

```
package main

import (
	"fmt"
	"log"
	"os/exec"
)

func main() {
	out, err := exec.Command("date").Output()
	if err != nil {
		log.Fatal(err)
	}
	fmt.Printf("The date is %s\n", out)
}
```

### func (*Cmd) Start

> func (c *Cmd) Start() error

Start starts the specified command but does not wait for it to complete.

The Wait method will return the exit code and release associated resources once the command exits.

```
package main

import (
	"log"
	"os/exec"
)

func main() {
	cmd := exec.Command("sleep", "5")
	err := cmd.Start()
	if err != nil {
		log.Fatal(err)
	}
	log.Printf("Waiting for command to finish...")
	err = cmd.Wait()
	log.Printf("Command finished with error: %v", err)
}
```

### func (*Cmd) StdoutPipe

> func (c *Cmd) StdoutPipe() (io.ReadCloser, error)

StdoutPipe returns a pipe that will be connected to the command's standard output when the command starts.

Wait will close the pipe after seeing the command exit, so most callers need not close the pipe themselves; however, an implication is that it is incorrect to call Wait before all reads from the pipe have completed. For the same reason, it is incorrect to call Run when using StdoutPipe. See the example for idiomatic usage.
```
package main

import (
	"encoding/json"
	"fmt"
	"log"
	"os/exec"
)

func main() {
	cmd := exec.Command("echo", "-n", `{"Name": "Bob", "Age": 32}`)
	stdout, err := cmd.StdoutPipe()
	if err != nil {
		log.Fatal(err)
	}
	if err := cmd.Start(); err != nil {
		log.Fatal(err)
	}
	var person struct {
		Name string
		Age  int
	}
	if err := json.NewDecoder(stdout).Decode(&person); err != nil {
		log.Fatal(err)
	}
	if err := cmd.Wait(); err != nil {
		log.Fatal(err)
	}
	fmt.Printf("%s is %d years old\n", person.Name, person.Age)
}
```