# GSoC 2016 Project: KDevelop: LLDB Support

## Introduction
Enabling kdevelop to use lldb as a debugging solution, which would be especially useful on Max OS X and Windows, where gdb support is rather scarce, and it can also help people that want to switch to lldb on linux (like me!) by adding decent IDE support.

## Project Goals
LLDB support in similar feature-completeness as current GDB support

- Switch to Debug mode
- Begin debug session
- Set breakpoints
    * breakpoint list
    * enable/disable breakpoints
    * conditional breakpoints
- Information inspection
    * Frame Stack: threads, call stack
    * Local variables
    * Symbol info when hovering mouse over the symbol
- Debug commands
    * next
    * step in
    * run to the end of the function
    * run lldb commands
- Attach to a running process
    * process selection (not sure if the function is implemented bound to gdb)

## Implementation

#### Possible LLDB Interface to Use
LLDB released LLDB-MI on 2014, and there is another project LLDBMI2 that does the same thing. Along with those, there is the C++ API that we can use. So the implementation can be done in three ways

######  [LLDB-MI](http://www.codeplay.com/portal/lldb-mi-driver---part-1-introduction)
Has a good separation of input/factory/output, seems only support remote debugging and have limited command implemented at the time of the blog written.

###### [LLDBMI2](https://github.com/freedib/lldbmi2)
Seems to be a simple MI interface to LLDB, which is more lightweight to LLDB-MI. But only support local debugging and only support Mac OS X

###### C++ API
Of course we can directly link to LLDB, which should be fast and the API is mature. However, we must find a way to protect ourselves when the debugger crashes.

#### Refactor with the GDB Plugin
There may be some code that I can reuse in the GDB plugin. It may be worthwhile to investigate and extract common code into a separate plugin like debugger-base and make GDB and LLDB plugin depends on that.

## Timeline

#### Checkpoints
TBD

#### Availability
I'm staying at the university for summer research, which means a flexible schedule and two project at the same time is the regular workload for a normal semaster. So there shouldn't be a problem.


## About me
My name is Peifeng Yu, a master student at the University of Michigan majoring in Computer Science. I've been using KDE as my major desktop for years and really like the high configuration possibilities it provides. As a programmer, I use KDevelop for most of my projects because of its better support for CMake based projects (and yes, sometimes QtCreator if I'm using qmake ;-) ). I was excited when it first announced the integration with clang compiler. Now here's opportunity to integrate further with llvm toolchain and it would be exciting if I can contribute to it.

As for the skills, I've been programming in C++ for about 5 years and it's my favorate language. And I love Qt, which makes coding a pleasure. I've several little programs written in Qt including my graduate project. Apart from that, I'm always curious about language implementation details and have read a lot about C++ and Qt internals. These knowledge seems unnecessary on the first sight, but they do help me handle tricky situations when the code goes wrong. I have experiences on CMake, too. All my own projects is using CMake as the build system. No need to mention Git, which I use on a daily basis.

#### Contact Information
Email: 7437103@gmail.com / aetf@unlimitedcodeworks.xyz
Website: https://unlimitedcodeworks.xyz
Github:https://github.com/Aetf
Mozillian: https://mozillians.org/en-US/u/Aetf/

[1]: https://bugs.eclipse.org/bugs/show_bug.cgi?id=405670
