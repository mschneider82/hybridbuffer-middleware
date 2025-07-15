# HybridBuffer Middleware Interface

A pluggable middleware interface for the HybridBuffer library, providing extensible data processing capabilities.

## Overview

This package defines the core middleware interface for HybridBuffer, enabling developers to create custom data processing plugins that can be seamlessly integrated into the HybridBuffer ecosystem.

## Features

- **Pluggable Architecture**: Clean interface for custom middleware development
- **Extensible Design**: Easy to implement new data processing capabilities
- **Type-Safe**: Strongly typed interface for reliable middleware integration
- **Performance Focused**: Minimal overhead design for high-performance applications

## Installation

```bash
go get schneider.vip/hybridbuffer/middleware
```

## Interface

```go
package middleware

import "io"

// Middleware defines the interface for data processing middleware
type Middleware interface {
    // Process applies the middleware transformation to the data
    Process(data []byte) ([]byte, error)
    
    // Reader returns an io.Reader that applies the middleware transformation
    Reader(r io.Reader) io.Reader
    
    // Writer returns an io.Writer that applies the middleware transformation
    Writer(w io.Writer) io.Writer
}
```

## Usage

### Implementing Custom Middleware

```go
package main

import (
    "io"
    "schneider.vip/hybridbuffer"
    "schneider.vip/hybridbuffer/middleware"
)

type CustomMiddleware struct {
    // Your custom fields
}

func (m *CustomMiddleware) Process(data []byte) ([]byte, error) {
    // Your custom processing logic
    return processedData, nil
}

func (m *CustomMiddleware) Reader(r io.Reader) io.Reader {
    // Return a reader that applies your transformation
    return &customReader{r: r, middleware: m}
}

func (m *CustomMiddleware) Writer(w io.Writer) io.Writer {
    // Return a writer that applies your transformation
    return &customWriter{w: w, middleware: m}
}
```

### Using with HybridBuffer

```go
package main

import (
    "schneider.vip/hybridbuffer"
    "schneider.vip/hybridbuffer/middleware"
)

func main() {
    // Create your custom middleware
    custom := &CustomMiddleware{}
    
    // Create HybridBuffer with middleware
    buf := hybridbuffer.New(
        hybridbuffer.WithMiddleware(custom),
    )
    defer buf.Close()
    
    // Use the buffer - middleware will be applied automatically
    buf.WriteString("Hello, World!")
}
```

## Available Middleware

The HybridBuffer ecosystem provides several ready-to-use middleware implementations:

- **[Compression](../hybridbuffer-middleware-compression)**: High-performance compression using klauspost/compress
- **[Compression (stdlib)](../hybridbuffer-middleware-compressionstdlib)**: Standard library compression
- **[Encryption](../hybridbuffer-middleware-encryption)**: AES-GCM encryption with secure key management

## Contributing

Contributions are welcome! Please feel free to submit a Pull Request.

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.