## Explain the concept of process creation in operating systems.
```c
-> WAYS OF CREATING PROCESS:-
	
	1. From the executable file, process can be created by using command “ ./a.out “ where “ a.out “ is the executable file obtained after compilation.
 
	2. By using “ Fork() “ as a system call:-

		System call:- This is used in user space to send the request in sub-system in kernel space.
					 Every system call has an equivalent call in kernel space which starts with “ sys_ “. Fork() system call can be used to create a child process and the process from where the fork() is invoked is called “ parent process ”.
					 Once we call fork(), fork() control immediately jumps to kernel space or invoke an equivalent function in kernel space starting with “ sys_fork() “, which is actually sending request from process management sub-system to kernel space process sub-system.

```
## What is the purpose of the wait() system call in process management?
```c
-> The wait() system call is used by a parent process to wait for the termination of its child process. Its main purposes are:-

		1. Prevent zombie processes -> Frees child’s resources after termination.

		2. Synchronize parent and child -> Ensures parent waits for child to finish.

		3. Get child's exit status Useful -> for decision-making in the parent.
 
##. Describe the role of the exec() family of functions in process management 

-> The exec() family of functions is used to replace the current process image with a new process image. It allows a process (usually after a fork()) to run a different program in the same process space.

	Work flow:-
		Parent
  	  	  └── fork()
         		└── Child
                 		└── exec() -> Loads new program

```
## Differentiate between the fork() and exec() system calls.
```c
---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
|Feature		|			fork()						|			exec() 							|
|-----------------------|-----------------------------------------------------------------------|-------------------------------------------------------------------------------|
|Definition		| Creates a new process (child) by duplicating the calling process	| Replaces the current process image with a new program 			|
|			|									|										|
|Process Creation	| Yes, creates a new child process					| No new process, replaces current process 					|
|			|									|										|
|Return Value		| 0 (child), PID (parent), -1 (error)					| -1 (on error), no return on success 						|
|			|									|										|
|Memory Sharing		| Child gets a copy of the parent’s memory (Copy-on-Write)		| Current process memory is replaced entirely 					|
|			|									|										|
|Execution Continuity	| Both parent and child continue after fork()				| The original process is replaced, execution starts from new program’s entry 	|
|			|									|										|
|Use Case		| When a process needs to create a duplicate of itself			| When a process needs to load and run a different program 			|
|			|									|										|
|System Resources	| Allocates a new PID and process entry in process table		| No new PID; reuses current PID and process table entry 			|
|			|									|										|
|Example Function	| pid_t pid = fork();							| execl("/bin/ls", "ls", NULL); 						|
---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
```
## How does the vfork() system call differ from fork()?
```c
->  1. Process Creation

	Both create a new child process.

    2. Memory Handling

	fork(): Child gets a copy of parent’s memory (Copy-on-Write).

	vfork(): Child shares the parent’s memory temporarily.

    3. Performance

	fork() is slower due to memory copying.

	vfork() is faster as no memory is copied.

     4. Execution Behavior

	fork(): Parent and child run independently.

	vfork(): Parent is suspended until child calls exec() or _exit().

     5. Use Case

	fork(): General-purpose, safe for all situations.

	vfork(): Use only if child calls exec() or _exit() immediately.

     6. Risk

	vfork() is dangerous if child modifies memory before exec().

     7. Return Value

	Same for both: 0 in child, child PID in parent, -1 on failure.

     8. Safety

	fork(): Safe

	vfork(): Not safe for general use because as this Shares memory/stack, Parent got suspended & Requires strict usage etc.
```
## Discuss the significance of the getpid() and getppid() system calls.
```c
-> Significance of getpid() and getppid() :–

	1. getpid() – Get Process ID

		Returns the PID of the current process.

		Each process has a unique PID in the system.

		Used for:- Logging or debugging process activity, Sending signals to itself (e.g., kill(getpid(), SIGTERM)) & Creating unique filenames or resources [exm:-/tmp/file_<pid>].

	2. getppid() – Get Parent Process ID

		Returns the PID of the parent process.

		Helps identify which process created the current one.

		Used for:- Debugging process hierarchy, Handling orphans (detecting if parent is dead) & Monitoring parent-child relationships.

	3. Common Use Case

		Often used after fork() to Identify child and parent roles & Print or log process relationships.
```
## Explain the concept of process termination in UNIX-like operating systems.
```c
->	1. Process termination occurs when a process finishes or crashes.

 	2. Exit status is returned to the parent via exit() or return.

 	3. wait()/waitpid() is used by the parent to collect exit status.

 	4. Zombie: When a process exits but the parent hasn’t read its exit status, it becomes a zombie. It still occupies a PID and entry in the process table. It is fixed by the parent calling wait() to reap the child process.

 	5. Orphan: If a parent process terminates before the child, the child becomes an orphan. Orphans process are automatically adopted by init (PID 1) or systemd

 	6. Forced termination: A process can be terminated by Signals like SIGTERM, SIGINT, SIGKILL & also by using the kill command or kill() system call
```
## Describe the process hierarchy in UNIX-like operating systems.
```c
-> 	The hierarchy starts with init or systemd (PID 1).

	Processes create child processes using fork().

	Every process has a parent PID (PPID).

	The structure forms a tree (parent → child).

	Orphans are adopted by init/systemd.

	Zombies remain in the process table until reaped.

	Hierarchy can be viewed with commands like pstree or ps -ejH.
```
## What is the purpose of the exit() function in C programming?
```c
-> The exit() function is used to “ terminate a program explicitly “ & “ return a status code to the operating system “.
	Purpose of exit():-
		
		1. Immediately stops the program, where it appears in the code ( as return is used to only exit from current function ).

		2. If exit(0) -> indicates successful execution.
   exit(1) -> generates “ error “ or “ abnormal termination ”.

		3. Before terminating whole program, exit() flushes output buffers, closes all the open files until where it mentioned & calls functions registered with “ atexit()”.

```
## Explain how the execve() system call works and provide a code example.
```c
-> execve() is used to execute a new program, replacing the current process image with a new one. It does not return on success — the calling process becomes the new program.

	Key Behavior:-

	•	It replaces the currently running program (code, data, stack) with the program specified.
	
	•	The PID remains the same, but the memory image is replaced.

	•	It's typically used after fork() to run a new program in the child process.
	
	Note:-
	
	•	If execve() succeeds, no code after it in the child runs.
	
	•	You must pass absolute path for execve() (unlike execvp() which searches PATH).
	
	•	Use NULL to terminate both argv[] and envp[].

	Example:-
	
	#include <stdio.h>
	#include<stdlib.h>
	#include <unistd.h>
	int main() {
    	// Define the arguments: program name, options, and NULL-terminator
    	char *args[] = {"/bin/ls", "-l", NULL};
    	// Use the current environment
    	char *env[] = {NULL};
    	// Replace current process with /bin/ls
    	if (execve("/bin/ls", args, env) == -1) {
        	perror("execve failed");
        	exit(EXIT_FAILURE);
    	}
    	return 0; // This will not be reached if execve succeeds
	}
```
## Discuss the role of the fork() system call in implementing multitasking
```c
-> The fork() system call enables multitasking by creating a new child process that runs concurrently with the parent. Each process gets a unique ID and initially shares memory using Copy-On-Write, making it efficient. This allows multiple tasks to run in parallel, forming the basis for multitasking in Unix-like systems. It's widely used in shells, servers, and background task management.

```
##. How does the exec() system call replace the current process image with a new one?
```c
-> Step-by-Step: How exec() Works

	1. Called by a Process

		Usually after fork() to let the child run a different program.

	2. Loads New Program

		exec() loads the executable file (like /bin/ls) from disk into the process's memory.

	3. Destroys Old Image

		The old code, data, heap, and stack of the calling process are discarded.

	4. Sets Up New Image

		Allocates new stack, heap, and data sections & Initializes new code segment, environment, and arguments.

	5. Starts Execution

		Execution begins at the entry point of the new program (usually main()).

		The PID remains the same, but everything else is changed.

	6. Does Not Return

		If successful, exec() does not return to the original code.

		If it fails (e.g., file not found), it returns -1.
 ```
