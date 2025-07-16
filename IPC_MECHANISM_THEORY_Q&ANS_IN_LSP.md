## What is meant by an IPC Mechanism?

-> IPC (Inter-Process Communication) mechanism refers to the methods or techniques that allow multiple processes to:

	1. Communicate with each other, and

	2. Coordinate or synchronize their actions.

	3. These processes could be running on the same system or different systems over a network.

## Why we use IPC Mechanism?

-> Inter-Process Communication (IPC) mechanisms are necessary because, in modern operating systems, processes run independently and do not share memory by default. Therefore, IPC is essential for building reliable, concurrent, and multi-process systems by enabling communication, data exchange, and synchronization between processes.

## What are the types of IPC Mechanism's?

-> Types of IPC Mechanism:- 
	
	1. Communication-Oriented IPC:

		Pipes

		Named Pipes (FIFOs)

		Message Queues

		Sockets

		Signals

	2. Data Sharing-Oriented IPC:

		Shared Memory

		Memory-Mapped Files

	3. Synchronization-Oriented IPC:-

		Semaphores

		Mutexes

		Condition Variables (used in threads)

## What is meant by “unicast” and “multicast” IPC?

-> In Inter-Process Communication (IPC), unicast and multicast refer to how messages are delivered from one process to others.

	1. Unicast IPC:-

		(i). Definition:- A one-to-one communication between a sender and a single receiver, which means the message is sent from one process to exactly one target process.

		(ii). Example:- A client sends a request to a specific server.

		(iii). IPC Mechanisms That Support Unicast:- Pipes, Sockets (TCP), Signals, Message Queues

	2. Multicast IPC:-

		(i). Definition:- A one-to-many communication where one process sends a message to multiple processes simultaneously or selectivel.

		(ii). Example:- A process notifies multiple subscribed processes about an event (e.g., using publish-subscribe model).

		(iii). Ways to Implement Multicast IPC:- Message Queues with Multiple Readers, Shared Memory + Semaphores (with multiple consumers)

## What is meant by PIPES?

-> A pipe is a unidirectional communication channel that allows one process to send data to another process, where the output of one becomes the input of the other.

## What is meant by Blocking Calls?

-> A blocking call suspends the execution of the calling process or thread until the operation completes.

## What are the types of Blocking Calls?

-> The types of Blocking Calls:-

	1. I/O Blocking Calls:- These occur when a process waits for input or output operations to complete like " read(), write(), recv(), fgets()/scanf(), send() etc."

	2. Process & Thread Synchronization Blocking:- These blocks execution until another process/thread or condition is met like " wait() / waitpid(), pthread_join(), sleep() / usleep() etc. ".
	3. File and Device I/O Blocking :- Waits for data from disk, device, or external input like " open(), read() etc."

## What are the different types of I/O Calls?

-> Types of I/O Calls are:-

	1. Basic I/O Calls:-

		(i). These are also called as Universal I/O Calls like open(), close(), read(), write(), ioctl(), fnctl() etc.

		(ii). These are system calls.
		
		(iii). These are operated on File descriptor table (fd table).

		(iv). No buffering in userspace, but sometimes buffering happens in the kernel space i.e., when data sent to the kernel, kernel memory may not be available instantaneously. So, it has to be hold in some registers.

	2. Standard I/O Calls:-

		(i). These re also called Buffered I/O Calls like fopen(), fclose(), fread(), fwrite(), fputs(), fgets(), fprintf(), fscanf() etc.

		(ii). These are Library Calls.

		(iii). These are operted on file pointers

		(iv). Buffering in user space takes place

		(v). Used to access only normal files.

	3. Memory Mapping Calls:-

		(i). Memory mapping is a mechanism by which the contents of a file or device are directly mapped into the virtual memory of a process, enabling file I/O through memory access (reads/writes) instead of traditional system calls.

		(ii). Types of Meory Mapping are of File mamping technique & Anomyous Mapping

		(iii). Used to access files.

## What are the I/O calls we are used in IPC Mechanisms?

