# CO Lab Framework - 2026

Welcome to the official Lab Framework for the 2025-2026 edition of the **Computer Organization** course at Vrije Universiteit Amsterdam.

For all information about the course, the assignments, or the Lab in general, please refer to the [Canvas page](https://canvas.vu.nl/courses/81559) or the official [Lab Manual](https://computerscienceeducation.gitbook.io/co-lab-manual).
This document serves as a technical documentation for the Lab Framework itself.

### Table of Contents

1. [Installation](#installation)
   <br>Do this first to set up your development environment.
2. [Using the Framework](#using-the-framework)
   <br>How to use the framework to complete the assignments.
3. [Playground](#playground)
   <br>Where you can experiment with the framework and the Assembly language.
4. [File Preambles](#file-preambles)
   <br>Explanation of the cryptic lines at the start of each Assembly file.
5. [Appendix](#appendix)
   <br>Additional information and troubleshooting.

## Installation

Before being able to use the Lab Framework, you will need to set up your development environment, based on your operating system.
Please refer to the [Lab Manual](https://computerscienceeducation.gitbook.io/co-lab-manual/setup-guides/technical-setup) for detailed instructions on how to do this.

This framework is set up to work with [Visual Studio Code (VSCode)](https://code.visualstudio.com/Download) including its GUI debugger.
Start by opening the framework's root directory in VSCode.

> ⚠️ **Note**: It is important that you do not open VS Code to a parent folder of the framework, as then the setup will not be recognized.

### Installing Extensions

To use the framework with all its features, you will need to install some extensions.
Upon first opening the framework in VS Code, a prompt should appear in the bottom right corner asking whether you want to install the recommended extensions.
If the prompt did not appear or you have accidentally dismissed it, simply follow these steps:

1. Open the _Extensions_ panel (_View_ → _Extensions_ | `⇧⌘X` | `Ctrl + Shift + X`)
2. Type `@recommended` in the search bar.
3. Install all (4) extensions that are listed.

If the aforementioned steps do not work for you, you can also find the list of required extensions in the `extensions.json` file as part of the `.vscode` directory of the framework and manually search and install the extensions.

## Using the Framework

For the first 7 assignments, skeleton files are provided that should serve as the starting point for your implementation.
The files with the extension `.S` are the assembly files that you will need to complete.

> ⚠️ **Note**: It is essential that you **do not delete or modify** the existing code.
> Otherwise your program might no longer work with the automated tests on CodeGrade.

Each file has a list of function names at the top.
These are the library functions that are you allowed to use for that particular assignment.
If you use any additional library functions, your submission will not be accepted during hand-in.
If you believe that a required function is missing from the list, please approach one of the TAs during your Lab session.

### Build system

The framework uses **CMake** as the build system.
The configuration can be found in the `CMakeLists.txt` file in the root directory of the framework.

> The steps to follow are automatically handled for you by VSCode when using the debugging functionality.
> You can also build the project manually, which we will cover below.

To start using CMake and start building your assignments, you first need to initialize the CMake project.
Open up a terminal and navigate to the root directory of the framework, and execute:

```bash
export CC=clang; cmake -B .build
```

Now you can build any assignment by running the following command:

```bash
cmake --build .build --target <assignment_target>
```

Where `<assignment_target>` is the name of the assignment you want to build, from the list: `a1`, `a2`, `a3a-iter`, `a3a-rec`, `a3b`, `a4`, `a5`, `a6`, `a7-encode`, `a7-decode`.

Once you have built an assignment, you can find the executable in the root directory of the framework.
You can run the executable by executing:

```bash
./<assignment_target>
```

Alternatively, the whole process of **building and running** the assignment can be done in one step by using the provided script:

```bash
./build-run <assignment_target> [other arguments required by the assignment]
```

More detailed information on the assembly and linking processes can be found in the `CMakeLists.txt` file or in the [associated chapter of the Lab Manual](https://computerscienceeducation.gitbook.io/co-lab-manual/reference-documentation/building-and-running-programs).

### Docker for address sanitization on macOS (Only on Apple Silicon)

If you are using macOS and have an Apple Silicon chip, you have to use Docker for ASan to work on your executables.
The guide to set up Docker for ASan can be found in the [setup section of the Lab Manual](https://computerscienceeducation.gitbook.io/co-lab-manual/setup-guides/technical-setup/macos#docker-install).

To use Docker, simply run the `co-docker` script the same way you would use the `build-run` script:

```bash
./co-docker <assignment_target> [other arguments required by the assignment]
```

This will start up an Ubuntu 24.04 container with the necessary tools to run your executables, producing address sanitization output.

> **Note**: You only need to use Docker if
>
> 1. You are on macOS and have an Apple Silicon chip and;
> 2. You want to use address sanitization on your executables (**highly recommended**).
>
> Otherwise, you can follow the regular steps in the [Build system](#build-system) section.

### Debugging

> Note that the installed LLDB extension does not work out-of-the-box on some Intel based Macs. If you are on a Mac with an Intel processor, follow the steps given in the [Intel Mac Debugger Fix](#intel-mac-debugger-fix) before continuing here.

To access the VS Code debugger, open the "Run and Debug" panel ("View" → "Run" | ⇧⌘D | Ctrl + Shift + D), select the desired configuration at the top, and click the run button next to the configuration name (F5). If no configurations are shown confirm that the VS Code is opened to the root folder of the framework and a folder names `.vscode` exists. That folder should contain a `launch.json` file.

<p align="center">
  <img src="https://i.imgur.com/89SsolG.png" alt="Debugger targets" width="300"/>
</p>

If you have never used a debugger before or need a quick refresher on how to work with it, the Lab Manual contains an introductory guide on [How to use a Debugger](https://computerscienceeducation.gitbook.io/co-lab-manual/appendix/how-to-use-a-debugger).

> ⚠️ **Warning**: A limitation of using the debugger is its incompatibility with **address sanitization**.
> Therefore, you will need to run your executables outside of the debugger (directly from the terminal) to see the address sanitization output.
>
> We highly recommend running your executables as standalone programs every now and then to check for memory errors.

## Playground

The directory `a0-playground` contains three files:

- the example `sieve` program as discussed in the [Lab Manual](https://computerscienceeducation.gitbook.io/co-lab-manual/reference-documentation/a0-a-running-example)
- a simple _Hello World_ program that prints a message to standard output
- a `sandbox` file that has a structure similar to the assignment skeleton files

You can use and modify any of these programs for experimenting or add further files.

**The `sieve` program is a great way to test your setup and also familiarize yourself with the x86-64 Assembly language.**

## File Preambles

All Assembly (.S) files have a number of lines at the start of the file that may look cryptic at first glance (`#ifdef MACOS ...`).
**Do not modify these lines!**

These _preambles_ are fundamental for compatibility of the written Assembly with both macOS and Linux operating systems as well as with the CodeGrade testing system. You may safely ignore the lines.

For those interested, a short explanation is provided below:

### Function Redirects

Due to historical reasons C function names are prefixed with an underscore on macOS.
So, a call like `printf()` in a C program would be compiled to `call _printf` in Assembly and a `call printf` instruction would give an `Undefined symbols` error during linking.

In order to allow the same programs to run on macOS and on Linux (and with that on CodeGrade), all function calls that are allowed for the particular assignment are redirected based on the operating system.
If the program is preprocessed with the `MACOS` flag (which is automatically included in the assembly commands of the CMake build system when run on macOS) all calls to functions that are allowed as part of the assignment are redirected to the associated function with a prefixed underscore.

These redirects for macOS are not sufficient on their own however.
If someone was to write a program on macOS and already include the needed function prefixes in their function calls, the program would run on their machine but would not compile on CodeGrade.
To avoid the confusion, the preamble also gives the analogue redirects (from `_func` to `func`) for when the `MACOS` flag is not set

### Standalone

Most assignments ask you to implement a `main` routine, so a standalone program, along with a certain subroutine.
As parts of the automated tests, not only your standalone program is tested, but also the required subroutine implementation.

When testing the subroutine implementation, the `main` routine of your program should be ignored (→ no longer be the entry point to the program) such that the `main` routine of the tests can be used.

One option would have been separate the `main` program and subroutine in separate files, such that the tests could then be linked with only the subroutine implementation.
This, however, seemed to needlessly blow up the number of files in the framework.
Thereby, the files include the preprocessor directives `#ifdef STANDALONE ...`, which is used to only make your `main` implementation global when the `STANDALONE` flag is set during the preprocessor run (which again is automatically handled by the CMake targets).

## Appendix

## Intel Mac Debugger Fix

The Code LLDB extension that is needed for the debugger seems to give issues on some macOS devices running on Intel processors.
If you are on an Intel based Mac, you will need to delete a file in the extension folder as explained below to fix the issue:

1. Open a Terminal window and type:\
   `cd ~/.vscode/extensions/vadimcn.vscode-lldb-` \
   but do not press enter yet.

2. Use the Tab key to autocomplete the command for the (newest) version of the extension that you have installed (e.g., 1.10.0) and press enter.

3. Now enter:\
   `rm lldb/bin/debugserver`