## Explain the concept of process scheduling in operating systems.
```c
-> Process scheduling is the method by which the operating system (OS) selects which process to run next on the CPU when multiple processes are ready to execute.

. The scheduler picks processes from the ready queue using algorithms like FCFS, Round Robin, Priority, and SJF, aiming to optimize metrics such as CPU utilization, turnaround time, and response time.

Scheduling can be preemptive, where a process can be interrupted, or non-preemptive, where a process runs until it finishes or blocks. Overall, process scheduling is essential for system performance, responsiveness, and managing multiple processes efficiently.
	
```
## Describe the role of the clone() system call in process management
```c
-> 	1. clone() creates new processes or threads with customizable resource sharing like Memory (CLONE_VM), File descriptors (CLONE_FILES), Signal handlers (CLONE_SIGHAND), Stack (CLONE_FS), Parent/child relationship (CLONE_PARENT) & Thread group (CLONE_THREAD).

	2. More powerful and flexible than fork() or vfork().

	3. Used internally to implement threads, containers, and namespaces.

	4. Requires manual stack allocation for the child.

	5. Supports various flags to control sharing (memory, signals, file descriptors, etc.).
```
## Discuss the significance of the setuid() and setgid() system calls in process management.
```c
-> The setuid() and setgid() system calls in Unix-like operating systems are used to change the user ID (UID) and group ID (GID) of a running process. These are critical for controlling process privileges and access rights.

	1. Purpose of setuid(uid):-

		Changes the User ID of the calling process.

		Allows a process to drop or gain privileges (e.g., root to user or vice versa).

		Often used by programs that start with root privileges but should perform tasks as a regular user (for security)

	2. Purpose of setgid(gid):-

		Changes the Group ID of the calling process.

		Similar to setuid(), but applies to group permissions.

	Note:-
		Only a process with root privileges can change its UID/GID to another user. 

		Once privileges are dropped (from root to normal user), they cannot be regained unless the process uses special techniques (like using saved IDs/executing another binary).
```
## Explain the concept of process groups and their significance in UNIX-like operating systems
```c
-> A process group is a collection of one or more processes that are logically grouped together and share a common process group ID (PGID)

	 Common Signals via Process Group:-

		SIGINT (Ctrl+C) — sent to foreground process group.

		SIGTSTP (Ctrl+Z) — stops all processes in the foreground group.

		SIGKILL — can kill all processes in a group using kill(-pgid, SIGKILL);
	
```
## Discuss the role of the execle() function in the exec() family of calls.
```c
-> The execle() function is a member of the exec() family used in UNIX-like systems to replace the current process image with a new program, while allowing you to specify a custom environment.

	Purpose:- 

		Executes a new program with a custom environment.

		Replaces the calling process without returning (if successful).

	It takes a list of arguments and a separate environment array.

	Does not search PATH — needs a full executable path.

	Replaces the calling process; does not return on success.

	Useful for applications needing custom, isolated environments.
```
## Describe the purpose of the nice() system call in process scheduling.
```c
-> The nice() system call is used in Linux operating systems to influence the scheduling priority of a process. It helps manage CPU time sharing among processes by making a process "nicer" (i.e., giving it lower priority) so others can get more CPU time.

	1. Changes the "niceness" value of a process.

		A higher nice value → lower priority (less CPU time).

		A lower nice value → higher priority (more CPU time).

	2. Default nice value:- 0

	3. Range: -20 (highest priority) to +19 (lowest priority)

	4. Helps in fair CPU time distribution.

	5. Only root can decrease the nice value (raise priority).

	6. Useful for background tasks and system load balancing.

	7. Only superuser (root) can decrease the nice value (i.e., increase priority).
```
## Explain the role of the getpid() and getppid() functions in process management.
```c
-> The getpid() and getppid() functions are used in UNIX-like operating systems to retrieve process-related information. They are essential for tracking process identity and relationships in process management.

	1. getpid() – Get Process ID:- 

		Returns: The PID (Process ID) of the calling process.

		Purpose:

			Uniquely identify the current process.

			Useful for debugging, logging, or sending signals.

			Helps in creating unique file names or shared memory keys.



 	2. getppid() – Get Parent Process ID:-

		Returns: The PID of the parent of the calling process.

		Purpose:

			Helps a child process know who created it.

			Useful for handling orphaned processes.

			Helps track process hierarchy in process trees

	Why They're Important :-

		Used in multi-process programs to manage process relationships.

		Help in signal handling, inter-process communication (IPC), and process control.

		Widely used in systems programming, daemons, and shell utilitie
```
## Discuss the difference between the fork() and clone() system calls
```c
| Feature           | `fork()`                                  | `clone()`                                                        |
| ----------------- | ----------------------------------------- | ---------------------------------------------------------------- |
| Purpose           | Creates a new process (copy of parent)    | Creates a new process or thread                             	   |
| Resource Sharing  | Child gets a separate memory, stack, etc. | Child can share memory, files, signals (controlled by flags)     |
| Flexibility       | Less flexible                             | Highly flexible with many `CLONE_` flags                         |
| Used For          | Normal process creation                   | Threads, containers, namespaces                                  |
| Stack Requirement | Uses same stack automatically             | Requires manual stack allocation                                 |
| Syntax            | `pid_t fork(void)`                        | `int clone(fn, stack, flags, arg, ...)`                          |
| System            | POSIX standard, portable                  | Linux-specific, not portable                                     |
| Underlying Use    | Simpler programs, daemons                 | Used in `pthread_create()`, Docker, etc.                         |
```
## Explain the concept of process states in UNIX-like operating systems.
```c
-> Processes go through different states like running, sleeping, stopped, or zombie. These states are managed by the kernel scheduler.

| State | Name                       | Description                                                                     |
| ----- | -------------------------- | ------------------------------------------------------------------------------- |
| R 	| Running                    | Process is actively executing or ready to run (on the CPU or in the run queue). |
| S 	| Sleeping (Interruptible)   | Waiting for an event (like I/O) that can be interrupted by signals.             |
| D 	| Sleeping (Uninterruptible) | Waiting for a resource (like disk I/O) and cannot be interrupted.               |
| T 	| Stopped/Traced             | Process has been stopped (e.g., via `SIGSTOP`) or is being debugged.            |
| Z 	| Zombie                     | Process has terminated, but the parent hasn’t read its exit status yet.         |
| X 	| Dead (rare)                | Process is being removed from the system (not normally visible).                |
```
## Describe the purpose of the chroot() system call and provide an example.agement.
```c
-> The chroot() system call changes the apparent root directory (/) for the current process and its children, creating a restricted environment called a "chroot jail."

	1. After calling chroot("/newroot"), the process sees /newroot as /.

	2. It cannot access any files outside /newroot.

	3. You must also chdir("/") afterward to make / the current directory.

	4.  Important Note:-

		chroot() does not guarantee full security like containers or VMs.

		It's often used in combination with dropping privileges using setuid().

		Can be escaped if not configured correctly (e.g., bash or shells inside). 

	5. Purpose of using chroot():- Security isolation, Testing, Software packaging & System recovery
```
## Discuss the role of the execv() function in the exec() family of call
```c
-> 1. Role of execv() in the exec() Family of Calls:-

		The execv() function is part of the exec() family used in UNIX-like systems to replace the current process image with a new program, passing arguments as an array.

   2. Purpose of execv():-

		Executes a new program from a specified path.

		Replaces the current process (same PID).

		Accepts arguments as an array (argv[]).

		Does not search the PATH environment variable (you must provide the full path).

    3. When to Use execv():-

		When you want to run another program with arguments.

		When your arguments are already stored in an array.

		For scripts, system utilities, or spawning controlled subprocesses after fork().
```
## Explain the significance of process identifiers (PIDs) in process management.
```c
-> In UNIX-like operating systems, a Process Identifier (PID) is a unique non-negative integer assigned by the kernel to each active process. PIDs play a crucial role in managing and controlling processes.
	
	1.Importances of PID's:-

		| 	Featur           | 				Description                                      |
		| ---------------------- | ----------------------------------------------------------------------------- |
		| Uniqueness             | Each running process gets a unique PID to distinguish it from others.         |
		| Tracking Processes     | Used by system tools (`ps`, `top`, `kill`) to monitor and control processes.  |
		| Signal Handling        | System calls like `kill()` use the PID to send signals to specific processes. |
		| Parent-Child Links 	 | PIDs help track parent-child relationships using `getpid()` and `getppid()`.  |
		| Scheduling & Debugging | The kernel uses PIDs to manage scheduling, priorities, and debugging.	 |

	2. PID Range:-

		Typically between 1 and 32768 (can be higher on modern systems).

		PID 1 is usually the init or systemd process — the ancestor of all processes.
```
## Discuss the concept of orphan processes and how they are handled in UNIX-like operating systems.
```c
-> 	1. An orphan process is a process whose parent has terminated before it did. The system ensures that these processes do not run unmanaged, by reassigning them to a special parent

	2. Normally, every process has a parent (tracked via getppid()).

	3. When the parent exits before the child, the child becomes an orphan.

	4. The kernel reassigns the orphaned process to init (PID 1) or systemd (in modern systems).

	5. Why the Kernel Adopts Orphans:-
T	
		To clean up orphaned processes after they finish (prevent zombies).

		To ensure the system maintains process tree integrity.

		The init/systemd process periodically calls wait() to collect exit status of adopted child processes.

	6. This way, the system ensures orphaned processes are adopted and cleaned up properly.
```
## Describe the concept of process priority and how it is managed in operating systems.
```c
-> Process priority determines which process gets CPU time first. Higher priority = more chance to run; lower = more waiting.

	Types of Priority:-

		Static Priority: Fixed during execution (used in real-time).

		Dynamic Priority: Changes based on process behavior (used in general-purpose OS).	
 
	How OS Uses Priority:-

		Real-time systems use strict priority (e.g., RT scheduling classes).

		General-purpose OS may dynamically adjust priority based on:

			CPU usage

			I/O wait

			Interactivity

```
## Explain the purpose of the fork() system call in creating copy-on-write (COW) processes.
```c
-> The fork() system call is used to create a new process by duplicating the calling process. In modern UNIX-like systems, it is optimized using a technique called Copy-On-Write (COW) to improve performance and memory efficiency

	1. What is Copy-On-Write (COW)?

		Copy-On-Write is a memory optimization technique where:

		The parent and child initially share the same physical memory pages.

		If either process modifies a shared page, the OS makes a separate copy for that process.

		This avoids unnecessary copying unless writes occur.

	2. Purpose of fork() with COW:-

		Efficient Process Creation:-

			Instead of copying entire memory, only page tables are duplicated.

			Actual copying is delayed until write access.

	 	Faster Performance

			fork() becomes much faster, especially for large processes.

		Lower Memory Usage

			Multiple processes can share memory safely until modification.

		Used in exec-based workflows

			fork() is often immediately followed by exec(), so memory is not copied at all unless needed.

	3.  How It Works Internally:-

		fork() creates a child process.

		Both parent and child share memory pages marked read-only.

		On write attempt, a page fault occurs.

		The kernel copies the page to a new location for the writing process.
```
## Discuss the role of the execvp() function in searching for executable files.

