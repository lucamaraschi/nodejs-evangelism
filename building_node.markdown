# Building Node.js’ core
Before being able to do anything with Node.js’ core, one has to be able to build it.

The [README.md file in node’s GitHub repository](https://github.com/joyent/node/blob/master/README.md) contains all the information needed to do that on all supported platforms. However, it does not describe:

* Build options that have the most impact on the resulting binaries.
* How the build system works.

This chapter covers these two important topics of the build system. It starts by describing in details some of the most important build options.

Then, it dives into the current build system’s design. It describes how each component of Node.js, as highlighted in the introduction, is built and integrated into a single binary.

At the end of this chapter, you should be well equipped to tailor the core of Node.js to your development and production needs. You should also be able to make changes to the build system to:

* Add or make changes to Node.js dependencies.
* Make changes to Node.js’ built-in library.
* Make changes to Node.js’ tests suite.
* Make changes to Node.js’ API documentation.

## Notable build options
As described in [Node.js’ repository’s README file](https://github.com/joyent/node/blob/master/README.md), the build process starts by running [the configure script at the root of the repository](https://github.com/joyent/node/blob/master/configure). This Python script accepts a lot of command line options that can be used to tailor Node.js binary to your needs. In this section, the most important build parameters are presented and split into sections for each category of build parameters.

### Debug builds
Probably one of the most important build parameter is the ability to build Node.js’ core with extended support for debugging its _native_ layer. This allows developers to properly display debugging information when debugging a node binary with a native debugger, such as gdb or lldb.

Code from the JavaScript layer (users' application code and the Node.js' built-in JavaScript library) doesn't need anything from the compilation step to be debuggable. Instead, it relies on a debugger service that runs within V8 and a communication protocol used to communicate between the application to debug and the debugger. More information about this can be found in the "Debugging Node.js' core" chapter.

To do that, simply use the `--debug` command line parameter of the configure script:
```
$ ./configure --debug
```
At the end of the build process, two binaries will be available: a release binary and a debug binary. By default, when not using the `--debug` configure option, only one release binary is built.

The difference between the two binaries is not as obvious as we may think. For instance, it is still possible to display some debugging information with a native debugger from a release binary. However, the debug binary will be the easiest to debug.

Also, the `--debug` parameter doesn't only allow developers to properly display the content of variables or of the call stack with native debuggers, it enables some critical debugging and verification code in Node.js' dependencies, such as V8.

### Dependencies
As described in the introduction, Node.js' native layer depends on several software components that are developed outside of the Node.js project. They are currently V8, libuv, OpenSSL, c-ares, zlib and http-parser. You can display the list of dependencies by listing the directories in the `deps/` directory at the root of the repository.

By default, these dependencies are built from their respective subdirectory in `deps/`. However, for various reasons, Node.js developers may want to use other versions of these dependencies. For instance, some GNU/Linux distributions ship Node.js binary packages where some dependencies have additional bug fixes for their supported platforms.

For each dependency, there are four different parameters that can be used to change how a dependency is included with Node.js:

* `--shared-depname`:
* `--shared-depname-includes`:
* `--shared-depname-libname`:
* `--shared-depname-libpath`:

### Tracing
Node.js currently supports two different tracing frameworks: DTrace on SmartOS, OSX and FreeBSD and ETW on Windows. For every notable even that can happen during the lifecycle of a Node.js program, statics probe are setup in Node.js' core for both tracing frameworks.

For instance, every HTTP request that is received by a Node.js HTTP server [triggers a static probe](https://github.com/joyent/node/blob/v0.12.1-release/lib/_http_server.js#L440-L441).

#### DTrace
To enable the support for DTrace static probes, simply use the `--with-dtrace` command line option of configure.
#### ETW
ETW (for Event Tracing for Windows) is, as its name indicates, a Windows-only tracing framework. It is enabled by default, and if you wish to disable it, you will need to pass the `noetw` argument ot the `vcbuild.bat` batch file.
### Cross-compilation
A Node.js binary that can run on a given hardware architecture and operating system can be built on a machine that runs on another hardware architecture or/and operating system. This is a process known as "cross-compilation".

#### Compiling for another hardware architecture target
To build a binary for another hardware architecture target, simply use the `--dest-cpu` command line switch for `configure`. For instance, to build a binary that can run on ARM from an Intel OSX development setup, the following command can use the following command:
```
./configure --dest-cpu=arm
```
#### Compiling for another operating system target
In the same way the target hardware architecture can be changed, the target operating system for the resulting binary can also be changed with the `--dest-os` configure option. For instance, to build a node binary that can run on GNU/Linux from an OSX development setup, the following command can be used:
```
./configure --dest-os=linux
```
### Internationalization
Node.js comes with support for the [ECMAScript internationalization API](http://www.ecma-international.org/publications/standards/Ecma-402.htm). Many command line parameters can be passed to the `configure` script to tailor this support to the desired level.

The support for the ECMAScript internationalization API depends on two essential but separate components:

* The code that provides the implementation for the API, such as the implementation for string comparison, date or number formatting.
*  The data that is used by the implementation to support each language.

Parameters for the `configure` script can be used to tweak each of these two components.

By default, support for the internationalization API is disabled, however it is enabled in some form or another by the build platform that produces official Node.js releases.

### SSL
SSL support is a key component of Node.js because it has a very significant impact on the security of Node.js applications. SSL is an old protocol used to secure communications over a network, and it has accumulated many security vulnerabilities over the years. In order to satisfy security requirements, one may need to disable some versions of the SSL protocol, or all of them.

Basically, there are three different levels at which SSL can be disabled:

* Only SSLv2 is disabled with `./configure --without-ssl2`.
* Only SSLv3 is disabled with `./configure --without-ssl3`.
* SSLv2 and SSLv3 are both disabled with `./configure --without-ssl`.

## Build system’s design
