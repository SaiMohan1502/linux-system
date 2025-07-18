## Discuss the significance of the getpid() and getppid() system calls.
```c
#include <stdio.h>
#include <unistd.h>
int main() {
    printf("My PID: %d\n", getpid());
    printf("My Parent's PID: %d\n", getppid());
    return 0;
}

		( OR )

#include <stdio.h>
#include <unistd.h>
int main() {
    pid_t pid = fork();
    if (pid == 0) {
        // Child process
        printf("\n[Child]\n");
        printf("PID  : %d\n", getpid());
        printf("PPID : %d\n", getppid());
    } else {
        // Parent process
        printf("\n[Parent]\n");
        printf("PID  : %d\n", getpid());
        printf("Child PID (from fork): %d\n", pid);
    } 
	return 0;
}
```

## Write a C program to demonstrate the use of fork() system call.

```c
#include <stdio.h>
#include <unistd.h>  // Required for fork()
int main() {
    int pid;
    pid = fork();  // Create a new process
    if (pid == 0) {
        // Child process
        printf("This is the child process.\n");
    } else {
        // Parent process
        printf("This is the parent process.\n");
    }
    return 0;
}
```

## Write a C program to illustrate the use of the execvp() function

```c
#include <stdio.h>
#include <unistd.h>
int main() {
    char *args[] = {"ls", "-l", NULL};  // Command and arguments
    printf("Before execvp()\n");
    execvp("ls", args); // Replace current process with 'ls -l'
    perror("execvp failed"); // Only runs if execvp fails
    return 0;
}
```

## Write a program in C to create a child process using fork() and print its PID.

```c
#include <stdio.h>
#include <unistd.h>
int main() {
    pid_t pid = fork();
    if (pid == 0) {
        // Child process
        printf("\n[Child]\n");
        printf("PID  : %d\n", getpid());
        printf("PPID : %d\n", getppid());
    } else {
        // Parent process
        printf("\n[Parent]\n");
        printf("PID  : %d\n", getpid());
        printf("Child PID (from fork): %d\n", pid);
    }
    return 0;
}
```

## Write a C program to create multiple child processes using fork() and display their PIDs.

```c
#include <stdio.h>
#include <unistd.h>
int main() {
int i;
for (i = 0; i < 3; i++) {
 if (fork() == 0) {
            // Child process
printf("Child %d: PID = %d\n", i + 1, getpid());

/*for ppid -> printf(“Child %d: PID = %d\n”, i+1, getpid(), getppid());*/

            return 0; // Important: child exits after printing
        }
    }

    return 0; // Parent exits after creating all children
}

```
## Write a C program to demonstrate the use of the waitpid() function for process synchronization

```c
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>
#include <sys/wait.h>

int main() {
    pid_t pid = fork();

    if (pid < 0) {
        perror("Fork failed");
        exit(1);
    }

    if (pid == 0) {
        // Child process
        printf("Child: PID = %d\n", getpid());
        sleep(2);
        printf("Child: Done.\n");
    } else {
        // Parent process
        printf("Parent: Waiting for child to finish...\n");
        waitpid(pid, NULL, 0);  // Wait for specific child without checking status
        printf("Parent: Child has finished.\n");
    }

    return 0;
}

What This Does:
•	fork() creates a child.
•	Child prints and sleeps for 2 seconds, then exits.
•	Parent waits for the child using waitpid(), no status handling.
•	Once the child finishes, parent prints and exits.

```

## Write a program in C to create a daemon process

```c
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>
int main() {
    pid_t pid = fork();  // Create a child process
    if (pid < 0) {
        // Error in fork
        perror("fork failed");
        exit(1);
    }
    if (pid > 0) {
        // Parent exits, child runs in background
        printf("Parent exiting. Daemon PID: %d\n", pid);
        exit(0);
    }
    // Child becomes a simple daemon
    while (1) {
        printf("Daemon is running in background...\n");
        sleep(3);  // Sleep for 3 seconds
    }
    return 0;
 }

To compile & Run the code:-
We use
	gcc filename.c -o filename
	./filename &
Where,
 ” & “ runs it in background. And output appears in terminal unless you redirect it.

To exit background running process we use
	click double enter
	then enter " kill daemon process_id " 
```

## Write a C program to demonstrate the use of the system() function for executing shell commands.