-> The execvp() function is part of the exec family of functions in Unix-like operating systems, which replace the current process image with a new process image (i.e., a new program starts running in place of the old one, using the same process ID).

	Role: PATH Searching.

		Unlike execv(), which requires the full path to the executable, execvp() automatically searches for the file in the directories listed in the $PATH environment variable.

		$PATH is a colon-separated list of directories (e.g., /bin:/usr/bin:/usr/local/bin).

		execvp() will look for the executable in each directory in $PATH in order until it finds a match.


```
## Explain the concept of process context switching and its impact on system performance.

-> Context switching is the process where the operating system (OS) switches the CPU from executing one process to another. This allows multitasking — letting multiple processes share a single CPU.

	1. Impact on System Performance:-

	| Impact Area       | 						Description                                                                                                      |
	| ----------------- | ---------------------------------------------------------------------------------------------------------------- |
	| Overhead          | Saving/restoring context takes time (CPU cycles), during which no "real" work is done.                           |
	| Cache Misses      | When switching processes, CPU cache, TLB (Translation Lookaside Buffer), etc., may no longer hold relevant data. |
	| Frequent Switches | Too many switches (e.g., from short time slices or I/O-heavy workloads) can degrade performance.                 |
	| Latency           | Real-time or interactive processes may suffer delays if context switching is not well-optimized.                 |


## Discuss the significance of the execve() function in process creation and execution. 
```c
-> execve() is the core function used by all other exec* functions. It replaces the current process image with a new program.

	Syntax:- int execve(const char *pathname, char *const argv[], char *const envp[]);

	Significance:-

		Executes a new program directly with specified arguments and environment.

		Does not return on success.

		All other exec* functions wrap around it.
