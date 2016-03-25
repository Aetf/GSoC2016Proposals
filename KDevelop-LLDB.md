# GSoC 2016 Project: KDevelop: LLDB Support

## Introduction
Enabling kdevelop to use lldb as a debugging solution, which would be especially useful on Max OS X and Windows, where gdb support is rather scarce, and it can also help people that want to switch to lldb on linux (like me!) by adding decent IDE support.

## Project Goals
LLDB support in similar feature-completeness as current GDB support

- Debug session management: begin, kill, restart
- Set breakpoints
    * breakpoint list
    * enable/disable breakpoints
    * conditional breakpoints
- Information inspection
    * Frame Stack: threads, call stack
    * Local variables
    * Symbol info when hovering mouse over the symbol
    * Disassembler
    * Register
- Debug commands
    * next
    * step
    * run to the end of the function
    * jump
    * run lldb commands
- Attach to a running process
- Examine core file
- Remote debug
- Drkonqi support

## Implementation
The whole work can be broken down to three major parts.

- __Communication with LLDB__: Implement a wrapper class that hides all hassles communicating with LLDB and provide a consistent api that is agnostic to specific communication methods, be it inter-processes or direct library link.
- __LLDB Specific Classes__: Implementation of LLDB specific controllers and manager classes. This includes `DebugSession` and friends, `*Controller` classes and config page factories. This part is higher level than the first one, but still remains LLDB specific, thus must be implemented separately from GDB variants.
- __UI and Other__: Hook the UI with the LLDB backend built from the above two parts. This part has the potential to share a large amount of code with GDB. However, currently all UI widgets are bounded to GDB backend. Thus refactors on these classes are needed to split common code out.

#### Relation with current GDB Plugin
Right now the debugger code in kdevplatform only provides very basic facilities to build a debug plugin. Most heavey works are still done in `GDBDebugger::CppDebuggerPlugin`. An investigation into the code shows that there are planty part of code that is debugger backend agnostic, and could be reused by LLDB plugin.

The ideal/desired design would be extract all debugger backend agnostic into kdevplatform and let GDB and LLDB plugin built on top of that. But we risk breaking the existing GDB plugin which already does a good job. Therefore, I plan to first focus on implementation of LLDB plugin with a copy of all needed classes in kdevplatform, and only after the LLDB plugin reaches a rather mature state, shall we begin port GDB to use the new infrastructure.

#### Communication with LLDB
This can be done by expose a interface class with all available lldb commands and internally delegate the command execution to one of the LLDB interface mentioned below. Other parts of the LLDB debugger plugin will rely solely on the interface class to post lldb commands/jobs. This design is similar to what is used in `GDBDebugger::CppDebuggerPlugin`.

###### Possible LLDB Interface to Use
LLDB released LLDB-MI on 2014, and there is another project LLDBMI2 that does the same thing. Along with those, there is the good old C++ API. Further investigation and evaluation is needed to finally determine on which one to use. Here just list a brief description of each choise.

- [LLDB-MI](http://www.codeplay.com/portal/lldb-mi-driver---part-1-introduction)
    * Included in the LLDB source code, which means better support
    * A good separation of input/factory/output
    * Seems only support remote debugging
    * Limited command implemented at the time of the blog written.
- [LLDBMI2](https://github.com/freedib/lldbmi2)
    * Seems to be a simple MI interface to LLDB, which is more lightweight than LLDB-MI
    * Not official but actively maintained
    * Only support local debugging and only support Mac OS X
- [C++ API](http://lldb.llvm.org/cpp_reference/html/index.html)
    * Directly link to LLDB, which should be fast and the API is mature
    * Must find a way to protect ourselves from debugger crashes
    * Write from scratch, can't reuse MI code for GDB

#### Implement LLDB Specific Classes
- DebugSession
    + BreakpointController
    + VariableController
    + FrameStackModel
- ConfigPageFactory
- Launcher and jobs
    + DebugJob

#### Move non GDB Specific Classes to KDevPlatform
- UI
    * ToolViews
        + RegisterView
        + DisassembleWidget
        + GDBOutputWidget
        + MemoryViewDlg
    * Context Menu
    * Dialogs
        + DebugTracingDialog
        + SelectCoreDialog
        + ProcessSelection
- Drkonqi
- DebugSession management

#### PretteyPrinters
Might not be necessary for LLDB

#### Tests
Appropriate unit tests



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
