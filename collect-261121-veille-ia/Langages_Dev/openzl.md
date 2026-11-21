---
id: collect-261121-veille-ia/langages-dev/openzl
title: "openzl"
domain: langages-dev
role: reference
task: article
actors: []
dates: []
keywords: ["openzl", "compressor", "compressed", "message", "data", "byte", "indicates", "errors"]
source: https://pkg.go.dev/github.com/boris-chu/go-openzl
source_anchor: ""
source_lines: [1, 621]
section: "Langages_Dev"
---
## Documentation ¶

### Overview ¶

Package openzl provides Go bindings for Meta's OpenZL format-aware compression library.

OpenZL delivers high compression ratios while preserving high speed, a level of performance that is out of reach for generic compressors. It takes a description of your data and builds from it a specialized compressor optimized for your specific format.

#### Quick Start ¶

For simple one-shot compression and decompression:

```
// Compress data
compressed, err := openzl.Compress([]byte("hello world"))
if err != nil {
	log.Fatal(err)
}
// Decompress data
decompressed, err := openzl.Decompress(compressed)
if err != nil {
	log.Fatal(err)
}
```
#### Context-Based API ¶

For better performance with repeated operations, use reusable contexts:

```
compressor, err := openzl.NewCompressor()
if err != nil {
	log.Fatal(err)
}
defer compressor.Close()
dst := make([]byte, compressor.CompressBound(len(src)))
n, err := compressor.Compress(dst, src)
if err != nil {
	log.Fatal(err)
}
compressed := dst[:n]
```
#### Thread Safety ¶

Compressor and Decompressor instances are safe for concurrent use by multiple goroutines. Each instance uses a mutex to serialize access to the underlying C context.

#### Requirements ¶

This package requires CGO and links against the OpenZL C library. The library will be automatically built as part of the Go build process if not already present.

#### Platform Support ¶

Currently supported platforms:

- Linux (amd64, arm64)
- macOS (amd64, arm64)
- Windows (amd64) - experimental

#### More Information ¶

For more details about OpenZL, see:

- GitHub: https://github.com/facebook/openzl
- Documentation: http://openzl.org/
- Whitepaper: https://arxiv.org/abs/2510.03203

## Example (CompressToZeroAlloc) ¶

Example_compressToZeroAlloc demonstrates the zero-allocation pattern

```
package main
import (
	"fmt"
	"github.com/boris-chu/go-openzl"
)
func main() {
	compressor, _ := openzl.NewCompressor()
	defer compressor.Close()
	// Pre-allocate buffer once for maximum message size
	maxSize := 1024 * 1024 // 1MB
	dst := make([]byte, openzl.CompressBound(maxSize))
	// Process many messages with zero allocations!
	messages := [][]byte{
		[]byte("First message"),
		[]byte("Second message"),
		[]byte("Third message"),
	}
	for i, msg := range messages {
		n, err := compressor.CompressTo(dst, msg)
		if err != nil {
			panic(err)
		}
		// Use dst[:n] - no allocation!
		fmt.Printf("Message %d compressed to %d bytes\n", i, n)
		_ = dst[:n] // In real code, send this over network/write to file
	}
}
```
Output:
Message 0 compressed to 45 bytes
Message 1 compressed to 46 bytes
Message 2 compressed to 45 bytes

### Index ¶

### Examples ¶

### Constants ¶

const (
	// DefaultFrameSize is the default buffer size for streaming compression.
	// 64KB provides a good balance between compression ratio and memory usage.
	DefaultFrameSize = 64 * 1024
	// MinFrameSize is the minimum frame size (4KB).
	MinFrameSize = 4 * 1024
	// MaxFrameSize is the maximum frame size (1MB).
	MaxFrameSize = 1024 * 1024
)

Version is the current version of go-openzl

### Variables ¶

var (
	// ErrEmptyInput indicates that the input buffer is empty
	ErrEmptyInput = errors.New("openzl: empty input")
	// ErrBufferTooSmall indicates that the destination buffer is too small
	ErrBufferTooSmall = errors.New("openzl: buffer too small")
	// ErrCorruptedData indicates that the compressed data is corrupted
	ErrCorruptedData = errors.New("openzl: corrupted data")
	// ErrInvalidParameter indicates an invalid parameter was passed
	ErrInvalidParameter = errors.New("openzl: invalid parameter")
	// ErrContextClosed indicates that the context has been closed
	ErrContextClosed = errors.New("openzl: context closed")
	// ErrOutOfMemory indicates that memory allocation failed
	ErrOutOfMemory = errors.New("openzl: out of memory")
)