-> 
	| 	IPC Type        | 		I/O Calls Involved                       |
	| --------------------- | ------------------------------------------------------ |
	| Pipes                 | `pipe()`, `read()`, `write()`, `close()`               |
	| Named Pipes           | `mkfifo()`, `open()`, `read()`, `write()`              |
	| Message Queues        | `msgget()`, `msgsnd()`, `msgrcv()`, `msgctl()`         |
	| Shared Memory         | `shmget()`, `shmat()`, `shmdt()`, `shmctl()`           |
	| Sockets               | `socket()`, `bind()`, `connect()`, `read()`, `write()` |
	| Signals               | `kill()`, `signal()`, `sigaction()`, `raise()`         |
	| Memory-Mapped I/O     | `mmap()`, `munmap()`                                   |

## What are the Blocking Calls used in IPC?

-> read, write, accept, connect, send/sendto, recv/recvfrom, sem_wait, wait/waitpid, sleep etc.
 
## What is meant by Named Pipes?

-> A Named Pipe (also called a FIFO — First In First Out) is a special type of file used for inter-process communication (IPC) that allows unrelated processes to communicate through a file name in the filesystem.

## Where is the FIFO Object created?

-> A FIFO (Named Pipe) is created as a special file in the filesystem — specifically, a device file of type p (pipe).

## What is the call used to create a FIFO Object?

-> we use the mkfifo() s a system call to create a FIFO object.

## What are the Blocking Calls used in Named Pipes?

-> open(O_RDONLY), open(O_WRONLY), read() & write()

## Why read system calls acts as a blocking call?

-> The read() system call is blocking by default to ensure that: read() waits until data is available to be read — it blocks the calling process to prevent it from continuing with incomplete or invalid data.

## Difference between the Named Pipes and Pipes?
----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
|				PIPES					|					NAMED PIPES (FIFO) 							|
|-----------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------|
|	A Pipe is a unidirectional (one-way) communication channel	|  A Named Pipe, also called  FIFO, is a special file that allows communication between unrelated processes	|
|	used between related processes (typically parent-child)		|  using a name in the filesystem.										|
|	to transfer data.						| 														|
|-----------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------|
|	Unidirectional or Half-Duplexed					|  Unidirectional or Half-Duplexed										|
|-----------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------|
|	Accessed using Basic I/O Calls					|  Accessed using Basic I/O Calls										|
|-----------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------|
|	Pipes are created or simulated using kernel object		|  Named pipes are created as special files in the system.							|
|	(i.e., file object, inode object, etc.)				|														|
|-----------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------|
|	Once the process is terminated, the pipe’s			|  Once a process terminates, the named pipes (special files) are not destroyed. They exist in file system.     |
|	corresponding objects are completely destroyed.			|  They exist in the file system.										|
|-----------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------|
|	It is created using pipe().					|  It is created using mkfifo().										|
|-----------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------|
|	read() acts as a blocking call.					|  open(), read() act as blocking calls		 								|
|-----------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------|

## What is return value of read system call?

->
	| **Return Value** | **Meaning**                              |
	| ---------------- | ---------------------------------------- |
	| `> 0`            | Number of bytes successfully read        |
	| `0`              | End-of-File (EOF) reached                |
	| `-1`             | Error occurred (check `errno` for cause) |

## What is meant by message queue?

-> A message queue is an IPC mechanism that allows processes to exchange data as discrete messages stored in a queue.

## Why we use message queues?

-> We use message queues for:- 

	1. Enables asynchronous communication

	2. Decouples sender and receiver

	3. Supports priority/message type-based retrieval

	4. Ensures reliable delivery

	5. Offers structured communication (better than raw pipes)

## What is difference between Named Pipe and Message Queue?

| Aspect              | 			Named Pipe (FIFO)                                           | 				Message Queue                           |                                         
--------------------- | ----------------------------------------------------------------------------------- | ------------------------------------------------------------------|
|  Definition         | A FIFO special file that allows 1-way or 2-way communication between processes.     | A data structure that allows processes to exchange discrete msg.
             
| Identification      | Identified by a filename in the filesystem [mkfifo() or mknod()].                   | Identified by a key or ID (using msgget(), msgsnd(), msgrcv()].              |

| Data Format         | Transfers byte stream (like a file or pipe).                                        | Transfers messages (with a structure: message type + data).                        

| Communication Style | Unstructured stream; reader must interpret data boundaries.                         | Structured, each message is distinct and tagged with a type.                       

| Blocking Behavior   | Blocking by default if one end is not ready (e.g., reading without a writer).       | Can be blocking or non-blocking based on flags.                                    

