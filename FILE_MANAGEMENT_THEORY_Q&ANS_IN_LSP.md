## What are the types of files present in Linux OS?
```c
- Regular files (-)
- Directory files (d)
- Character device files (c)
- Block device files (b)
- Symbolic links (l)
- Socket files (s)
- Named pipes (FIFOs) (p)
```
## How do IPC Objects, named pipes, be accessed?
```c
- Named pipes (FIFOs) are accessed via filesystem pathnames, just like regular files.
- Created using mkfifo() or mknod().
- Accessed using open(), read(), and write() system calls.
```
## User space application sends request to hardware by using which I/O calls?
```c
- read(), write(), ioctl(), mmap()
```
## Why are basic I/O calls called universal I/O calls?
```c
- Because they work uniformly across all file types: regular files, pipes, sockets, devices.
```
## What is the content of the Inode Object?
```c
- File type, Permissions, Owner UID/GID, Timestamps, File size, Pointers to data blocks, Link count, Device ID
```
## In which object information of the file get stored?
```c
- In the inode object.
```
## Kernel uses which object to represent a file?
```c
- struct file (file object) and struct inode
```
## Can we access the file information present in inode from the user application? If yes, then how?
```c
- Yes, indirectly using stat(), fstat(), or lstat() system calls.
```
## Which system calls are used to access the file information?
```c
- stat(), fstat(), lstat()
```
## ls command internally invokes which system call to access the file information?
```c
- stat(), lstat()
```
## What happens after the kernel finds its inode object?
```c
- It creates a file object, sets up file descriptors, and loads metadata from the inode.
```
## What are the contents of the file object?
```c
- File position, Access mode, Reference to inode, Pointer to file operations, Flags, File descriptor
```
## Kernel uses which object to represent the open files?
```c
- struct file
```
## What is the difference between the primary data and other members of the file object?
```c
- Primary data: File offset, access flags
- Other members: Pointers to inode, operations, private data
```
## Where does the file object base address get stored?
```c
- In the File Descriptor Table (FDT) of the process.
```
## When is the fd table created and what is the size of the fd table?
```c
- Created during process creation. Default limit: 1024 (can be changed via ulimit/sysctl).
```
## When is the file object created?
```c
- When a file is opened using open() or similar system calls.
```
## What does open() return?
```c
- A file descriptor (int)
```
## When a file opens for the first time by using open() calls in your program, what is returned?
```c
- A non-negative integer (file descriptor), or -1 on failure.
```
## Why do read() system calls need access to file objects?
```c
- To access file cursor, mode, inode info, and other file-related state.
```
## Which system call do we use to change the cursor position without reading and writing on a file?
```c
- lseek()
```
## Can we open the same file from multiple processes? Explain the memory segment in kernel space?
```c
- Yes. Each process has its own FD table pointing to file objects, which refer to common inode object in kernel space.
```
## Kernel uses which object to represent a file?
```c
- struct inode and struct file
```
## Is there any limit on number of files that can be opened from the program?
```c
- Yes. Soft limit (ulimit -n), Hard limit (/proc/sys/fs/file-max)
```
## What are the standard I/O calls? Give some examples and alternate name.
```c
- Buffered I/O or Library I/O. Examples: fopen(), fread(), fwrite(), fgets(), fprintf()
```
## What are the basic I/O calls? Give some examples and alternate name.
```c
- System I/O or Low-level I/O. Examples: open(), read(), write(), close()
```
## What is the difference between basic I/O calls and standard I/O calls?
```c
| Basic I/O (System) | Standard I/O (Buffered) |
|--------------------|-------------------------|
| Uses system calls  | Uses stdlib wrappers    |
| File descriptor    | FILE* pointer           |
| Unbuffered         | Buffered                |
````
## Other than basic and standard I/O calls, is there any method to access the file?
```c
- Yes: Memory-mapped I/O (mmap), Asynchronous I/O (AIO), Direct I/O (O_DIRECT)
```
## How do user space applications get access to the content of inode objects?
```c
- Indirectly via stat() or by reading /proc or ls -i
```
## How do you get access to kernel objects/data structure/information present in kernel space?
```c
- Through: /proc, /sys, System calls, Writing kernel modules
```
## Which system calls are used to access the file object?
```c
- open(), read(), write(), close(), dup(), fcntl()
```
## Which system calls are used to access the Inode object?
```c
- stat(), fstat(), lstat()
```
## How to print "Hello World" on the screen using basic I/O calls?
```c
#include <unistd.h>
int main() {
    write(1, "Hello World\n", 12);
    return 0;
}
```
## Which system call do we use to duplicate the file descriptor?
```c
- dup(), dup2(), dup3()
```
## What are the advantages of dup system calls?
```c
- Redirect output/input, Allows multiple FDs to point to same file object, Useful in I/O redirection
```
## Which object stores the ownership information of a file?
```c
- Inode object
```
## Which command is used to display the username corresponding ID?
```c
- id or whoami or getent passwd <UID>
```
## How can we see the multiple user information in our system?
```c
- cat /etc/passwd or getent passwd
```
## Which command is used to change the UID and GID?
```c
- usermod -u <new_uid> username
- groupmod -g <new_gid> groupname
```