```
## Explain the difference between process creation using fork() and pthread_create().
```c
| Feature          | `fork()`               | `pthread_create()`               |
| ---------------- | ---------------------- | -------------------------------- |
| Creates          | New **process**        | New **thread**                   |
| Address space    | **Separate**, with COW | **Shared**                       |
| PID              | Unique PID             | Same PID, unique thread ID (TID) |
| File descriptors | Copied (COW)           | Shared                           |
| Signal handlers  | Copied                 | Shared                           |
| Communication    | Requires IPC           | Shared memory                    |
| Overhead         | Higher                 | Lower                            |
```
## Discuss the significance of the execvp() function in searching for executables in the PATH environment variable
```c
-> execvp() replaces the current process with a new one, and searches the executable in the directories listed in $PATH.

  Syntax:- int execvp(const char *file, char *const argv[]);

  Significance:-

	Automates executable lookup similar to how a shell works.

	Convenient for launching standard commands without full path.

	Falls back to execve() once the correct path is found	

```
## Describe the role of the fork() system call in implementing the shell's job control.
```c
-> In a shell, fork() is used to:-

	Create a new child process for every user command.

	The parent (shell) uses the child’s PID to manage the job.

    Job control:-

	Shell assigns process groups.

	Uses setpgid(), tcsetpgrp() to manage foreground/background jobs.

	Tracks job status via waitpid().

    Allows:-

	Background execution (&)

	Foreground control (fg, bg)

	Signal forwarding (e.g., Ctrl+Z, Ctrl+C)
```
## Explain the purpose of the execlp() function and provide an example.
```c
-> execlp() is used to execute a program, searching for it in $PATH, and passing arguments as a list (variadic).

   Syntax:- int execlp(const char *file, const char *arg, ..., NULL);

   Purpose:- Similar to execvp() but with a list instead of an array.
```
## Discuss the significance of the setpgid() system call in managing process groups.
```c
-> setpgid() sets the process group ID of a process, helping group multiple processes for job control and signal handling.

   Syntax:- int setpgid(pid_t pid, pid_t pgid);
   
   Significance:-

	Used by shells to assign child processes into a group.

	Enables killpg() to send signals to the entire group.

	Essential for foreground/background job control in terminals.
```
## Explain the concept of process priority inheritance and its importance in real-time systems.
```c
-> Priority inheritance is a technique where a lower-priority task inherits the higher priority of a task waiting on a shared resource.

	Why it's important in RTOS:-

		Prevents priority inversion, where a high-priority task is blocked by a low-priority task holding a resource.

		Ensures predictable and timely execution of real-time tasks.

	Example:-

		Task A (low) locks mutex.

		Task B (high) waits on the mutex.

		OS boosts Task A's priority until it unlocks the mutex.
```
## Describe the role of the fork() system call in copy-on-write (COW) mechanism and its benefits
```c
-> fork() creates a child process by duplicating the parent’s memory space, but uses Copy-On-Write (COW) optimization.

	COW behavior:-

		Parent and child share pages marked read-only.

		If one modifies a page, a private copy is created.

	Benefits:-

		Efficient memory use

		Faster fork() execution

		Ideal for forking processes followed by exec()
```
## Explain the concept of process scheduling policies such as FIFO, Round Robin, and Priority scheduling.
```c
-> 
```
## Discuss the role of the clone() system call in creating threads in Linux
```c
-> The clone() system call is a low-level, flexible system call used to create processes or threads by sharing specific resources between the parent and child.

   Role in thread creation:-

	Threads are created by calling clone() with flags like CLONE_VM, CLONE_FS, CLONE_FILES, CLONE_SIGHAND, and CLONE_THREAD.

	These flags allow the new thread to share memory space, file descriptors, and signal handlers with the parent process.

   Used internally by: pthread_create() in the glibc library to implement POSIX threads.
```
## Explain the concept of process swapping and its role in memory management.
```c
-> Process swapping is a memory management technique where entire processes are moved between main memory and secondary storage (swap space) to free up RAM.

   Role:-

	Maximizes memory utilization by removing inactive processes from RAM.

	Enables multitasking on systems with limited memory.

	Used less frequently today due to virtual memory and paging, but still important in low-memory or embedded systems.

   Drawback:- Swapping is slow due to disk I/O, leading to increased latency (thrashing if overused).

```
## Discuss the difference between the fork() and pthread_create() functions in terms of process/thread creation.
```c
| Feature          | `fork()`                        | `pthread_create()`                   |
| ---------------- | ------------------------------- | ------------------------------------ |
| Creates          | A **new process**               | A **new thread** within same process |
| Memory           | New copy of address space (COW) | Shares memory space with parent      |
| PID              | Returns a new PID               | Shares same PID, new TID (thread ID) |
| File descriptors | Duplicated                      | Shared                               |
| Communication    | Requires IPC mechanisms         | Can use shared memory                |
| Overhead         | Higher                          | Lower                                |
| Use case         | Multi-processing                | Multi-threading                      |
 
