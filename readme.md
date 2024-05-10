# go-upload

[![Static Badge](https://img.shields.io/badge/Go-blue.svg)](https://go.dev/) [![Static Badge](https://img.shields.io/badge/v2.0.0-blue.svg)](https://go.dev/)

Go-upload is a Go package for handling file uploads in HTTP requests. It simplifies the process of uploading single or multiple files and saving them to a specified directory.

## Installation

You can install `go-upload` using the `go get` command:

```bash
go get github.com/celpung/go-upload
```

## Usage

### Importing the Package

Import the package in your Go code:

```go
import "github.com/celpung/go-upload"
```

### Single file upload
To handle the upload of a single file from an HTTP request, you can use the "Single" function:
```go
uploadedFile, err := goupload.Single(request, directory, fieldName)
if err != nil {
    // Handle the error
}
if uploadedFile != nil {
    // The file was successfully uploaded
    fmt.Printf("Uploaded file: %s\n", uploadedFile.Filename)
}
```

### Multiple file upload
To handle the upload of multiple files from an HTTP request, you can use the "Multiple" function:
```go
uploadedFiles, err := goupload.Multiple(request, directory, fieldName)
if err != nil {
    // Handle the error
}
if len(uploadedFiles) > 0 {
    // Files were successfully uploaded
    for _, file := range uploadedFiles {
        fmt.Printf("Uploaded file: %s\n", file.Filename)
    }
}
```

### Example
Here's a simple example of how to use go-upload in a web server:
```go
package main

import (
    "fmt"
    "net/http"

    "github.com/celpung/goupload"
)

func uploadHandler(w http.ResponseWriter, r *http.Request) {
    // Handle single file upload
    uploadedFile, err := goupload.Single(r, "./uploads", "file")
    if err != nil {
        http.Error(w, "Upload failed", http.StatusInternalServerError)
        return
    }
    if uploadedFile != nil {
        fmt.Fprintf(w, "Uploaded file: %s\n", uploadedFile.Filename)
    }

    // Handle multiple file upload
    uploadedFiles, err := goupload.Multiple(r, "./uploads", "files")
    if err != nil {
        http.Error(w, "Upload failed", http.StatusInternalServerError)
        return
    }
    if len(uploadedFiles) > 0 {
        for _, file := range uploadedFiles {
            fmt.Fprintf(w, "Uploaded file: %s\n", file.Filename)
        }
    }
}

func main() {
    http.HandleFunc("/upload", uploadHandler)
    http.ListenAndServe(":8080", nil)
}
```

## Contributing

Contributions are welcome! To contribute to go-upload, follow these steps:

1. Fork this repository.
2. Create a new branch: git checkout -b new-feature
3. Make changes and commit: git commit -m 'Add new feature'
4. Push to your forked repository: git push origin new-feature
5. Create a pull request explaining your changes.
6. Thank you for contributing!

## License

This package is distributed under the [MIT License](https://opensource.org/license/mit/).