```c
#include <stdio.h>
#include <stdlib.h>
int main() {
    printf("Listing files in the current directory:\n");
    system("ls -l");  // Execute 'ls -l' shell command
    printf("\nCreating a file named 'demo.txt':\n");
    system("touch demo.txt");
    printf("\nDisplaying current date and time:\n");
    system("date");
    printf("\nRemoving 'demo.txt':\n");
    system("rm demo.txt");
    return 0;
}

 To compile & Run:-
	gcc filename.c -o filename
	./filename

 What This Program Does:-
	•	Runs ls -l to list files & Creates a file named “demo.txt”.
	•	Prints current date and time. Deletes the file
```

## Write a C program to create a process using fork() and pass arguments to the child process.

```c
#include <stdio.h>
#include <unistd.h>
#include <sys/wait.h>

int main() {
    pid_t pid = fork();

    if (pid == 0) {
        // Child process
        char *args[] = {"echo", "Hello from child", NULL};
        execvp("echo", args);  // Replaces child with 'echo' command
        perror("execvp failed");  // Only runs if exec fails
    } else if (pid > 0) {
        // Parent process
        wait(NULL);  // Wait for child to finish
        printf("Parent: Child finished\n");
    } else {
        perror("fork failed");
    }

    return 0;
}

```
## Write a program in C to demonstrate process synchronization using semaphore

```c
#include <stdio.h>
#include <unistd.h>
#include <semaphore.h>
#include <fcntl.h>
#include <sys/wait.h>

int main() {
    sem_t *sem = sem_open("/mysem", O_CREAT, 0644, 0);
    if (fork() == 0) {
        sem_wait(sem);
        printf("Child: Got signal\n");
        sem_close(sem);
    } else {
        sleep(1);
        sem_post(sem);
        wait(NULL);
        sem_close(sem);
        sem_unlink("/mysem");
    }
    return 0;
}

	To Compile & Run
		gcc semaphore_sync.c -o semaphore_sync -pthread
		./semaphore_sync
```
## Write a C program to demonstrate the use of the execvpe() function

```c
#define _GNU_SOURCE
#include <stdio.h>
#include <unistd.h>
#include <stdlib.h>

int main() {
    char *args[] = {"printenv", "MY_VAR", NULL};  // Program and argument
    char *envp[] = {"MY_VAR=Hello_from_execvpe", NULL};  // Custom environment

    execvpe("printenv", args, envp);  // Execute with custom env
    perror("execvpe failed");  // If exec fails
    return 1;
}


	To Compile & Run
		gcc execvpe_demo.c -o execvpe_demo
		./execvpe_demo
```

## Write a C program to create a process group and change its process group ID (PGID)

```c
#include <stdio.h>
#include <unistd.h>
#include <sys/types.h>
#include <sys/wait.h>

int main() {
    pid_t pid = fork();

    if (pid == 0) {
        // Child process
        printf("Child PID: %d\n", getpid());
        setpgid(0, 0);  // Set its own PGID to its PID
        printf("Child PGID changed to: %d\n", getpgrp());
    } else {
        // Parent process
        wait(NULL);  // Wait for child
        printf("Parent PID: %d, PGID: %d\n", getpid(), getpgrp());
    }

    return 0;
}

```
## Write a program in C to demonstrate inter-process communication (IPC) using shared memory

```c
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <unistd.h>
#include <sys/ipc.h>
#include <sys/shm.h>
#include <sys/wait.h>

int main() {
    key_t key = 1234;
    int shmid = shmget(key, 100, IPC_CREAT | 0666);
    char *data = (char *)shmat(shmid, NULL, 0);

    if (fork() == 0) {
        // Child process
        sleep(1);  // Wait for parent to write
        printf("Child reads: %s\n", data);
        shmdt(data);
    } else {
        // Parent process
        strcpy(data, "Hello from parent");
        wait(NULL);
        shmdt(data);
        shmctl(shmid, IPC_RMID, NULL);  // Remove shared memory
    }

    return 0;
}

```
## Write a C program to create a child process using vfork() and demonstrate its usage

```c
#include <stdio.h>
#include <unistd.h>
#include <stdlib.h>

int main() {
    pid_t pid;

    pid = vfork();  // Create child process using vfork()

    if (pid < 0) {
        perror("vfork failed");
        exit(1);
    } else if (pid == 0) {
        // Child process
        printf("Child: PID = %d\n", getpid());
        _exit(0);  // Use _exit() with vfork
    } else {
        // Parent process
        printf("Parent: PID = %d, Child PID = %d\n", getpid(), pid);
    }

    return 0;
}

	Key Points:
		1. vfork() is similar to fork(), but:

		2. The child shares the address space of the parent until it calls _exit() or exec().

		3. The parent is suspended until the child exits or executes a new program.

		4. Always use _exit() instead of exit() in the child when using vfork().

```
## Write a C program to create a pipeline between two processes using the pipe() system call.