| Persistence         | FIFO file exists in the filesystem but data is lost when read.                      | Messages are kept in the queue until explicitly removed (read).                        

| Access Control      | Controlled via filesystem permissions.                                              | Controlled via System V IPC permissions.                                           

| Used For            | Simple, fast communication between related or unrelated processes on same system.   | Complex and asynchronous communication, suitable for multiple readers/writers. 

| Kernel Buffering    | Uses kernel buffer like a regular pipe.                                             | Kernel maintains a message queue with limits on size and number of msgs.           

| Creation Function   | `mkfifo()` or shell command `mkfifo filename`                                       | `msgget()`, `msgsnd()`, `msgrcv()`, `msgctl()`                                         

## What is the system call used to create the message queue?

-> To create the message queue we use " msgget() "

## Where was the message queue created?

-> The message queue is created inside the kernel (in kernel space memory) — not in the filesystem.
 
## What is meant by Shared Memory?

-> Shared Memory is an Inter-Process Communication (IPC) mechanism that allows multiple processes to access the same memory region. It is the fastest IPC method because data doesn’t need to be copied between processes — they share it directly.

## Why we use Shared Memory?

-> We use Shared Memory because:-

	1. Extremely Fast

	2. Efficient Resource Sharing

	3. Low Overhead

	4. Allows Communication Between Unrelated Processes

	5. Suitable for Producer-Consumer Patterns

## Difference between Shared Memory and Message Queues?

| **Aspect**             | **Shared Memory**                                                          | **Message Queue**                                                       |
| ---------------------- | -------------------------------------------------------------------------- | ----------------------------------------------------------------------- |
| **Definition**         | A memory segment that is shared between processes.                         | A queue structure used to exchange discrete messages between processes. |
| **Speed**              | **Very fast** – processes access the same memory directly.                 | Slower than shared memory – messages pass through the kernel.           |
| **Data Access**        | Direct – processes **read/write** to same memory location.                 | Indirect – processes **send/receive** messages via system calls.        |
| **Data Format**        | Unstructured – raw memory, you define the structure.                       | Structured – messages have type + data.                                 |
| **Kernel Involvement** | Only for **setup and teardown** (access is user-level).                    | Every send/receive involves the **kernel**.                             |
| **Synchronization**    | **Not built-in** – must use **semaphores/mutexes** to coordinate access.   | Messages are automatically queued and synchronized.                     |
| **Best Use Case**      | For **large data sharing**, like images, buffers, or arrays.               | For **small, structured messages** or control information.              |
| **Persistence**        | Data stays until cleared or process detaches.                              | Messages stay in queue until read or removed.                           |
| **Ease of Use**        |  Complex – you handle memory layout and locking.                           | Easier – send/receive interface is simple.                              |
| **Security**           | Must ensure processes don’t overwrite each other’s data.                   | Messages are safely queued and managed.                                 |
| **Example Use Case**   | Shared cache, video frame buffers, simulation data.                        | Task coordination, status updates, small data packets.                  |

## What is use of stat command?

-> The stat command is used to display detailed information about a file or directory.

## What is the use of semctl command?

-> The semctl system call is used to control or manage System V semaphore sets.

## How do we destroy the shared memory object?

-> To destroy a shared memory object we use " shmctl(shmid, IPC_RMID, NULL); ". As this tells the kernel to delete the memory segment when it's no longer in use.

## What is meant by Semaphores?

-> A semaphore is a synchronization tool used to control access to a shared resource by multiple processes or threads in a concurrent system.

## Why we use semaphores?

-> Semaphores are used to:-

	1. Safely share resources between processes

	2. Avoid data corruption

	3. Ensure correct execution order

	4. Synchronize process behavior

## What is meant by Synchronization?

-> Synchronization in system programming means coordinating the execution of multiple processes or threads so that they work together correctly without conflict when accessing shared resources.

## What is meant by Asynchronization?

-> Asynchronization (or asynchronous execution) means that tasks run independently, without waiting for each other to complete.

## Why we use mutex locks?

-> We use mutex locks to protect shared resources in multithreaded programs and ensure safe, consistent, and error-free execution.

## What is the difference between mutex locks and semaphores?