### Functions ¶

Compress compresses the input data using OpenZL with default settings. It returns the compressed data or an error.

This is a simple one-shot compression function suitable for occasional use. For better performance with repeated operations, use the Compressor type.

Example:

```
data := []byte("hello world")
compressed, err := openzl.Compress(data)
if err != nil {
	log.Fatal(err)
}
```
  
        #### func CompressBound ¶

CompressBound returns the maximum size of compressed data for input of size srcSize.

Use this function to pre-allocate buffers for CompressTo():

dst := make([]byte, openzl.CompressBound(len(src)))
n, err := compressor.CompressTo(dst, src)
compressed := dst[:n]

#### func CompressNumeric ¶

CompressNumeric compresses a slice of numeric values using OpenZL's typed compression.

This function leverages OpenZL's format-aware compression to achieve significantly better compression ratios (2-5x) on numeric data compared to the untyped Compress function. It works best with structured or sorted numeric data.

Supported types: int8, uint8, int16, uint16, int32, uint32, int64, uint64, float32, float64

Example:

```
numbers := []int64{1, 2, 3, 4, 5, 100, 101, 102, 103}
compressed, err := openzl.CompressNumeric(numbers)
if err != nil {
	log.Fatal(err)
}
// Decompress back to typed slice
decompressed, err := openzl.DecompressNumeric[int64](compressed)
```
Returns an error if:

- the input slice is empty
- the compression operation fails

CompressorCompressNumeric compresses a slice of numeric values using a reusable compression context.

This function combines the performance benefits of the Context API (Phase 2) with the compression ratio improvements of typed compression (Phase 3).

Example:

```
compressor, _ := openzl.NewCompressor()
defer compressor.Close()
numbers := []int64{1, 2, 3, 4, 5, 100, 101, 102}
compressed, err := openzl.CompressorCompressNumeric(compressor, numbers)
```
Returns an error if:

- the input slice is empty
- the compression operation fails

#### func Decompress ¶

Decompress decompresses OpenZL-compressed data. It returns the decompressed data or an error.

This is a simple one-shot decompression function suitable for occasional use. For better performance with repeated operations, use the Decompressor type.

Example:

```
decompressed, err := openzl.Decompress(compressed)
if err != nil {
	log.Fatal(err)
}
```
  
        #### func DecompressNumeric ¶

DecompressNumeric decompresses data that was compressed with CompressNumeric.

The type parameter T must match the type used during compression, otherwise the decompression will fail or produce incorrect results.

Example:

```
compressed, _ := openzl.CompressNumeric([]int64{1, 2, 3, 4, 5})
decompressed, err := openzl.DecompressNumeric[int64](compressed)
if err != nil {
	log.Fatal(err)
}
// decompressed is []int64{1, 2, 3, 4, 5}
```
Returns an error if:

- the input is empty
- the compressed data is invalid or corrupted
- the type parameter doesn't match the original compression type

DecompressorDecompressNumeric decompresses numeric data using a reusable decompression context.

This function combines the performance benefits of the Context API (Phase 2) with typed decompression (Phase 3).

Example:

decompressor, _ := openzl.NewDecompressor()
defer decompressor.Close()
decompressed, err := openzl.DecompressorDecompressNumeric[int64](decompressor, compressed)

Returns an error if:

- the input is empty
- the compressed data is invalid or corrupted
- the type parameter doesn't match the original compression type

### Types ¶

#### type Compressor ¶

```
type Compressor struct {
	// contains filtered or unexported fields
}
```
    Compressor provides a reusable compression context with thread safety.

Unlike the one-shot Compress function, Compressor maintains an internal compression context that can be reused across multiple operations, providing 10-50% better performance for repeated compressions.

Compressor is safe for concurrent use by multiple goroutines. Each compression operation is protected by an internal mutex, ensuring thread safety while allowing the underlying context to be reused.

Example:

```
compressor, err := openzl.NewCompressor()
if err != nil {
	log.Fatal(err)
}
defer compressor.Close()
// Reuse the compressor for multiple operations
for _, data := range inputs {
	compressed, err := compressor.Compress(data)
	if err != nil {
		log.Printf("compression failed: %v", err)
		continue
	}
	// Use compressed data...
}
```
#### func NewCompressor ¶

NewCompressor creates a new reusable Compressor with optional configuration.

The returned Compressor is safe for concurrent use by multiple goroutines. When finished, call Close() to release the underlying compression context and prevent memory leaks.

Options can be provided to customize compression behavior:

compressor, err := openzl.NewCompressor(
	openzl.WithCompressionLevel(9),
	openzl.WithChecksum(true),
)