```c
#include <stdio.h>
#include <unistd.h>
#include <string.h>

int main() {
    int fd[2];
    pipe(fd);  // Create the pipe

    if (fork() == 0) {
        // Child process - read end
        close(fd[1]);  // Close write end
        char buffer[100];
        read(fd[0], buffer, sizeof(buffer));
        printf("Child received: %s\n", buffer);
        close(fd[0]);
    } else {
        // Parent process - write end
        close(fd[0]);  // Close read end
        char msg[] = "Hello from parent";
        write(fd[1], msg, strlen(msg) + 1);
        close(fd[1]);
    }

    return 0;
}

```
## Write a program in C to demonstrate the use of the nice() system call for adjusting process priority

```c
#include <stdio.h>
#include <unistd.h>
#include <sys/time.h>
#include <sys/resource.h>

int main() {
    int old_nice = getpriority(PRIO_PROCESS, 0);
    printf("Original priority: %d\n", old_nice);

    // Increase niceness by 5 (lower priority)
    int new_nice = nice(5);

    printf("New priority after nice(5): %d\n", new_nice);

    // Simulate some work
    for (int i = 0; i < 5; i++) {
        printf("Process is running... (%d)\n", i + 1);
        sleep(1);
    }

    return 0;
}

```
## Write a C program to demonstrate the use of the clone() system call to create a thread

```c
#define _GNU_SOURCE
#include <sched.h>
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>
#include <signal.h>

int child_func(void *arg) {
    printf("Child PID: %d, Msg: %s\n", getpid(), (char *)arg);
    return 0;
}

int main() {
    char *stack = malloc(4096); // small stack
    clone(child_func, stack + 4096, SIGCHLD | CLONE_VM, "Hi from clone");
    sleep(1); // give time to child
    free(stack);
    return 0;
}

```
## Write a C program to create a child process using fork() and communicate between parent and child using pipes.

```c
#include <stdio.h>
#include <unistd.h>
#include <string.h>

int main() {
    int fd[2]; // fd means file_descriptor, fd[0] -> read end, fd[1] -> write end
    pipe(fd);  // Create pipe

    if (fork() == 0) {
        // Child process
        close(fd[1]);  // Close write end
        char msg[100];
        read(fd[0], msg, sizeof(msg));
        printf("Child received: %s\n", msg);
        close(fd[0]);
    } else {
        // Parent process
        close(fd[0]);  // Close read end
        char *text = "Hello from parent!";
        write(fd[1], text, strlen(text) + 1);
        close(fd[1]);
    }

    return 0;
}

```
## Write a C program to demonstrate process synchronization using the
fork() and wait() system calls.

```c
#include <stdio.h>
#include <unistd.h>
#include <sys/wait.h>

int main() {
    pid_t pid = fork();

    if (pid == 0) {
        // Child process
        printf("Child: Doing some work...\n");
        sleep(2);  // Simulate work
        printf("Child: Work done!\n");
    } else {
        // Parent process
        wait(NULL);  // Wait for child to finish
        printf("Parent: Child finished. Continuing...\n");
    }

    return 0;
}

```
## Write a C program to create a child process using fork() and demonstrate inter-process communication (IPC) using shared memory

```c
#include <stdio.h>
#include <sys/ipc.h>
#include <sys/shm.h>
#include <unistd.h>
#include <string.h>
#include <sys/wait.h>

int main() {
    key_t key = 1234; 
    int shmid = shmget(key, 100, IPC_CREAT | 0666);  // Create shared memory
    char *data = (char *)shmat(shmid, NULL, 0);      // Attach to shared memory

    pid_t pid = fork();

    if (pid == 0) {
        // Child process reads
        sleep(1);  // Wait for parent to write
        printf("Child read from shared memory: %s\n", data);
        shmdt(data);  // Detach
    } else {
        // Parent process writes
        strcpy(data, "Hello from parent");
        wait(NULL);  // Wait for child to finish
        shmdt(data);  // Detach
        shmctl(shmid, IPC_RMID, NULL);  // Remove shared memory
    }

    return 0;
}

```
## Write a C program to demonstrate the use of the prctl() system call to change process attributes.

