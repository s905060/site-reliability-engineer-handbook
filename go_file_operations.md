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

### Hard Links and Symlinks
```
package main

import (
    "os"
    "log"
    "fmt"
)

func main() {
    // Create a hard link
    // You will have two file names that point to the same contents
    // Changing the contents of one will change the other
    // Deleting/renaming one will not affect the other
    err := os.Link("original.txt", "original_also.txt")
    if err != nil {
        log.Fatal(err)
    }

fmt.Println("creating sym")
    // Create a symlink
    err = os.Symlink("original.txt", "original_sym.txt")
    if err != nil {
        log.Fatal(err)
    }

    // Lstat will return file info, but if it is actually
    // a symlink, it will return info about the symlink.
    // It will not follow the link and give information
    // about the real file
    // Symlinks do not work in Windows
    fileInfo, err := os.Lstat("original_sym.txt")
    if err != nil {
        log.Fatal(err)
    }
    fmt.Printf("Link info: %+v", fileInfo)

    // Change ownership of a symlink only 
    // and not the file it points to
    err = os.Lchown("original_sym.txt", os.Getuid(), os.Getgid())
    if err != nil {
        log.Fatal(err)
    }
}
```

### Copy a File
```
package main

import (
    "os"
    "log"
    "io"
)

// Copy a file
func main() {
    // Open original file
    originalFile, err := os.Open("test.txt")
    if err != nil {
        log.Fatal(err)
    }
    defer originalFile.Close()

    // Create new file
    newFile, err := os.Create("test_copy.txt")
    if err != nil {
        log.Fatal(err)
    }
    defer newFile.Close()

    // Copy the bytes to destination from source
    bytesWritten, err := io.Copy(newFile, originalFile)
    if err != nil {
        log.Fatal(err)
    }
    log.Printf("Copied %d bytes.", bytesWritten)
    
    // Commit the file contents
    // Flushes memory to disk
    err = newFile.Sync()
    if err != nil {
        log.Fatal(err)
    }
}
```

### Seek Positions in File
```
package main

import (
    "os"
    "fmt"
    "log"
)

func main() {
    file, _ := os.Open("test.txt")
    defer file.Close()

    // Offset is how many bytes to move
    // Offset can be positive or negative
    var offset int64 = 5

    // Whence is the point of reference for offset
    // 0 = Beginning of file
    // 1 = Current position
    // 2 = End of file
    var whence int = 0
    newPosition, err := file.Seek(offset, whence)
    if err != nil {
        log.Fatal(err)
    }
    fmt.Println("Just moved to 5:", newPosition)

    // Go back 2 bytes from current position
    newPosition, err = file.Seek(-2, 1)
    if err != nil {
        log.Fatal(err)
    }
    fmt.Println("Just moved back two:", newPosition)

    // Find the current position by getting the
    // return value from Seek after moving 0 bytes
    currentPosition, err := file.Seek(0, 1)
    fmt.Println("Current position:", currentPosition)

    // Go to beginning of file
    newPosition, err = file.Seek(0, 0)
    if err != nil {
        log.Fatal(err)
    }
    fmt.Println("Position after seeking 0,0:", newPosition)
}
```

### Write Bytes to a File
```
package main

import (
    "os"
    "log"
)

func main() {
    // Open a new file for writing only
    file, err := os.OpenFile(
        "test.txt",
        os.O_WRONLY|os.O_TRUNC|os.O_CREATE,
        0666,
    )
    if err != nil {
        log.Fatal(err)
    }
    defer file.Close()

    // Write bytes to file
    byteSlice := []byte("Bytes!\n")
    bytesWritten, err := file.Write(byteSlice)
    if err != nil {
        log.Fatal(err)
    }
    log.Printf("Wrote %d bytes.\n", bytesWritten)
}
```

### Quick Write to File
```
package main

import (
    "io/ioutil"
    "log"
)

func main() {
    err := ioutil.WriteFile("test.txt", []byte("Hi\n"), 0666)
    if err != nil {
        log.Fatal(err)
    }
}
```

