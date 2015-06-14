`
# Introduction to Node.js.
## What is Node.js?
## Node.js use cases
## Current node.js' design
Describes layers that compose Node.js: JavaScript built-in library, V8, custom C++ bindings, libuv, other dependencies like OpenSSL, c-ares, http-parser and zlib.
Describes how these layers fit together. Use diagrams as much as possible.
# Working with Node.js' core
## Building Node.js' core
### Build system's design
Describes how the build system is designed. Covers, among other things, how the configure script invokes gyp. Covers how dependencies are built. Covers how Node.js's dependencies are linked statically to produce a binary that can be placed anywhere on the system.

### Building Node.js' for release and development
Describes how to build release and debug versions.

### Other significant build parameters
Describe ways to significantly alter the binaries that are built, covers:
* --with-intl.
* --without-snapshot.
* --dest-cpu.
* SSL options.

## Debugging Node.js' core
Covers how to debug node.js' core.
### Using a "traditional" source-level debugger
Covers how to debug Node.js on all supported platforms with source-level debuggers. Describes the shortcomings of this approach
### Using NODE_DEBUG
### Using dynamic tracing
#### Dtrace
#### ETW
####Whatever is available on Linux
### Using mdb/postmortem debugging

# Anatomy of a basic Node.js program
Describes how a very simple Node.js program, without any I/O, works. Covers how Node.js starts, how it compiles and runs the code in the program's file.

# Fundamentals of how V8 is embedded within Node.js
Covers the basics of how Node.js and V8 communicate:
* How Node.js adds functions and objects within a given V8 context.
* How Node.js can call JavaScript functions, and how JavaScript functions can call Node.js native functions.
* Describe how native objects are wrapped within JavaScript objects. Cover the inheritance hierarchy with BaseObject, AsyncWrap, HandleWrap and ReqWrap classes.

# Fundamentals of how libuv is embedded within Node.js
Covers the basics of how Node.js uses libuv. Describes what the libuv events loop is, where/when/how Node.js starts/runs the libuv events loop, and what impact it has on a Node.js program. For instance, it describes what it means to keep the event loop active (the program keeps running), what keeps the loop active (handles and requests).
## Introduction to ReqWrap and HandleWrap
* Ref and UnRef methods.
* getActiveHandles() and getActiveRequests().

# Modules
## Introduction
Presents use cases of modules and simple code examples to set the context.
## Binary add-ons
## Common.js specification and differences with actual Node.js implementation
## The special case for the "main" module
## Implementation
### How NativeModule bootstraps the module.js implementation (which is a module!)
### How modules are compiled and run
Covers how modules are wrapped within a function (__filename, __dirname, module, ...) {}.
### Binary add-ons
Mentions how binary add-ons are loaded. Also covers how node-gyp works to build binary add-ons upon installation.
### Debugging module loading
Mentions NODE_DEBUG=module to debug module loading.

# Event emitters
## Introduction
Describes how an event emitter is really just an implementation of the Observer/Observable pattern, and how events are not emitted asynchronously. Describes why most events are emitted within a process.nextTick call.
## What is an event emitter in Node.js?
Describes what objects are event emitters in Node.js.
## Issues with event emitters
Lack of encapsulation: any user-land code can remove an event listener that had been setup by Node.js' core.

# Streams
## What is a stream in Node.js?
Describes what objects are streams in Node.js
## Differences between streams1, streams2 and streams3
## Issues with streams
Sequence of events not well defined in most cases.

# File I/O
Covers how file I/O is implemented.
## The "requests" concept
### ReqWrap class
### libuv requests and threadpool, libuv filesystem requests
### Synchronous and asynchronous requests
## Putting it all together
Walkthrough of how a simple Node.js application that writes to a file works under the hood, using the information presented in this chapter.

# Network I/O

# Network names resolution
## System names resolution
## Actual names resolution (using c-ares and DNS protocol)
## Comparison between the two methods

#Scheduling arbitrary async operations
Covers setTimeout, setInterval, process.nextTick and setImmediate.
## setTimeout and setInterval
### Timers
#### Internal vs User timers
## process.nextTick
## setImmediate

# HTTP and HTTP/S
Covers http-parser, client requests, server responses, etc.
