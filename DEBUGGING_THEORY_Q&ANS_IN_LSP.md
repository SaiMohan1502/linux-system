## What is meant by debugging?
```c
Debugging is the process of identifying, analyzing, and fixing bugs (errors) or unexpected behavior in software or hardware.
```
## What are the types of debugging tools?
```c
Debugging tools can be broadly categorized as:
- Software Debuggers: GDB, LLDB, WinDbg
- Memory Debuggers: Valgrind, AddressSanitizer
- Static Analyzers: cppcheck, Clang Static Analyzer
- Tracing Tools: strace, ltrace
- Hardware Debuggers: JTAG, Lauterbach TRACE32
- IDE Debuggers: Eclipse, Visual Studio
```
## Why do we use debugging?
```c
We use debugging to:
- Identify and fix errors in the program
- Understand how the code behaves
- Improve code quality
- Ensure reliability and correctness
```
## What is the difference between hardware debugging and software debugging?
```c
| Feature              | Hardware Debugging                          | Software Debugging                    |
|----------------------|---------------------------------------------|---------------------------------------|
| Scope                | Analyzes embedded systems, firmware         | Analyzes software (applications)      |
| Tools                | JTAG, TRACE32                               | GDB, Valgrind, IDEs                   |
| Level                | Low-level (registers, memory, peripherals)  | High-level (variables, functions)     |
| Usage                | Real-time or embedded system debugging      | Application or OS-level debugging     |
```
## What command is used to add debugging information?
```c
gcc -g filename.c -o output
```
## What are the compilation stages?
```c
1. Preprocessing
2. Compilation
3. Assembly
4. Linking
````
## How do you stop the process after the 1st stage of compilation, and what happens in this stage?
```c
Stage: Preprocessing
Command:
gcc -E file.c -o file.i
What happens: Header files are expanded, macros are replaced.
```
## How do you stop the process after the 2nd stage of compilation, and what happens in this stage?
```c
Stage: Compilation
Command:
gcc -S file.c -o file.s
What happens: C code is translated to assembly code.
```
## How do you stop the process after the 3rd stage of compilation, and what happens in this stage?
```c
Stage: Assembly
Command:
gcc -c file.c -o file.o
What happens: Assembly code is converted into object code (machine code without linking).
```
## How do you generate assembly code from the executable file?
```c
Use objdump:
objdump -d executable > asm_output.txt
```
## What is the use of Strace?
```c
strace traces system calls made by a program. Useful to debug runtime issues like file access, permissions, etc.
```
## What is the use of Ltrace?
```c
ltrace traces library calls (like printf, malloc, etc.) made by a program.
```
## How do you see the source code in GDB?
```c
list
```
## How do you see the assembly code in GDB?
```c
disassemble or disassemble main
```
## How do you check if a particular variable is changing or not every time in GDB?
```c
Use a watchpoint:
watch variable_name
```
## What instruction is given to set breakpoints, and why do we need them?
```c
break <function_name or line_number>
Breakpoints are used to pause execution at specific points to inspect state.
```
## How do you check how many breakpoints a program contains?
```c
info breakpoints
```
## What is meant by a backtrace?
```c
A backtrace shows the call stack — the sequence of function calls that led to the current point. In GDB:
backtrace
```
## What instruction is used to move into a function call in GDB?
 ```c
step
```
## What is meant by memory leakage?
```c
Memory leakage occurs when a program allocates memory but fails to release it. Over time, this wastes system memory.
```
## What tools do we have to check for memory leakage?
```c
- Valgrind
- AddressSanitizer (ASan)
- Electric Fence
- Dr. Memory
```