```
## Describe the purpose of the prctl() system call in process management
```c
-> management.
The prctl() system call allows a process to get or set various process-specific behaviors and attributes.

   Examples of usage:-

	Set process name (PR_SET_NAME)

	Prevent core dumps on termination (PR_SET_DUMPABLE)

	Enable parent-death signal (PR_SET_PDEATHSIG)

	Control seccomp filters for sandboxing

   Syntax:-
		int prctl(int option, unsigned long arg2, ...);

	Useful for security, debugging, and fine-grained control of process behavio

```
## Explain the concept of process preemption and its impact on system responsiveness
```c
-> Process preemption is the ability of the OS to interrupt a running process and assign the CPU to another process, typically a higher-priority one.

	Impact:-

		Enables real-time responsiveness

		Ensures fair CPU allocation

		Helps implement priority scheduling

	Used in: Preemptive multitasking OS like Linux.

	Trade-off: Frequent preemption increases context-switch overhead

## Discuss the role of the exec functions in handling file descriptors during process execution.
```c
-> exec() functions replace the current process image with a new program.

	File Descriptor Handling:-

		By default, open file descriptors remain open after exec().

		To close them automatically, use the FD_CLOEXEC flag via fcntl():

				fcntl(fd, F_SETFD, FD_CLOEXEC);

	Use case:- Prevent leaking sensitive or unneeded file descriptors to new processes.

```
## Explain the significance of process priorities and how they affect scheduling decisions.
```c
-> Process priority determines how much CPU time a process receives.

	Higher priority → scheduled more often.

	Nice values (Linux): Range from -20 (highest priority) to 19 (lowest).

	Real-time priorities: Use fixed scheduling with policies like SCHED_FIFO.

   Affects scheduling decisions:

	Real-time processes preempt others.

	Lower-priority processes may starve without fair scheduling.

	Commands: nice, renice, setpriority()
```
## Describe the process of process termination and the various ways it can occur.
```c
-> A process can terminate in several ways:-

	Normal exit:

		exit() or returning from main()

	Error exit:

		abort() or exit with non-zero status

	Signal termination:

		Killed by a signal (e.g., SIGKILL, SIGSEGV)

	Killed by other process:

		Via kill() or sigqueue()

    After termination:-

	The process becomes a zombie until the parent reads the exit status using wait() or waitpid().
```
## Discuss the role of the exit status in process termination and how it can be retrieved by the parent process
```c
-> When a process terminates, it returns an exit status to its parent (via exit()).

   Role:-

	Indicates success or failure (0 = success).

	Used in shell scripting and process monitoring.

    Retrieval by parent:-

		int status;
		waitpid(pid, &status, 0);
		if (WIFEXITED(status))
    		printf("Exited with code %d\n", WEXITSTATUS(status));

```
## Explain the concept of process suspension and resumption using signals.
```c
-> Suspension: A process can be suspended (paused) using signals like SIGSTOP.

   Resumption: It can be resumed using SIGCONT.

	Examples:-
		kill -STOP <pid>   # suspend
		kill -CONT <pid>   # resume
   NOTE:- Useful in job control (e.g., Ctrl+Z suspends, fg resumes), and debugging.

```
## Discuss the role of process scheduling algorithms in determining the order of execution among processes.
```c
-> Scheduling algorithms decide which process runs next on the CPU.

   Common algorithms:-

	Round Robin: Fair time-slicing; used in general-purpose OS.

	Priority Scheduling: High-priority tasks first; can cause starvation.

	Multilevel Queue: Different queues for different priority classes.

	Real-Time Scheduling: SCHED_FIFO, SCHED_RR, SCHED_DEADLINE.

    Role:-

	Balances responsiveness, throughput, and fairness

	Crucial in real-time systems, multi-user systems, and servers
```
## Explain the concept of process migration and its relevance in distributed systems.
```c
-> Process migration refers to the act of moving a process from one machine or node to another within a distributed system, either during execution or between execution phases.

  Relevance in distributed systems:

	Load balancing: Move processes from overloaded nodes to underutilized ones.

	Fault tolerance: Migrate processes away from failing hardware.

	Data locality: Move processes closer to the data they need to access.

	Administrative management: For example, during system maintenance or upgrades.

  Challenges:-

	Maintaining open file descriptors, socket connections, memory state.

	Synchronizing the state between nodes without data loss or corruption
```
# Describe the role of process identifiers (PIDs) in process management and their uniqueness within the system
```c
-> A Process ID (PID) is a unique, non-negative integer assigned by the kernel to each running process.

	Roles:-

		Identification: Allows the OS and users to refer to and manage processes.

		System calls: Used with many APIs like kill(pid, sig), waitpid(pid, ...), etc.

		Uniqueness: Each PID is unique at a point in time; however, it can be reused after a process terminates.

        PID limits: Usually governed by /proc/sys/kernel/pid_max
```
## Explain the concept of process tracing and its importance in debugging and monitoring.
```c
-> Process tracing is a mechanism that allows one process (typically a debugger) to observe and control another process.

	Importance:-

		Debugging: Tools like gdb use tracing to read/write process memory, monitor syscalls, and step through code.

		System call tracing: strace traces syscalls made by a process.

		Security monitoring: Intrusion detection tools can trace process behavior.

        System call used: ptrace() is the primary syscall used for implementing tracing
```
## Discuss the role of the fork() system call in creating copy-on-write (COW) processes and its impact on memory usage.
```c
->  Instead of immediately copying all memory pages, parent and child share pages marked as read-only.

    When either process writes to a shared page, the OS copies it (hence "copy-on-write").

		Impact on memory:-

			Saves memory and speeds up fork() execution.

			Commonly used before exec() to launch new programs efficiently.
```
## Describe the concept of process control blocks (PCBs) and their role in process management.
```c
->    PID, PPID

	Process state (running, sleeping, zombie, etc.)

	CPU registers, program counter

	Memory management info (page tables)

	File descriptors

	Scheduling and priority info

	Signal handling state

     Role: The PCB is essential for context switching, scheduling, and resource tracking.
```
## Explain the concept of process scheduling classes and their significance in real-time operating systems.
```c
-> Scheduling classes are policies used by the OS to schedule processes. In Linux, they include:

		SCHED_OTHER – Normal time-sharing (default).

		SCHED_FIFO – First-in-first-out real-time scheduling.

		SCHED_RR – Round-robin real-time scheduling.

		SCHED_DEADLINE – Task must meet a specific deadline.

	Significance in RTOS:-

		Real-time systems require deterministic behavior.

		Prioritize time-critical tasks over background or user tasks.

		Prevents starvation by ensuring predictable scheduling.
```
## Discuss the role of the vfork() system call in process creation and its differences from fork().
```c
-> vfork() creates a new process without duplicating the parent's address space.

	Key differences from fork():-

		Parent is suspended until the child calls exec() or _exit().

		No copy-on-write: The child runs in the same address space as the parent.

		Faster and lighter, but dangerous if the child modifies memory.

	Use case: When the child is going to call exec() immediately.
```
## Describe the purpose of the sigaction() system call in handling signals in processes.
```c
-> sigaction() allows setting custom signal handlers with more flexibility and reliability than signal().

  syntax:-
	struct sigaction 
      {
        void (*sa_handler)(int);
        sigset_t sa_mask;
        int sa_flags;
      };
  Purpose:-

	Specify a handler function for a signal (e.g., SIGINT).

	Block other signals during execution of the handler.

	Set flags like SA_RESTART, SA_SIGINFO, etc.

	More robust and portable than legacy signal().
```
## Discuss the role of the sigprocmask() system call in managing signal masks for processes.
```c
-> sigprocmask() is used to block or unblock signals from being delivered to a process.

   Prototype:-

	int sigprocmask(int how, const sigset_t *set, sigset_t *oldset);

   Roles:-

	Temporarily block signals during critical sections.

	Combine with sigaction() for safe signal handling.

	Prevents signal handlers from interrupting unsafe code regions (e.g., non-reentrant functions).

    Modes:-

	SIG_BLOCK – Add signals to current block list.

	SIG_UNBLOCK – Remove signals from block list.

	SIG_SETMASK – Replace block list.
```
## Describe the purpose of the setrlimit() system call in setting resource limits for processes.
```c
-> The setrlimit() system call is used to set resource limits for a process to prevent it from overusing system resources.

Examples of resource limits:-

	1. Maximum file size (RLIMIT_FSIZE)

	2. Maximum number of open files (RLIMIT_NOFILE)

	3. Maximum stack size (RLIMIT_STACK)

	4. Maximum CPU time (RLIMIT_CPU)
```
## Discuss the concept of process groups and their importance in job control and signal propagation
```c
-> A process group is a collection of related processes that can be managed together, especially in job control (e.g., foreground/background jobs in a shell).

Importance:-

	Signal Propagation: Sending a signal to a group (killpg()) affects all members.

	Job Control: The shell groups processes (e.g., pipelines) into process groups for easier control.

 	Example:- In a shell pipeline: cat file | grep text | sort → all are in one process group
```
## Explain the role of the prlimit() system call in setting resource limits for processes.
```c
-> prlimit() combines the functionality of setrlimit() and getrlimit(), and allows setting limits for any process (not just the current one)

	In C :- int prlimit(pid_t pid, int resource, const struct rlimit *new_limit, struct rlimit *old_limit);
 
	where, pid:- Set to 0 for current process, Can get and set limits in one call
```
## Discuss the concept of process scheduling policies in multi-core systems and their implications.
```c
-> Scheduling policies determine how and when processes are assigned to CPUs.

Common Policies:-

	1. SCHED_FIFO – Real-time, first in first out

	2. SCHED_RR – Real-time, round-robin

	3. SCHED_OTHER – Default, time-shared policy

	4. SCHED_BATCH – For long-running background tasks

	5. SCHED_IDLE – Lowest priority

In Multi-core Systems:-

	Policies work per-core, but can be optimized using CPU affinity

	Helps in load balancing, reducing latency and improving throughput
```
## Describe the role of the setpriority() system call in adjusting process priorities
```c
-> This call is used to adjust the priority (also called nice value) of a process, affecting how much CPU time it gets. 

	prio ranges from -20 (high priority) to 19 (low priority).

	 Affects only SCHED_OTHER policy processes
```
## Explain the concept of process scheduling latency and its impact on system responsiveness
```c
-> Scheduling latency is the delay between a process being ready and actually running on the CPU.

 Impacts:-

	High latency → poor responsiveness (bad for real-time apps)

	Important in audio, video, and interactive systems

Controlled using:-

	Real-time policies (SCHED_FIFO, SCHED_RR)

	Preemption and priority tuning
```
## Discuss the role of the prlimit64() system call in setting resource limits for processes with 64-bit address space.
```c
-> prlimit64() is similar to prlimit() but supports 64-bit resource limits, useful for systems with large memory or file sizes.

	Used in 64-bit systems

	Ensures processes can handle large address spaces or limit
```
## Describe the purpose of the sched_getaffinity() system call in querying the CPU affinity of a process.
```c
-> Used to query which CPUs a process is allowed to run on.

   syntax:-

	int sched_getaffinity(pid_t pid, size_t cpusetsize, cpu_set_t *mask);

   Helps in analyzing load balancing

   Can be paired with sched_setaffinity() to optimize CPU usage
```
## Discuss the concept of process checkpointing and its relevance in fault tolerance and process migration
```c
-> Checkpointing is the process of saving the complete state of a running process so it can be resumed later.

Uses:-

	Fault Tolerance: Resume from checkpoint after crash

	Process Migration: Move a process to another machine

Tools:-

	CRIU (Checkpoint/Restore In Userspace) in Linux
```
## Explain the significance of the /proc filesystem in providing information about processes in Linux.
```c
-> /proc is a virtual filesystem that provides real-time info about system and processes.

Example paths:-
	
	/proc/[pid]/status – Info about a specific process

	/proc/cpuinfo – CPU details

	/proc/meminfo – Memory info

Benefits:-
	
	Useful for monitoring, debugging, and system management

	No disk I/O – data is generated dynamically
```
## Describe the purpose of the sched_setaffinity() system call in setting CPU affinity for processes.
```c
-> sched_setaffinity() is used to bind a process to specific CPU(s).

	This improves performance by avoiding cache misses or unnecessary context switching.

  syntax :- int sched_setaffinity(pid_t pid, size_t cpusetsize, const cpu_set_t *mask);

	pid: Process ID (0 for current)

	mask: Tells which CPUs the process can run on
```
## Discuss the concept of process re-parenting and its implications in process management.
```c
-> When a parent process terminates before its child, the child becomes an orphan.

   The OS re-parents the orphaned process to init (PID 1) or systemd.

	Implications:-
		Ensures that zombie processes are reaped properly.

		Maintains process hierarchy for proper resource cleanup.
```
## what is process and what is the difference between process and program?
```c
-> A process is an active execution of a program. It is a running instance of a program, with its own:-

	1. Memory (code, data, stack, heap)

	2. CPU registers

	3. Program counter (PC)

	4. Process ID (PID)

| Feature        | **Program**                              | **Process**                               |
| -------------- | ---------------------------------------- | ----------------------------------------- |
| **Definition** | A **passive** set of instructions (file) | An **active** execution of a program      |
| **State**      | Stored on disk (e.g., `.out`, `.exe`)    | Exists in memory and uses CPU             |
| **Instance**   | One program can be run many times        | Each run creates a new process (with PID) |
| **Lifespan**   | Permanent until deleted                  | Temporary, ends when execution finishes   |
| **Examples**   | A C compiled file `app.out`              | Running `./app.out` creates a process     |
```
## what type of infomation pcb contains ?
```c
-> PCB contains all info the OS needs to manage a process:-

	Process ID (PID)

	Process state (Running, Ready, etc.)

	Program counter

	CPU registers

	Memory mappings

	Open file descriptors, signal disposition table, page table

	Parent PID

	Scheduling info (priority, etc.)
```
## Explain about memory segements ?
```c
| Segment   | Purpose                               |
| --------- | ------------------------------------- |
| Text 	    | Code (instructions), read-only        |
| Data      | Global/static variables (initialized) |
| BSS       | Uninitialized global/static vars      |
| Heap      | Dynamic memory (`malloc()`)           |
| Stack     | Function calls, local variables       |
```
## when fork invoked what are things will happen ?
```c
-> 	1. A new process (child) is created.

	2. Child gets:-

		Copy of parent’s memory space (via Copy-On-Write)

		Same code and file descriptors

		Different PID and PPID

	3. Both processes continue from the same point, with different return values from fork().
```
## what is the difference between ps and top?
```
| Feature             | `ps` (Process Status)                    | `top` (Table of Processes)                     |
| ------------------- | ---------------------------------------- | ---------------------------------------------- |
| **Output Type**     | **Snapshot** of processes at that moment | **Real-time**, continuously updating view      |
| **Interface**       | One-time output (static)                 | Interactive (you can sort, search, kill, etc.) |
| **Usage Style**     | Used for **scripting and logs**          | Used for **live monitoring and diagnostics**   |
| **CPU Usage Info**  | Not real-time                            | Real-time CPU usage shown per process          |
| **Default Display** | Only shows your processes                | Shows **all running processes**                |
```
## what are the types of processes we have explain each process breifly?
```c
-> 	1. Foreground Process: Runs in terminal, interacts with user

	2. Background Process: Runs in background (&)

	3. Daemon Process: Long-running, detached from terminal (e.g., sshd)

	4. Orphan Process: Parent dies before child

	5. Zombie Process: Process finished, but parent hasn’t read its exit status
```
## what is the ppid of orphan process?
```c
-> 	When a process becomes orphan, the PPID becomes 1 (init or systemd).

	OS re-parents it automatically to ensure cleanup.
```
## what is the difference between exit vs return?
```c
| Feature               | `return`                            | `exit()`                                                  |
| --------------------- | ----------------------------------- | --------------------------------------------------------- |
| **Definition**        | Ends execution of a **function**    | Ends execution of the **entire process**                  |
| **Scope**             | Only exits the **current function** | Exits the **whole program**, no matter where it is called |
| **Used In**           | Functions (like `main`, `foo`)      | Anywhere in the program                                   |
| **Return Type**       | Returns a value to the **caller**   | Returns a value to the **operating system**               |
| **Performs Cleanup?** | No automatic cleanup                | Yes, cleans up buffers and open files                     |
| **Header File**       | Not needed                          | Requires `#include <stdlib.h>`                            |
```
## In parent process can you access to the exit code of return value of child process (or) in parent process how do you block to get the id of child?
```c
-> Yes, the parent can access the child’s exit code using the wait() or waitpid() system call.

	Example:-
		int status;
		pid_t pid = wait(&status); // Blocks parent until child exits
		if (WIFEXITED(status)) 
		{
    		printf("Child exited with code %d\n", WEXITSTATUS(status));
		}
	
	1. wait() blocks the parent until a child process exits.

	2. It also returns the PID of the terminated child.