Returns an error if the underlying compression context cannot be created or if any of the provided options are invalid.

Close releases the underlying compression context and frees associated memory.

After calling Close, the Compressor cannot be used for further compression operations. Calling Close multiple times is safe and has no effect after the first call.

It is recommended to use defer to ensure Close is called:

```
compressor, err := openzl.NewCompressor()
if err != nil {
	return err
}
defer compressor.Close()
```
  
  Compress compresses the input data using the reusable compression context.

This method is safe for concurrent use by multiple goroutines. Each call acquires an internal lock, compresses the data using the shared context, and then releases the lock.

The input data is not modified. The returned compressed data is a newly allocated slice containing only the compressed bytes (no extra capacity).

Returns an error if:

- src is empty (use ErrEmptyInput check)
- the underlying compression operation fails

Example:

```
compressed, err := compressor.Compress([]byte("hello world"))
if err != nil {
	log.Fatal(err)
}
```
  
  #### func (*Compressor) CompressTo ¶

CompressTo compresses src into dst, returning the number of bytes written.

This method allows the caller to provide the destination buffer, avoiding allocations entirely in steady-state compression. This is a Klaus Post-inspired pattern for maximum performance.

The dst buffer must be large enough to hold the compressed data. Use CompressBound(len(src)) to determine the required size.

Returns an error if:

- src is empty
- dst is too small to hold compressed data
- compression fails

Example (zero allocations in steady state):

```
// Pre-allocate buffer once
dst := make([]byte, openzl.CompressBound(maxInputSize))
for _, data := range inputs {
	n, err := compressor.CompressTo(dst, data)
	if err != nil {
		log.Fatal(err)
	}
	// Use dst[:n] - no allocation!
	sendCompressed(dst[:n])
}
```
  
  #### type Decompressor ¶

```
type Decompressor struct {
	// contains filtered or unexported fields
}
```
    Decompressor provides a reusable decompression context with thread safety.

Unlike the one-shot Decompress function, Decompressor maintains an internal decompression context that can be reused across multiple operations, providing 10-50% better performance for repeated decompressions.

Decompressor is safe for concurrent use by multiple goroutines. Each decompression operation is protected by an internal mutex, ensuring thread safety while allowing the underlying context to be reused.

Example:

```
decompressor, err := openzl.NewDecompressor()
if err != nil {
	log.Fatal(err)
}
defer decompressor.Close()
// Reuse the decompressor for multiple operations
for _, compressed := range compressedInputs {
	decompressed, err := decompressor.Decompress(compressed)
	if err != nil {
		log.Printf("decompression failed: %v", err)
		continue
	}
	// Use decompressed data...
}
```
#### func NewDecompressor ¶

NewDecompressor creates a new reusable Decompressor.

The returned Decompressor is safe for concurrent use by multiple goroutines. When finished, call Close() to release the underlying decompression context and prevent memory leaks.

Example:

```
decompressor, err := openzl.NewDecompressor()
if err != nil {
	return err
}
defer decompressor.Close()
```
Returns an error if the underlying decompression context cannot be created.

Close releases the underlying decompression context and frees associated memory.

After calling Close, the Decompressor cannot be used for further decompression operations. Calling Close multiple times is safe and has no effect after the first call.

It is recommended to use defer to ensure Close is called:

```
decompressor, err := openzl.NewDecompressor()
if err != nil {
	return err
}
defer decompressor.Close()
```
  
  #### func (*Decompressor) Decompress ¶

Decompress decompresses OpenZL-compressed data using the reusable decompression context.

This method is safe for concurrent use by multiple goroutines. Each call acquires an internal lock, decompresses the data using the shared context, and then releases the lock.

The input data is not modified. The returned decompressed data is a newly allocated slice containing only the decompressed bytes (no extra capacity).

Returns an error if:

- src is empty (use ErrEmptyInput check)
- src does not contain valid OpenZL compressed data
- the compressed data is corrupted
- the underlying decompression operation fails

Example:

```
decompressed, err := decompressor.Decompress(compressedData)
if err != nil {
	log.Fatal(err)
}
```
  
  ```
type Numeric interface {
	int8 | uint8 | int16 | uint16 | int32 | uint32 | int64 | uint64 | float32 | float64
}
```
    Numeric is a constraint that permits all numeric types that OpenZL supports. OpenZL supports numeric types with widths of 1, 2, 4, and 8 bytes.

```
type Reader struct {
	// contains filtered or unexported fields
}
```
    Reader implements io.ReadCloser for streaming decompression.