```c
#include <stdio.h>
#include <sys/prctl.h>
#include <string.h>
#include <unistd.h>

int main() {
    // Set process name
    prctl(PR_SET_NAME, "MyProc");

    // Buffer to store the name
    char name[20];
    prctl(PR_GET_NAME, name);

    printf("Current process name: %s\n", name);
    printf("PID: %d\n", getpid());

    return 0;
}

 where, 
	prctl() -> Process Control ( used to get/set process related attributes ) 
```
## Write a C program to create a child process using fork() and demonstrate process synchronization using semaphores.

```c
#include <stdio.h>
#include <unistd.h>
#include <semaphore.h>
#include <fcntl.h>
#include <sys/wait.h>

int main() {
    sem_t *sem = sem_open("/sync", O_CREAT, 0644, 0); // sem_t -> to declare/hold a semaphore

    if (fork() == 0) {
        sem_wait(sem);
        printf("Child: Got signal from parent\n");
        sem_close(sem);
    } else {
        sleep(1);
        printf("Parent: Sending signal\n");
        sem_post(sem);
        wait(NULL);
        sem_close(sem);
        sem_unlink("/sync");
    }

    return 0;
}

```
## Write a C program to create a child process using fork() and demonstrate process communication using message queues.

```c
#include <stdio.h>       // for printf
#include <string.h>      // for strcpy
#include <unistd.h>      // for fork
#include <sys/ipc.h>     // for key creation
#include <sys/msg.h>     // for message queue
#include <sys/wait.h>    // for wait

// Define message structure
struct msgbuf {
    long mtype;          // message type (must be > 0)
    char text[50];       // message text
};

int main() {
    key_t key = ftok(".", 1);                // generate a unique key
    int qid = msgget(key, 0666 | IPC_CREAT); // create a message queue
    struct msgbuf msg;

    if (fork() == 0) {
        // In child: send message
        msg.mtype = 1;                               // set message type
        strcpy(msg.text, "Hello from child!");       // put message in text
        msgsnd(qid, &msg, sizeof(msg.text), 0);      // send message to queue
    } else {
        // In parent: receive message
        msgrcv(qid, &msg, sizeof(msg.text), 1, 0);    // receive message
        printf("Parent got: %s\n", msg.text);         // print it
        msgctl(qid, IPC_RMID, NULL);                  // delete the message queue
    }

    return 0;
}

```
## Write a C program to create a child process using fork() and demonstrate process synchronization using condition variables

```c
#include <stdio.h>
#include <unistd.h>
#include <pthread.h>
#include <sys/mman.h>
#include <sys/wait.h>

int main() {
    struct { pthread_mutex_t m; pthread_cond_t c; int r; } *sh;
    sh = mmap(0, sizeof(*sh), PROT_READ | PROT_WRITE, MAP_SHARED | MAP_ANON, -1, 0);

    pthread_mutexattr_t ma; pthread_condattr_t ca;
    pthread_mutexattr_init(&ma); pthread_condattr_init(&ca);
    pthread_mutexattr_setpshared(&ma, PTHREAD_PROCESS_SHARED);
    pthread_condattr_setpshared(&ca, PTHREAD_PROCESS_SHARED);
    pthread_mutex_init(&sh->m, &ma); pthread_cond_init(&sh->c, &ca);

    if (fork() == 0) {
        pthread_mutex_lock(&sh->m);
        while (!sh->r) pthread_cond_wait(&sh->c, &sh->m);
        printf("Child got signal\n");
        pthread_mutex_unlock(&sh->m);
    } else {
        sleep(1);
        pthread_mutex_lock(&sh->m);
        sh->r = 1; pthread_cond_signal(&sh->c);
        pthread_mutex_unlock(&sh->m);
        wait(0);
    }
    return 0;
}

	SIMPLE EXPLANATION:-

		Shared memory block holds mutex m, condition variable c, and flag r.

		Child waits until r == 1.

		Parent sets r = 1 and sends signal.

		Child resumes and prints a message.
```

## Write a C program to create a child process using fork() and demonstrate process communication using sockets