### Use Buffered Writer
```
package main

import (
    "log"
    "os"
    "bufio"
)

func main() {
    // Open file for writing
    file, err := os.OpenFile("test.txt", os.O_WRONLY, 0666)
    if err != nil {
        log.Fatal(err)
    }
    defer file.Close()

    // Create a buffered writer from the file
    bufferedWriter := bufio.NewWriter(file)

    // Write bytes to buffer
    bytesWritten, err := bufferedWriter.Write(
        []byte{65, 66, 67},
    )
    if err != nil {
        log.Fatal(err)
    }
    log.Printf("Bytes written: %d\n", bytesWritten)

    // Write string to buffer
    // Also available are WriteRune() and WriteByte()   
    bytesWritten, err = bufferedWriter.WriteString(
        "Buffered string\n",
    )
    if err != nil {
        log.Fatal(err)
    }
    log.Printf("Bytes written: %d\n", bytesWritten)

    // Check how much is stored in buffer waiting
    unflushedBufferSize := bufferedWriter.Buffered()
    log.Printf("Bytes buffered: %d\n", unflushedBufferSize)

    // See how much buffer is available
    bytesAvailable := bufferedWriter.Available()
    if err != nil {
        log.Fatal(err)
    }
    log.Printf("Available buffer: %d\n", bytesAvailable)

    // Write memory buffer to disk
    bufferedWriter.Flush()

    // Revert any changes done to buffer that have
    // not yet been written to file with Flush()
    // We just flushed, so there are no changes to revert
    // The writer that you pass as an argument
    // is where the buffer will output to, if you want
    // to change to a new writer
    bufferedWriter.Reset(bufferedWriter) 

    // See how much buffer is available
    bytesAvailable = bufferedWriter.Available()
    if err != nil {
        log.Fatal(err)
    }
    log.Printf("Available buffer: %d\n", bytesAvailable)

    // Resize buffer. The first argument is a writer
    // where the buffer should output to. In this case
    // we are using the same buffer. If we chose a number
    // that was smaller than the existing buffer, like 10
    // we would not get back a buffer of size 10, we will
    // get back a buffer the size of the original since
    // it was already large enough (default 4096)
    bufferedWriter = bufio.NewWriterSize(
        bufferedWriter,
        8000,
    )

    // Check available buffer size after resizing
    bytesAvailable = bufferedWriter.Available()
    if err != nil {
        log.Fatal(err)
    }
    log.Printf("Available buffer: %d\n", bytesAvailable)
}
```

### Read up to n Bytes from File
```
package main

import (
    "os"
    "log"
)

func main() {
    // Open file for reading
    file, err := os.Open("test.txt")
    if err != nil {
        log.Fatal(err)
    }
    defer file.Close()

    // Read up to len(b) bytes from the File
    // Zero bytes written means end of file
    // End of file returns error type io.EOF
    byteSlice := make([]byte, 16)
    bytesRead, err := file.Read(byteSlice)
    if err != nil {
        log.Fatal(err)
    }
    log.Printf("Number of bytes read: %d\n", bytesRead)
    log.Printf("Data read: %s\n", byteSlice)
}
```
### Read Exactly n Bytes
```
package main

import (
    "os"
    "log"
    "io"
)

func main() {
    // Open file for reading
    file, err := os.Open("test.txt")
    if err != nil {
        log.Fatal(err)
    }

    // The file.Read() function will happily read a tiny file in to a large
    // byte slice, but io.ReadFull() will return an
    // error if the file is smaller than the byte slice.
    byteSlice := make([]byte, 2)
    numBytesRead, err := io.ReadFull(file, byteSlice)
    if err != nil {
        log.Fatal(err)
    }
    log.Printf("Number of bytes read: %d\n", numBytesRead)
    log.Printf("Data read: %s\n", byteSlice)
}
```

### Read At Least n Bytes
```
package main

import (
    "os"
    "log"
    "io"
)

func main() {
    // Open file for reading
    file, err := os.Open("test.txt")
    if err != nil {
        log.Fatal(err)
    }

    byteSlice := make([]byte, 512)
    minBytes := 8
    // io.ReadAtLeast() will return an error if it cannot
    // find at least minBytes to read. It will read as
    // many bytes as byteSlice can hold. 
    numBytesRead, err := io.ReadAtLeast(file, byteSlice, minBytes)
    if err != nil {
        log.Fatal(err)
    }
    log.Printf("Number of bytes read: %d\n", numBytesRead)
    log.Printf("Data read: %s\n", byteSlice)
}
```

### Read All Bytes of File
```
package main

import (
    "os"
    "log"
    "fmt"
    "io/ioutil"
)

func main() {
    // Open file for reading
    file, err := os.Open("test.txt")
    if err != nil {
        log.Fatal(err)
    }

    // os.File.Read(), io.ReadFull(), and
    // io.ReadAtLeast() all work with a fixed
    // byte slice that you make before you read

    // ioutil.ReadAll() will read every byte
    // from the reader (in this case a file),
    // and return a slice of unknown slice
    data, err := ioutil.ReadAll(file)
    if err != nil {
        log.Fatal(err)
    }

    fmt.Printf("Data as hex: %x\n", data)
    fmt.Printf("Data as string: %s\n", data)
    fmt.Println("Number of bytes read:", len(data))
}
```

### Quick Read Whole File to Memory
```
package main

import (
    "log"
    "io/ioutil"
)

func main() {
    // Read file to byte slice
    data, err := ioutil.ReadFile("test.txt")
    if err != nil {
        log.Fatal(err)
    }

    log.Printf("Data read: %s\n", data)
}
```

### 