```
## what is the difference between Zombie and Orphan process?
```c
| Feature         | Zombie Process                                                                                   | Orphan Process                                                      |
| --------------- | ------------------------------------------------------------------------------------------------ | ------------------------------------------------------------------- |
| Definition      | A process that has **finished execution**, but its **exit status is not yet read by the parent** | A process whose **parent has terminated** while it is still running |
| Exists in table | Yes (until `wait()` is called by parent)                                                         | Yes (adopted by `init` or systemd)                                  |
| Harm            | Can cause resource leakage if too many                                                           | No harm (handled by OS)                                             |
```
## what is the use of fork() 
```c
-> 	1. fork() is used to create a new process.

	2. It creates a child process that is an exact copy of the parent.

	3. Both processes run concurrently and can run different code using the return value of fork().
```
## define system call name some blocking system calls?
```c
->  Definition:
		A system call is a programmatic way for user-space programs to request services from the operating system kernel.

Examples of blocking system calls:
	
	read() – blocks if data not available

	wait() – blocks until child terminates

	accept() – blocks until a client connects

	recv() – blocks until data is received

	sleep() – blocks for time
```
## why do not we run program with in harddisk (or) why do we copy program to RAM for execution?
```c
->  Hard Disk:-

	1. Very slow (high latency and low bandwidth)

	2. Not directly accessible by CPU

 RAM:-
	1. Fast access time

	2. CPU can only execute instructions from RAM

	3. OS loads executable into RAM and CPU fetches instructions from RAM

	Hence, for speed and execution, programs are copied from disk to RAM.