Reader reads compressed frames from the underlying reader and decompresses them on demand. This allows decompression of large data streams without loading the entire compressed input into memory.

Example:

```
file, _ := os.Open("input.zl")
reader, _ := openzl.NewReader(file)
defer reader.Close()
// Decompress data as it's read
io.Copy(destWriter, reader)
```
The Reader reads frames written by Writer, which have a 4-byte little-endian frame length header followed by compressed data.

NewReader creates a new Reader that reads compressed data from r and decompresses it.

The returned Reader implements io.ReadCloser. You should call Close() when done reading to release resources.

Example:

```
file, err := os.Open("input.zl")
if err != nil {
    log.Fatal(err)
}
defer file.Close()
reader, err := openzl.NewReader(file)
if err != nil {
    log.Fatal(err)
}
defer reader.Close()
data, err := io.ReadAll(reader)
if err != nil {
    log.Fatal(err)
}
```
  
  Close releases resources associated with the Reader.

Calling Close() multiple times is safe and has no effect after the first call.

Read decompresses data from the underlying reader into p.

Read implements the io.Reader interface. It reads and decompresses frames as needed to fill p. When the end-of-stream marker is reached, Read returns io.EOF.

If an error occurs, the Reader enters an error state and all subsequent Read calls will return the same error.

Reset resets the Reader to read from a new underlying reader.

This allows reuse of the Reader and its internal decompressor context for better performance when decompressing multiple streams.

If the Reader was previously closed, Reset will create a new decompressor.

Example:

reader, _ := openzl.NewReader(file1)
io.Copy(dest1, reader)
reader.Close()
reader.Reset(file2)  // Reuse the reader
io.Copy(dest2, reader)
reader.Close()

```
type Writer struct {
	// contains filtered or unexported fields
}
```
    Writer implements io.WriteCloser for streaming compression.

Writer buffers input data and compresses it in chunks, writing compressed frames to the underlying writer. This allows compression of large data streams without loading the entire input into memory.

Example:

```
file, _ := os.Create("output.zl")
writer, _ := openzl.NewWriter(file)
defer writer.Close()
// Compress data as it's written
io.Copy(writer, sourceReader)
```
Important: You must call Close() to flush any buffered data and ensure all compressed data is written to the underlying writer.

NewWriter creates a new Writer that compresses data and writes it to w.

The returned Writer implements io.WriteCloser. You must call Close() when done writing to flush any buffered data.

Example:

```
file, err := os.Create("output.zl")
if err != nil {
    log.Fatal(err)
}
defer file.Close()
writer, err := openzl.NewWriter(file)
if err != nil {
    log.Fatal(err)
}
defer writer.Close()
fmt.Fprintf(writer, "Hello, OpenZL!")
```
  
  Close flushes any buffered data, writes final compressed frame, and releases resources.

You must call Close() to ensure all data is written. Calling Close() multiple times is safe and has no effect after the first call.

Reset resets the Writer to write to a new underlying writer.

This allows reuse of the Writer and its internal compressor context for better performance when compressing multiple streams.

If the Writer was previously closed, Reset will create a new compressor.

Example:

writer, _ := openzl.NewWriter(file1)
io.Copy(writer, data1)
writer.Close()
writer.Reset(file2)  // Reuse the writer
io.Copy(writer, data2)
writer.Close()

Write compresses data and writes it to the underlying writer.

Write buffers input data until a full frame is available, then compresses and writes the frame. This implements the io.Writer interface.

If an error occurs, the Writer enters an error state and all subsequent Write calls will return the same error.

#### type WriterOption ¶

WriterOption configures a Writer.

#### func WithFrameSize ¶

WithFrameSize sets the frame size for buffered compression.

Larger frame sizes generally provide better compression ratios but use more memory. Smaller frame sizes reduce memory usage but may reduce compression ratio.

The frame size must be between MinFrameSize (4KB) and MaxFrameSize (1MB). If not specified, DefaultFrameSize (64KB) is used.

## Directories ¶

| Path | Synopsis | 
|---|---|
| examples        |  | 
|  | Package main demonstrates using the OpenZL Context API for better performance. | 
| internal        |  | 
|  | Package cgo provides low-level CGO bindings to the OpenZL C library. | 
|  | Package codec provides the interface and registry for OpenZL codecs. | 
|  | Package frame provides Pure Go implementation for reading and writing OpenZL frame format. | 
|  | Package graph provides the compression graph execution engine. | 
|  | Package purgo provides Pure Go OpenZL decompression. | 
| test        |  | 
|  | format_analyzer.go - Analyze OpenZL frame format empirically |