```c
#include <stdio.h>
#include <unistd.h>
#include <sys/socket.h>
#include <string.h>

int main() {
    int sv[2]; // sv[0] for parent, sv[1] for child
    socketpair(AF_UNIX, SOCK_STREAM, 0, sv);

    if (fork() == 0) {
        // Child process
        close(sv[0]);
        char msg[] = "Hello from child";
        write(sv[1], msg, strlen(msg));
        close(sv[1]);
    } else {
        // Parent process
        close(sv[1]);
        char buf[100];
        read(sv[0], buf, sizeof(buf));
        printf("Parent received: %s\n", buf);
        close(sv[0]);
    }

    return 0;
}

	SIMPLE EXPLANATION:-

		A parent and child process are created using fork().

		A socketpair is used for communication between them.

		Child sends a message using one socket.

		Parent reads the message from the other socket.

		Simple and fast inter-process communication (IPC) using sockets — no networking required.
```

## Write a C program to create a child process using fork() and demonstrate process synchronization using mutexes.

```c
#include <stdio.h>
#include <unistd.h>
#include <pthread.h>
#include <sys/mman.h>
#include <sys/wait.h>

int main() {
    // Shared memory for mutex
    pthread_mutex_t *mutex = mmap(NULL, sizeof(*mutex),
        PROT_READ | PROT_WRITE, MAP_SHARED | MAP_ANON, -1, 0);

    pthread_mutexattr_t attr;
    pthread_mutexattr_init(&attr);
    pthread_mutexattr_setpshared(&attr, PTHREAD_PROCESS_SHARED);
    pthread_mutex_init(mutex, &attr);

    if (fork() == 0) {
        pthread_mutex_lock(mutex);
        printf("Child: Inside critical section\n");
        sleep(1);
        pthread_mutex_unlock(mutex);
    } else {
        sleep(0.5);  // Ensure child tries first
        pthread_mutex_lock(mutex);
        printf("Parent: Inside critical section\n");
        pthread_mutex_unlock(mutex);
        wait(NULL);
    }

    return 0;
}

	SIMPLE EXPLANATION

		mmap() is used to share the mutex between parent and child.

		PTHREAD_PROCESS_SHARED allows the mutex to work across processes.

		Only one process enters the critical section at a time due to the mutex.
```

## Write a C program to create a child process using fork() and demonstrate process communication using named pipes (FIFOs)

```c
#include <stdio.h>
#include <unistd.h>
#include <sys/types.h>
#include <sys/stat.h>
#include <fcntl.h>
#include <string.h>
#include <sys/wait.h>

int main() {
    char *fifo = "myfifo";
    mkfifo(fifo, 0666);  // Create named pipe

    if (fork() == 0) {
        // Child process: write to FIFO
        int fd = open(fifo, O_WRONLY);
        char msg[] = "Hello from child";
        write(fd, msg, strlen(msg));
        close(fd);
    } else {
        // Parent process: read from FIFO
        char buf[100];
        int fd = open(fifo, O_RDONLY);
        read(fd, buf, sizeof(buf));
        printf("Parent received: %s\n", buf);
        close(fd);
        wait(NULL);
        unlink(fifo); // Remove the FIFO file
    }

    return 0;
}

	SIMPLE EXPLnation 
		
		mkfifo() creates a named pipe called "myfifo".

		Child writes a message to the FIFO.

		Parent reads the message from the FIFO.

		After communication, the FIFO file is deleted using unlink().
```

## Write a C program to create a child process using fork() and demonstrate process communication using shared memory and semaphore

```c
#include <stdio.h>
#include <stdlib.h>
#include <sys/ipc.h>
#include <sys/shm.h>
#include <sys/sem.h>
#include <unistd.h>
#include <wait.h>

#define SHM_KEY 1234
#define SEM_KEY 5678

union semun { int val; };

int main() {
    int shmid = shmget(SHM_KEY, sizeof(int), IPC_CREAT | 0666);
    int semid = semget(SEM_KEY, 1, IPC_CREAT | 0666);
    union semun arg = {1};
    semctl(semid, 0, SETVAL, arg);
    int *data = (int *)shmat(shmid, NULL, 0);
    *data = 0;

    if (fork() == 0) {
        struct sembuf p = {0, -1, SEM_UNDO}, v = {0, 1, SEM_UNDO};
        semop(semid, &p, 1);
        *data += 10;
        semop(semid, &v, 1);
        shmdt(data);
        exit(0);
    } else {
        wait(NULL);
        struct sembuf p = {0, -1, SEM_UNDO}, v = {0, 1, SEM_UNDO};
        semop(semid, &p, 1);
        *data += 5;
        printf("Final shared value: %d\n", *data);
        semop(semid, &v, 1);
        shmdt(data);
        shmctl(shmid, IPC_RMID, NULL);
        semctl(semid, 0, IPC_RMID);
    }
    return 0;
}

```
