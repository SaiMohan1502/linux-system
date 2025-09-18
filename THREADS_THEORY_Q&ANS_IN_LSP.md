## Why are we using pthread_create() instead of clone() for creating threads?
```c
pthread_create() is a high-level, standardized API defined by POSIX for thread creation. It abstracts the complexity of clone() (a Linux-specific, low-level system call) and provides portability, safety, and ease of use.
```
## Why does a stack grow?
```c
A stack grows (usually downward in memory) when a function is called and local variables or return addresses are pushed onto the stack. It allows nested function calls and maintains execution context.
```
## What segments are shared by multiple threads within a process?
```c
- Code (text)
- Data (global/static)
- Heap (malloc)
- File descriptors
- Process ID
Each thread has its own stack and thread-local storage.
```
## Can you fetch the thread entry point return value in your main thread?
```c
Yes, using pthread_join(thread, &retval); where retval gets the return value from the thread function.
```
## What happens when main() function is invoked?
```c
The OS loads the program, initializes stack, heap, and other runtime structures, then jumps to main() function as the entry point of execution.
```
## What happens when the CPU stops executing?
```c
The system enters an idle state or halts. In multiprogramming OS, control is transferred to another process/thread using a context switch.
```
## During a context switch, which instruction is used to copy CPU registers to the PCB (Process Control Block)?
```c
There’s no single instruction; the OS uses assembly-level code with instructions like PUSH, MOV, or saves state via SAVE_CONTEXT macros in kernel code.
```
## How do you create a separate process?
```c
Using fork() or clone() system calls in C.
```
## How does a server create a separate thread?
```c
By calling pthread_create() and passing the function that handles the client or task.
```
## Advantage of thread over process:
```c
- Lightweight (less overhead)
- Faster creation and context switching
- Shared memory space (easy communication)
```
## Advantage of process over thread:
```c
- Better isolation (crash in one process doesn’t affect others)
- Better security and fault tolerance
```
## How do you overcome synchronization issues when multiple threads access global variables?
```c
Use synchronization primitives like mutexes, semaphores, condition variables, or atomic operations to protect critical sections.
```
## How much CPU time is given to user-space thread and kernel-space thread?
```c
- Kernel-level threads are scheduled by the kernel — they get actual CPU time.
- User-level threads require a user-space scheduler and may not use multiple CPUs without kernel support (unless mapped to kernel threads).
```
## Explain POSIX vs System V:
```c
- POSIX is a standard API (pthread, semaphore, shared memory, message queues).
- System V is an older IPC model (System V semaphores, shared memory, message queues).
- POSIX is newer, more portable, and preferred in modern systems.
```
## Points to remember when using mutex locks to protect critical sections:
```c
- Always unlock what you lock (avoid deadlocks)
- Lock as late as possible, unlock as early as possible
- Avoid blocking for long in critical sections
- Avoid nested locks unless necessary
```
## By using pthread_mutex_lock, what are you achieving?
```c
You are ensuring mutual exclusion — only one thread can enter the critical section at a time.
```
## Difference between mutex locks and semaphore:
```c
Feature         | Mutex              | Semaphore
----------------|--------------------|---------------------
Ownership       | Yes (thread-owned) | No ownership
Count-based     | Binary             | Binary or counting
Use case        | Mutual exclusion   | Signaling or sync
Unlock by       | Same thread only   | Any thread

Mutex :- A mutex (mutual exclusion) is a locking mechanism that ensures only one thread can access a shared resource at a time
Semaphore :- A synchronization tool in operating systems used to control access to a shared resource by multiple processes or threads.

```
## Variants of pthread_mutex_lock:
```c
- pthread_mutex_lock() – Blocks until lock is acquired
- pthread_mutex_trylock() – Non-blocking attempt
- pthread_mutex_timedlock() – Waits for a given timeout
```
## How to create a thread?
```c
pthread_t tid;
pthread_create(&tid, NULL, thread_function, NULL);
```
## Explain the compilation of a thread?
```c
Compile using -pthread flag:
gcc thread.c -o thread -pthread
```
## What are the arguments of pthread_create?
```c
int pthread_create( pthread_t *thread, const pthread_attr_t *attr, void *(*start_routine)(void *), void *arg );
```
## Explain the return value of thread:
```c
- Thread function returns a void* via return or pthread_exit()
- Fetched using pthread_join()
```
## Working of pthread_mutex_trylock():
```c
- Tries to lock mutex.
- If already locked, returns EBUSY without blocking.
```
## Application of pthread_mutex_timedlock():
```c
Used when you want to wait for a lock only up to a certain time. Useful in real-time or time-sensitive applications.
```
## What is mutual exclusion?
```c
A concurrency control technique to prevent multiple threads from accessing shared resources at the same time. Achieved using mutexes, semaphores, etc.
```