```
## how do you copy data from RAM to CPU registers ?
```c
-> bash:- mov eax, [mem_address]

   c :- int x = *ptr;

	1. The CPU issues a load instruction

	2. The MMU translates the virtual address

	3. The data from RAM is loaded into CPU registers
```
## how do you copy data from CPU registers to RAM ?
```c
-> bash :- mov [mem_address], eax

   c :- *ptr = x;

	1. The value in the register is written to RAM.

	2. The CPU controls this using address/data buses and memory controller.
```
## explain about paging technique ?
```c
-> Paging is a memory management scheme that:

	Divides virtual memory into pages

	Divides physical memory into frames (of same size)

	Maintains a page table to map virtual pages to physical frames

 Benefits:-

	Solves external fragmentation

	Allows non-contiguous memory allocation

	Supports virtual memory
```
## why the no of pages are not fixed ?
```c
-> The number of pages depends on:

	The size of virtual memory (VM)

	The page size (PS)

     In c :- No of pages = Virtual Memory Size / Page Size

		So, if either page size or virtual memory size changes (which can differ per system), the number of pages also changes.
```
## explain about shared memory why we require shared memory?
```c
-> Shared memory is an Inter-Process Communication (IPC) mechanism that allows multiple processes to access the same region of memory.

    Why do we need it?
	1. It is the fastest IPC mechanism because no kernel involvement after setup.

	2. Allows data exchange between processes without copying.

	3. Useful when large data needs to be shared (e.g., image, sensor data, buffers).
