# Go file operations

### Create Empty File
```
package main

import (
    "log"
    "os"
)

var (
    newFile *os.File
    err     error
)

func main() {
    newFile, err = os.Create("test.txt")
    if err != nil {
        log.Fatal(err)
    }
    log.Println(newFile)
    newFile.Close()
}
```

### Truncate a File
```
package main

import (
    "log"
    "os"
)

func main() {
    // Truncate a file to 100 bytes. If file
    // is less than 100 bytes the original contents will remain
    // at the beginning, and the rest of the space is
    // filled will null bytes. If it is over 100 bytes,
    // Everything past 100 bytes will be lost. Either way
    // we will end up with exactly 100 bytes.
    // Pass in 0 to truncate to a completely empty file

    err := os.Truncate("test.txt", 100)
    if err != nil {
        log.Fatal(err)
    }
}
```

### Get File Info
```
package main

import (
    "fmt"
    "log"
    "os"
)

var (
    fileInfo *os.FileInfo
    err      error
)

func main() {
    // Stat returns file info. It will return
    // an error if there is no file.
    fileInfo, err := os.Stat("test.txt")
    if err != nil {
        log.Fatal(err)
    }
    fmt.Println("File name:", fileInfo.Name())
    fmt.Println("Size in bytes:", fileInfo.Size())
    fmt.Println("Permissions:", fileInfo.Mode())
    fmt.Println("Last modified:", fileInfo.ModTime())
    fmt.Println("Is Directory: ", fileInfo.IsDir())
    fmt.Printf("System interface type: %T\n", fileInfo.Sys())
    fmt.Printf("System info: %+v\n\n", fileInfo.Sys())
}
```