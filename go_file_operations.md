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

### Rename and Move a File
```
package main

import (
    "log"
    "os"
)

func main() {
    originalPath := "test.txt"
    newPath := "test2.txt"
    err := os.Rename(originalPath, newPath)
    if err != nil {
        log.Fatal(err)
    }
}
```

### Delete a File
```
package main

import (
    "log"
    "os"
)

func main() {
    err := os.Remove("test.txt")
    if err != nil {
        log.Fatal(err)
    }
}
```

# Open and Close Files
```
package main

import (
    "log"
    "os"
)

func main() {
    // Simple read only open. We will cover actually reading
    // and writing to files in examples further down the page
    file, err := os.Open("test.txt")
    if err != nil {
        log.Fatal(err)
    }
    file.Close()

    // OpenFile with more options. Last param is the permission mode
    // Second param is the attributes when opening
    file, err = os.OpenFile("test.txt", os.O_APPEND, 0666)
    if err != nil {
        log.Fatal(err)
    }
    file.Close()

    // Use these attributes individually or combined
    // with an OR for second arg of OpenFile()
    // e.g. os.O_CREATE|os.O_APPEND
    // or os.O_CREATE|os.O_TRUNC|os.O_WRONLY

    // os.O_RDONLY // Read only
    // os.O_WRONLY // Write only
    // os.O_RDWR // Read and write
    // os.O_APPEND // Append to end of file
    // os.O_CREATE // Create is none exist
    // os.O_TRUNC // Truncate file when opening
}
```

### Check if File Exists
```
package main

import (
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
        if os.IsNotExist(err) {
            log.Fatal("File does not exist.")
        }
    }
    log.Println("File does exist. File information:")
    log.Println(fileInfo)
}
```

### Check Read and Write Permissions
```
package main

import (
    "log"
    "os"
)

func main() {
    // Test write permissions. It is possible the file
    // does not exist and that will return a different
    // error that can be checked with os.IsNotExist(err)
    file, err := os.OpenFile("test.txt", os.O_WRONLY, 0666)
    if err != nil {
        if os.IsPermission(err) {
            log.Println("Error: Write permission denied.")
        }
    }
    file.Close()

    // Test read permissions
    file, err = os.OpenFile("test.txt", os.O_RDONLY, 0666)
    if err != nil {
        if os.IsPermission(err) {
            log.Println("Error: Read permission denied.")
        }
    }
    file.Close()
}
```

### Change Permissions, Ownership, and Timestamps 
```
package main

import (
    "log"
    "os"
    "time"
)

func main() {
    // Change perrmissions using Linux style
    err := os.Chmod("test.txt", 0777)
    if err != nil {
        log.Println(err)
    }

    // Change ownership
    err = os.Chown("test.txt", os.Getuid(), os.Getgid())
    if err != nil {
        log.Println(err)
    }

    // Change timestamps
    twoDaysFromNow := time.Now().Add(48 * time.Hour)
    lastAccessTime := twoDaysFromNow
    lastModifyTime := twoDaysFromNow
    err = os.Chtimes("test.txt", lastAccessTime, lastModifyTime)
    if err != nil {
        log.Println(err)
    }
}
```