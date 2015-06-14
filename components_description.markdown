#Node.js Core Foundations

###V8
is the JavaScript execution engine, which compiles the JavaScript source code just-in-time to native code instead of interpreting it in real time, being the Node.js JavaScript runtime.

###http.parser [link](https://github.com/joyent/http-parser)
This is a parser for HTTP messages written in C. It parses both requests and responses. The parser is designed to be used in performance HTTP applications. It does not make any syscalls nor allocations, it does not buffer data, it can be interrupted at anytime.
**Features:**
- No dependencies
- Handles persistent streams (keep-alive).
- Decodes chunked encoding.
- Upgrade support
- Defends against buffer overflow attacks.

**The parser extracts the following information from HTTP messages:**
- Header fields and values
- Content-Length
- Request method
- Response status code
- Transfer-Encoding
- HTTP version
- Request URL
- Message body
A simplistic zero-copy URL parser is provided as http_parser_parse_url(). Users of this library may wish to use it to parse URLs constructed from consecutive on_url callbacks.


###c-ares
is the C library responsible of asynchronous DNS resolving. [link](https://github.com/bagder/c-ares)

###libuv
is the C library responsible for the asynchronous handling of I/O operations. [link](https://github.com/libuv/libuv)

###zlib
is the library responsible for the compression.

###OpenSSL
is the C library responsible of basic cryptographic functions, exposing a multitude of utilities.

##Project Structure

- benchmark: contains all the scripts to perform performance benchmark tests
- deps: contains all dependencies libraries
- doc: contains the documentation (in markdown format) used in the node.js.org website
- lib: contains all the JavaScript files that are composing the Node.js Core Library
- src: contains all the C++ files that are composing the Node.js Core Bindings
- test: contains all the tests
- tools: contains a collection of utilities and tools used for general purposes

The project contains also a list of files located in the root of the project. These are:

- android-configure: bash file to set configurations needed to build for Android devices on arm platform
- BSDmakefile: alerts on BSD systems to use gmake instead of make
- common.gypi: Chromium configuration file containing generic building configurations for all the supported platforms
- configure: Python script
- Makefile: make script which exposes ALL possible tasks
- node.gyp:
- vcbuild.bat: Windows’ users makefile