```
##. How does multiple process can share data ?
```c
-> Multiple processes can share data using IPC methods like:

	1. Shared Memory (fastest)

	2. Pipes (unidirectional)

	3. Message Queues

	4. Sockets

	5. Files

In shared memory:

	1. One process creates the shared segment.

	2. Others attach to it using an identifier.

	3. All processes read/write directly to the same memory.
```
## explain the scenario when pages of process-1 and pages of process-2 are copied into same frames (or) explain the scenario where pages of multiple process are sharing the same physical frames(or) Can the page table of multiple process point to same  physical frames?
```c
-> Yes, this is possible and common in scenarios like:

	1. Shared Libraries: Code segments (read-only) like libc can be mapped to same physical frames across processes.

	2. Shared Memory: When processes intentionally map a shared memory region.

	3. Copy-On-Write: After fork(), both parent and child initially point to same physical pages (read-only). Actual copying happens only on write.

Thus this allows efficient memory usage.

```
## How child process uses the memory segment of parent process?
```c
-> After a fork():

	1. Child gets a copy of the parent's address space.

	2. Initially, both share the same physical memory pages using Copy-On-Write.

	3. Only when one writes, a new physical page is created for that process.

  This saves memory and improves performance.
```
## how the parent process and child process keep track of which instruction they are going to execute ?
```c
-> Each process has its own:

	1. Program Counter (PC) – stores the next instruction to execute.

	2. After fork(), both parent and child continue from same instruction, but:

		 Return value of fork() tells whether it’s parent or child.

		 Based on that, they may execute different code paths.
```
## how parent an child process executes different blocks of code after fork()
```c
-> pid_t pid = fork();

if (pid == 0) {
    // Child process block
} else {
    // Parent process block
}
 
	fork() returns 0 to child and child PID to parent.

		Using this return value, we split logic between child and parent.
```
## explain write operation to pages shared to parent and child process?
```c
-> After fork(), both processes share physical pages using Copy-On-Write (COW).

 When a write happens:
	The kernel copies the page to a new physical location.

	The writing process gets its own private copy.

	Page table is updated for that process.

	This ensures memory isolation and efficiency.
```
## explain about adress translation?
```c
-> Address translation converts a virtual address (used by programs) into a physical address (used by hardware).

Steps:-
	1. CPU generates virtual address.

	2. MMU (Memory Management Unit) uses page tables to translate it.

	3. The virtual address is broken into:

		Page number (used to index page table)

		Offset (added to physical frame base address)

Virtual Address --> [Page Number | Offset]
                   ↓
              Page Table Lookup
                   ↓
    Physical Frame + Offset = Physical Address
```
## what adress cpu places an adress bus?
```c
-> The CPU places a physical address on the address bus.

After translating a virtual address (used by programs) into a physical address using the MMU (Memory Management Unit), the CPU sends the physical address to memory via the address bus.

## when virtual adress are generated ?

-> Virtual addresses are generated when a program accesses memory (e.g., reading/writing a variable or calling a function).

The CPU generates virtual addresses during instruction fetch or data access in user space.
```
## when physical adresss are generated from virtual adress ?
```c
-> Physical addresses are generated during address translation, which is handled by the MMU.

This translation happens just before the memory access, during execution of a memory instruction like load, store, or fetch.
```
## what is exec () family of calls and what is the need of exac() family of calls and what are those?
```c
-> The exec() family of functions replaces the current process image with a new executable program.

-> To run a new program in the same process, usually after using fork() to create a new child process.

| Function    | Arguments Format          | Searches PATH |
| ----------- | ------------------------- | ------------- |
| `execl()`   | List of arguments         |  No          |
| `execlp()`  | List of arguments         |  Yes         |
| `execle()`  | List of arguments + env   |  No          |
| `execv()`   | Array of arguments        |  No          |
| `execvp()`  | Array of arguments        |  Yes         |
| `execvpe()` | Array + env + PATH search |  Yes         |

```
## most of the system call why they return error?
```c
-> System calls return errors when:

Resources are unavailable (e.g., file not found, memory full)

Invalid parameters are passed

Permissions are denied

Hardware or OS limitations are reached

They return -1 (or other specific value), and set the global variable errno to indicate the specific error.
```
## what is the difference between Execl() and Execv()?
```c
-> System calls interact with hardware and kernel resources, which can fail.
 Hence, they return:
   0 or positive on success
   -1 on error and set errno
Example reasons:
   Invalid parameters
    Permission denied
    Resource not available
    Memory or file limits exceeded
This is why error checking is critical after each system call.
```
## What is the difference between Execlp() and Execvp()
```c
-> 
| Feature                | `execlp()`                                                  | `execvp()`                                            |
| ---------------------- | ----------------------------------------------------------- | ----------------------------------------------------- |
| **Function Signature** | `int execlp(const char *file, const char *arg, ..., NULL);` | `int execvp(const char *file, char *const argv[]);`   |
| **Arguments Format**   | Arguments passed **individually**                           | Arguments passed as **array of strings**              |
| **NULL Termination**   | Last argument **must be NULL**                              | Array must be **NULL-terminated**                     |
| **Use Case**           | When number of arguments is known at compile-time           | When arguments are stored in an array (e.g., dynamic) |

```