| **Feature**           | **Mutex Lock**                                            | **Semaphore**                                               |
| --------------------- | --------------------------------------------------------- | ----------------------------------------------------------- |
| **Meaning**           | **Mutual exclusion** — only one thread at a time          | Counting mechanism to allow **one or more** threads         |
| **Type**              | Binary only (0 or 1)                                      | Can be **binary or counting** (e.g., 0 to N)                |
| **Used For**          | Protecting **critical sections** (shared resources)       | Managing **access to limited resources** (e.g., 3 printers) |
| **Ownership**         | Has **ownership** — only the thread that locks can unlock | **No ownership** — any thread can wait/post                 |
| **Blocking Behavior** | Thread **blocks** if mutex is already locked              | Waits if count is 0; continues if count > 0                 |
| **Unlocking**         | Only the **locking thread** can unlock                    | Any thread can signal/post                                  |
| **Deadlock Risk**     | Higher if not used carefully                              | Also possible, especially with multiple semaphores          |
| **Used In**           | Mostly for **threads (pthread\_mutex)**                   | For **both threads and processes** (`sem_get`, `sem_wait`)  |

## What is meant by Race Condition?

-> A race condition is a situation where two or more processes or threads access shared data at the same time, and the final outcome depends on the order of execution. 

## What is meant by Deadlock?

-> A deadlock is a situation where two or more processes or threads are waiting for each other to release resources, and none can proceed — they are stuck forever. 

## What is meant by Critical Section?

-> A Critical Section is a part of code that accesses shared resources (like variables, memory, files), and must not be executed by more than one thread/process at a time.
							( or )
   A critical section is a block of code where only one thread/process is allowed to enter at a time to avoid conflicts or data corruption.

## What is the difference between system v and POSIX?

	| **Feature**        | **System V IPC**                          | **POSIX IPC**                               |
	| ------------------ | ----------------------------------------- | ------------------------------------------- |
	| **Origin**         | UNIX System V (1980s)                     | POSIX standard (IEEE)                       |
	| **Naming**         | Uses integer keys (`ftok()`, IDs)         | Uses string names (e.g., `"/myqueue"`)      |
	| **Ease of Use**    | More complex, older style                 | Simpler, modern API                         |
	| **Portability**    | Less portable                             | More portable across UNIX-like systems      |
	| **Shared Memory**  | `shmget()`, `shmat()`                     | `shm_open()`, `mmap()`                      |
	| **Semaphores**     | `semget()`, `semctl()`, `semop()`         | `sem_open()`, `sem_wait()`, `sem_post()`    |
	| **Message Queues** | `msgget()`, `msgsnd()`, `msgrcv()`        | `mq_open()`, `mq_send()`, `mq_receive()`    |
	| **Cleanup**        | Must be manually removed (e.g., `shmctl`) | Can unlink like files (`shm_unlink`, etc.)  |
	| **Storage**        | Kernel-managed                            | Appears under filesystem (e.g., `/dev/shm`) |

## Describe the steps involved in creating and using a named pipe (FIFO) for IPC
-> 
	| **Step**             | **Action**                         |
	| -------------------- | ---------------------------------- |
	| 1. Create            | `mkfifo()` or `mkfifo` command     |
	| 2. Open              | `open()` from both reader & writer |
	| 3. Read/Write        | `read()`, `write()`                |
	| 4. Close             | `close()`                          |
	| 5. (Optional) Delete | `rm myfifo`                        |

## Describe the steps involved in creating and using a pipe for IPC
->
	| **Step**        | **Action**                                      |
	| --------------- | ----------------------------------------------- |
	| 1. Create       | `pipe(fd);`                                     |
	| 2. Fork         | `fork();`                                       |
	| 3. Close Unused | Each process closes the pipe end it doesn't use |
	| 4. Communicate  | `write()` in one, `read()` in the other         |
	| 5. Close        | Close all ends after use                        |
	| 6. Wait         | (Optional) `wait()` for synchronization         |
			

 ## What is Interprocess communication?
a) allows processes to communicate and synchronize their actions when using the same address space
✅b) allows processes to communicate and synchronize their actions
c) allows the processes to only synchronize their actions without communication
d) none of the mentioned

## Message passing system allows processes to 
✅a) communicate with each other without sharing the same address space
b) communicate with one another by resorting to shared data
c) share data
d) name the recipient or sender of the message

## Which of the following two operations are provided by the IPC facility?
a) write & delete message
b) delete & receive message
c) send & delete message
✅d) receive & send message

## Messages sent by a process __________
a) have to be of a fixed size
b) have to be a variable size
✅c) can be fixed or variable sized
d) none of the mentioned

## The link between two processes P and Q to send and receive messages is called
✅a) communication link
b) message-passing link
c) synchronization link
d) all of the mentioned

## Which of the following are TRUE for direct communication?
a) A communication link can be associated with N number of process(N = max. number of processes supported by system)
✅b) A communication link is associated with exactly two processes
c) Exactly N/2 links exist between each pair of processes(N = max. number of processes supported by system)
d) Exactly two link exists between each pair of processes

## In indirect communication between processes P and Q __________
a) there is another process R to handle and pass on the messages between P and Q
b) there is another machine between the two processes to help communication
✅c) there is a mailbox to help communication between P and Q
d) none of the mentioned

## In the non blocking send __________
a) the sending process keeps sending until the message is received
✅b) the sending process sends the message and resumes operation
c) the sending process keeps sending until it receives a message
d) none of the mentioned

## What is the role of a semaphore in IPC?
✅a) Coordinating access to shared resources
b) Transferring data between processes
c) Sending signals to other processes
d) Creating communication channels

## Bounded capacity and Unbounded capacity queues are referred to as __________
a) Programmed buffering
✅b) Automatic buffering
c) User defined buffering
d) No buffering

## What is the role of a semaphore in IPC?
✅a) Coordinating access to shared resources
b) Transferring data between processes
c) Sending signals to other processes
d) Creating communication channels

## Which IPC mechanism provides a message-oriented communication model?
a) Shared memory
b) Pipes
✅c) Message queues
d) Sockets

## In the context of IPC, what are signals used for?
a) Synchronizing processes
b) Coordinating access to shared memory
c) Sending messages between processes
✅d) Notifying processes about events or conditions

## Which synchronization primitive is used to ensure mutually exclusive access to critical sections of code?
a) Semaphore
✅b) Mutex
c) Condition variable
d) Barrier

## Which IPC mechanism provides a communication endpoint for processes to communicate over a network or between processes on the same system?
a) Named pipes
b) Message queues
c) Shared memory
✅d) Sockets

## What is the primary advantage of using message queues for IPC compared to shared memory?
a) Higher performance
✅b) More flexible communication model
c) Simplicity of implementation
d) Direct access to shared data

## Which IPC mechanism involves copying data between processes, potentially incurring additional overhead?
a) Shared memory
b) Message queues
✅c) Sockets
d) Semaphores

## Which aspect is crucial in IPC to ensure that multiple processes or threads coordinate their activities and access shared resources safely?
a) Data transfer efficiency
✅b) Synchronization
c) Message passing
d) Resource allocation

## Which IPC mechanism involves copying data between processes through a temporary storage area in the kernel?
a. Shared memory
b. Message queues
c. Sockets
✅d. Named pipes

## Which IPC mechanism provides a high-performance, shared memory region for data exchange between processes?
a) Pipes
b) Semaphores
c) Message queues
✅d) Shared memory

## Which IPC mechanism is being used in the following pseudocode snip?
// Producer process
while (true) {
produce_item(item);
send_item_to_queue(item);
}
// Consumer process
while (true) {
receive_item_from_queue(item);
consume_item(item);
}
a) Shared memory
✅b) Message queues
c) Sockets
d) Named pipes

## What synchronization primitive is being used in the following pseudocode snippet?
// Mutex initialization
mutex = create_mutex();
// Thread 1
lock_mutex(mutex);
// Critical section
// Access shared resource
unlock_mutex(mutex);
// Thread 2
lock_mutex(mutex);
// Critical section
// Access shared resource
unlock_mutex(mutex);
a) Semaphore
✅b) Mutex
c) Condition variable
d) Barrier

## What type of communication is being used in the following pseudocode snippet?
// Server process
create_socket();
bind_socket();
listen_for_connections();
while (true)
{
accept_connection();
receive_data_from_client();
process_data();
send_response_to_client();
}
// Client process
create_socket();
connect_to_server();
send_data_to_server();
receive_response_from_server();
a) Shared memory
b) Message passing
c) Message queues
✅d) Sockets
