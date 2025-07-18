## Implement a program that uses pipes for communication between a parent and child process. Show how data can be passed between processes using pipes.

```c
#include <stdio.h>
#include <unistd.h>

int main() {
    int fd[2];
    char msg[] = "Hi child";
    char buffer[20];

    pipe(fd);
    
    if (fork() == 0) {
        // Child reads
        read(fd[0], buffer, sizeof(buffer));
        printf("Child got: %s\n", buffer);
    } else {
        // Parent writes
        write(fd[1], msg, sizeof(msg));
    }

    return 0;
}

```
## Create a program where two processes communicate synchronously using pipes. Ensure that one process waits for the other to finish before proceeding.

```c
#include <stdio.h>
#include <unistd.h>
#include <string.h>
#include <sys/wait.h>

int main() {
    int fd[2];
    char msg[] = "Message from parent";
    char buffer[100];

    pipe(fd);  // Create the pipe

    if (fork() == 0) {
        // Child process
        close(fd[1]); // Close unused write end
        read(fd[0], buffer, sizeof(buffer));
        printf("Child received: %s\n", buffer);
        close(fd[0]); // Close read end
    } else {
        // Parent process
        close(fd[0]); // Close unused read end
        write(fd[1], msg, strlen(msg) + 1); // Write with null terminator
        close(fd[1]); // Close write end
        wait(NULL);   // Wait for child to finish
        printf("Parent: Child has finished reading.\n");
    }

    return 0;
}

```
## Implement a program that uses Named pipes for communication between two processes.

```c
#include <stdio.h>
#include <unistd.h>
#include <fcntl.h>
#include <sys/stat.h>
#include <string.h>

int main() {
    const char *fifo = "/tmp/myfifo";
    char buffer[100];

    // Create the named pipe (FIFO)
    mkfifo(fifo, 0666);  // 0666 = read & write permissions for everyone

    pid_t pid = fork();

    if (pid < 0) {
        perror("Fork failed");
        return 1;
    }

    if (pid == 0) {
        // Child process: acts as reader
        int fd = open(fifo, O_RDONLY);
        read(fd, buffer, sizeof(buffer));
        printf("Child received: %s\n", buffer);
        close(fd);
    } else {
        // Parent process: acts as writer
        int fd = open(fifo, O_WRONLY);
        char msg[] = "Hello from parent (writer)";
        write(fd, msg, strlen(msg) + 1);  // +1 for '\0'
        close(fd);
        wait(NULL);  // Wait for child to finish
    }

    // Remove the FIFO file
    unlink(fifo);

    return 0;
}

```
## Write a C program to create a message queue using the msgget system call. Ensure that the program checks for errors during the creation process.

```c
#include <stdio.h>
#include <sys/ipc.h>
#include <sys/wait.h>
#include <sys/msg.h>

int main() {
    key_t key;
    int msgid;

    // Generate a unique key
    key = ftok("msgqueuefile", 65);  // "msgqueuefile" can be any existing file

    if (key == -1) {
        perror("ftok failed");
        return 1;
    }

    // Create a message queue with read & write permission (0666)
    msgid = msgget(key, 0666 | IPC_CREAT);

    if (msgid == -1) {
        perror("msgget failed");
        return 1;
    }

    printf("Message queue created successfully. ID = %d\n", msgid);

    return 0;
}

/* 1st use " touch msgqueuefile " to compile & run */
```
## Develop two separate C programs, one for sending messages and the other for receiving messages through a created message queue.

```c
#include <stdio.h>
#include <sys/ipc.h>
#include <sys/msg.h>
#include <sys/wait.h>
#include <unistd.h>
#include <string.h>

struct msg {
    long type;
    char text[100];
};

int main() {
    key_t key = ftok("msgqueuefile", 65);
    int id = msgget(key, 0666 | IPC_CREAT);
    struct msg m;

    if (fork() == 0) {
        // Child (Receiver)
        msgrcv(id, &m, sizeof(m.text), 1, 0);
        printf("Child got: %s\n", m.text);
    } else {
        // Parent (Sender)
        m.type = 1;
        strcpy(m.text, "Hello!");
        msgsnd(id, &m, sizeof(m.text), 0);
        wait(NULL);
        msgctl(id, IPC_RMID, NULL);
    }
    return 0;
}

/* 1st use " touch msgqueuefile " to compile & run */

```
## Create a program to remove an existing message queue using the msgctl system call. Ensure that the program prompts the user for confirmation before deleting the message queue.

```c
#include <stdio.h>
#include <sys/ipc.h>
#include <sys/msg.h>

int main() {
    key_t key = ftok("msgqueuefile", 65);
    int id = msgget(key, 0666);
    char c;

    if (id == -1) return perror("msgget"), 1;

    printf("Delete queue (ID %d)? (y/n): ", id);
    scanf(" %c", &c);

    if (c == 'y' || c == 'Y')
        msgctl(id, IPC_RMID, NULL), printf("Deleted.\n");
    else
        printf("Cancelled.\n");

    return 0;
}

```
## Design a multithreaded program where threads communicate through named pipes.

```c
#include <stdio.h>
#include <pthread.h>
#include <unistd.h>
#include <fcntl.h>
#include <sys/stat.h>

#define FIFO "/tmp/myfifo"

void* writer(void* arg) {
    int fd = open(FIFO, O_WRONLY);
    write(fd, "Hello", 6);
    close(fd);
    return NULL;
}

void* reader(void* arg) {
    char buf[100];
    int fd = open(FIFO, O_RDONLY);
    read(fd, buf, sizeof(buf));
    printf("Reader got: %s\n", buf);
    close(fd);
    return NULL;
}

int main() {
    mkfifo(FIFO, 0666);

    pthread_t t1, t2;
    pthread_create(&t2, NULL, reader, NULL);
    sleep(1); // Make sure reader starts first
    pthread_create(&t1, NULL, writer, NULL);

    pthread_join(t1, NULL);
    pthread_join(t2, NULL);

    unlink(FIFO);
    return 0;
}

```
## Write a C program where two processes communicate using message queues.Implement sending and receiving messages between the processes using msgget, msgsnd, and msgrcv.

```c
#include <stdio.h>
#include <sys/ipc.h>
#include <sys/msg.h>
#include <sys/wait.h>
#include <unistd.h>
#include <string.h>

struct msg { long type; char text[100]; };

int main() {
    key_t key = ftok("msgqueuefile", 65);
    int id = msgget(key, 0666 | IPC_CREAT);
    struct msg m;

    if (fork() == 0) {
        msgrcv(id, &m, sizeof(m.text), 1, 0);
        printf("Child got: %s\n", m.text);
    } else {
        m.type = 1;
        strcpy(m.text, "Hello");
        msgsnd(id, &m, sizeof(m.text), 0);
        wait(NULL);
        msgctl(id, IPC_RMID, NULL);
    }

    return 0;
}

/* 1st use " touch msgqueuefile " to compile & run */
```
## Implement a program where two processes communicate synchronously using message queues. Ensure that one process waits for the other to finish before proceeding.

```c
#include <stdio.h>
#include <sys/ipc.h>
#include <sys/msg.h>
#include <unistd.h>
#include <string.h>

struct msg { long type; char text[100]; };

int main() {
    key_t key = ftok("msgfile", 65);
    int id = msgget(key, 0666 | IPC_CREAT);
    struct msg m;

    if (fork() == 0) {
        msgrcv(id, &m, sizeof(m.text), 1, 0);
        printf("Child got: %s\n", m.text);
        m.type = 2;
        strcpy(m.text, "Hi parent");
        msgsnd(id, &m, sizeof(m.text), 0);
    } else {
        m.type = 1;
        strcpy(m.text, "Hello child");
        msgsnd(id, &m, sizeof(m.text), 0);
        msgrcv(id, &m, sizeof(m.text), 2, 0);
        printf("Parent got: %s\n", m.text);
        msgctl(id, IPC_RMID, NULL);
    }
    return 0;
}

/* 1st use " touch msgfile " to compiler & run */
```
## Design a program that uses a message queue for synchronization between multipleprocesses.

```c
#include <stdio.h>
#include <stdlib.h>
#include <sys/ipc.h>
#include <sys/msg.h>
#include <unistd.h>
#include <string.h>
#include <sys/wait.h>

#define CHILD_COUNT 3

struct msg {
    long type;
    char text[100];
};

int main() {
    key_t key = ftok("msgsync", 65);
    int msgid = msgget(key, 0666 | IPC_CREAT);
    struct msg m;

    for (int i = 0; i < CHILD_COUNT; i++) {
        if (fork() == 0) {
            // Child process
            m.type = 1;
            sprintf(m.text, "Child %d done", i + 1);
            msgsnd(msgid, &m, sizeof(m.text), 0);
            exit(0);
        }
    }

    // Parent: wait for all children
    for (int i = 0; i < CHILD_COUNT; i++) {
        msgrcv(msgid, &m, sizeof(m.text), 1, 0);
        printf("Parent received: %s\n", m.text);
    }

    while (wait(NULL) > 0);  // Reap all children
    msgctl(msgid, IPC_RMID, NULL);  // Clean up

    return 0;
}

```
## Write a C program that initializes a shared memory segment using shmget.

```c
#include <stdio.h>
#include <sys/ipc.h>
#include <sys/shm.h>

int main() {
    key_t key = ftok("shmfile", 65);  // Generate a unique key
    if (key == -1) {
        perror("ftok");
        return 1;
    }

    int shmid = shmget(key, 1024, 0666 | IPC_CREAT);  // Create shared memory segment
    if (shmid == -1) {
        perror("shmget");
        return 1;
    }

    printf("Shared memory segment created. ID = %d\n", shmid);
    return 0;
}

/* 1st use " touch shmfile " to compile & run */ 

```
## Develop a program that attaches to a previously created shared memory segment using shmat and detaches using shmdt.

```c
#include <stdio.h>
#include <sys/ipc.h>
#include <sys/shm.h>
#include <string.h>

int main() {
    key_t key = ftok("shmfile", 65);  // Must match key used during creation
    int shmid = shmget(key, 1024, 0666);  // Access existing segment

    if (shmid == -1) {
        perror("shmget");
        return 1;
    }

    // Attach to shared memory
    char *data = (char*) shmat(shmid, NULL, 0);
    if (data == (char*)(-1)) {
        perror("shmat");
        return 1;
    }

    // Write to shared memory
    strcpy(data, "Hello from attached process!");

    // Read and print from shared memory
    printf("Shared memory says: %s\n", data);

    // Detach from shared memory
    if (shmdt(data) == -1) {
        perror("shmdt");
        return 1;
    }

    printf("Detached from shared memory.\n");
    return 0;
}

/* 1st use " touch shmfile " to compile & run */ 

```
## Create a program that forks multiple processes, and each process communicates using shared memory.

```c
#include <stdio.h>
#include <sys/ipc.h>
#include <sys/shm.h>
#include <unistd.h>
#include <sys/wait.h>

int main() {
    int n = 3;
    key_t k = ftok("shmfile", 65);
    int id = shmget(k, n * sizeof(int), 0666 | IPC_CREAT);
    int *data = (int*) shmat(id, NULL, 0);

    for (int i = 0; i < n; i++)
        if (fork() == 0) { data[i] = getpid(); shmdt(data); return 0; }

    while (wait(NULL) > 0);
    for (int i = 0; i < n; i++)
        printf("Child %d PID: %d\n", i + 1, data[i]);

    shmdt(data);
    shmctl(id, IPC_RMID, NULL);
    return 0;
}

/* 1st use " touch shmfile " to compile & run */ 

```
## Write a program that dynamically creates shared memory segments based on user input.

```c
#include <stdio.h>
#include <sys/ipc.h>
#include <sys/shm.h>
#include <string.h>

int main() {
    int size;
    printf("Size in bytes: ");
    scanf("%d", &size);

    key_t key = ftok("shmfile", 65);
    int id = shmget(key, size, 0666 | IPC_CREAT);
    char *data = (char*) shmat(id, NULL, 0);

    printf("Message: ");
    getchar(); // clear leftover newline
    fgets(data, size, stdin);

    printf("Stored: %s", data);
    shmdt(data);
    return 0;
}

/* 1st use " touch shmfile " to compile & run */ 

```
## Create a program that monitors and displays the usage statistics of shared memory segments, such as the amount of memory used and the number of attached processes.

```c
#include <stdio.h>
#include <sys/ipc.h>
#include <sys/shm.h>

int main() {
    key_t key = ftok("shmfile", 65);  // Key must match an existing segment
    int shmid = shmget(key, 0, 0666); // Get existing segment (size 0 means no new allocation)

    if (shmid == -1) {
        perror("shmget");
        return 1;
    }

    struct shmid_ds info;

    if (shmctl(shmid, IPC_STAT, &info) == -1) {
        perror("shmctl");
        return 1;
    }

    printf("Shared Memory ID   : %d\n", shmid);
    printf("Segment Size       : %zu bytes\n", info.shm_segsz);
    printf("Attached Processes : %lu\n", (unsigned long)info.shm_nattch);

    return 0;
}

/* 1st use " touch shmfile " to compile & run */ 

```
## Design a program that dynamically resizes a shared memory segment based on changing requirements.

```c
#include <stdio.h>
#include <string.h>
#include <sys/ipc.h>
#include <sys/shm.h>

int main() {
    key_t k1 = ftok("shmfile", 65), k2 = ftok("shmfile", 66);
    int old_id = shmget(k1, 64, 0666 | IPC_CREAT);
    char *old = (char*) shmat(old_id, NULL, 0);
    strcpy(old, "Hello");

    int new_id = shmget(k2, 256, 0666 | IPC_CREAT);
    char *new = (char*) shmat(new_id, NULL, 0);
    strcpy(new, old);
    strcat(new, " (resized)");

    printf("New: %s\n", new);

    shmdt(old); shmdt(new);
    shmctl(old_id, IPC_RMID, NULL);
    shmctl(new_id, IPC_RMID, NULL);
    return 0;
}

/* 1st use " touch shmfile " to compile & run */

```
## Write a C program that uses semget to create a new semaphore set

```c
#include <stdio.h>
#include <sys/ipc.h>
#include <sys/sem.h>

int main() {
    key_t key = ftok("semfile", 75);  // Generate unique key
    int semid = semget(key, 1, 0666 | IPC_CREAT);  // Create 1 semaphore

    if (semid == -1) {
        perror("semget");
        return 1;
    }

    printf("Semaphore set created. ID = %d\n", semid);
    return 0;
}

/* 1st use " touch semfile " to compile & run */

```
## Enhance the program to initialize the values of the semaphores(binary semaphore) in the set using semctl.

```c
#include <stdio.h>
#include <sys/ipc.h>
#include <sys/sem.h>
#include <sys/types.h>
#include <stdlib.h>

int main() {
    key_t key = ftok("semfile", 75);
    int semid = semget(key, 1, 0666 | IPC_CREAT);  // Create 1 semaphore

    if (semid == -1) {
        perror("semget");
        return 1;
    }

    // Initialize the semaphore to 1 (binary semaphore)
    if (semctl(semid, 0, SETVAL, 1) == -1) {
        perror("semctl - SETVAL");
        return 1;
    }

    printf("Binary semaphore created and initialized to 1. ID = %d\n", semid);
    return 0;
}

/* 1st use " touch semfile " to compile & run */

```
## Design a program where multiple processes are forked and synchronization is achieved using semaphores.

```c
#include <stdio.h>
#include <sys/ipc.h>
#include <sys/sem.h>
#include <unistd.h>
#include <sys/wait.h>

union semun { int val; };

void P(int id) { struct sembuf s = {0, -1, 0}; semop(id, &s, 1); }
void V(int id) { struct sembuf s = {0,  1, 0}; semop(id, &s, 1); }

int main() {
    key_t key = ftok("semfile", 75);
    int semid = semget(key, 1, 0666 | IPC_CREAT);
    union semun u = {1}; semctl(semid, 0, SETVAL, u);

    for (int i = 0; i < 3; i++)
        if (fork() == 0) {
            P(semid);
            printf("Child %d in critical section\n", i + 1);
            sleep(1);
            V(semid);
            return 0;
        }

    while (wait(NULL) > 0);
    semctl(semid, 0, IPC_RMID);
    return 0;
}

/* 1st use " touch semfile " to compile & run */
```
## Write a program that combines semaphores and shared memory for synchronization between processes.

```c
#include <stdio.h>
#include <sys/ipc.h>
#include <sys/shm.h>
#include <sys/sem.h>
#include <unistd.h>
#include <sys/wait.h>

union semun { int val; };
void P(int id) { struct sembuf b = {0, -1, 0}; semop(id, &b, 1); }
void V(int id) { struct sembuf b = {0, 1, 0};  semop(id, &b, 1);  }

int main() {
    int sid = semget(ftok("semfile", 1), 1, 0666 | IPC_CREAT);
    semctl(sid, 0, SETVAL, (union semun){1});
    int shid = shmget(ftok("shmfile", 1), sizeof(int), 0666 | IPC_CREAT);
    int *c = (int*) shmat(shid, NULL, 0); *c = 0;

    for (int i = 0; i < 3; i++)
        if (fork() == 0) {
            P(sid); (*c)++; printf("Child %d: %d\n", i + 1, *c); V(sid);
            shmdt(c); return 0;
        }

    while (wait(NULL) > 0);
    printf("Final: %d\n", *c);
    shmdt(c); shmctl(shid, IPC_RMID, NULL); semctl(sid, 0, IPC_RMID);
    return 0;
}

/* 1st use " touch semfile shmfile" to compile & run */

```
## Create a multithreaded program where threads synchronize using semaphore sets.

```c
#include <stdio.h>
#include <pthread.h>
#include <sys/ipc.h>
#include <sys/sem.h>
#include <unistd.h>

union semun { int val; };
int semid;

void P() { struct sembuf s = {0, -1, 0}; semop(semid, &s, 1); }
void V() { struct sembuf s = {0,  1, 0}; semop(semid, &s, 1); }

void* run(void *arg) {
    int id = *(int*)arg;
    P(); printf("Thread %d in\n", id); sleep(1); printf("Thread %d out\n", id); V();
    return NULL;
}

int main() {
    semid = semget(ftok("semfile", 1), 1, 0666 | IPC_CREAT);
    semctl(semid, 0, SETVAL, (union semun){1});

    pthread_t t[3]; int id[3] = {1,2,3};
    for (int i = 0; i < 3; i++) pthread_create(&t[i], NULL, run, &id[i]);
    for (int i = 0; i < 3; i++) pthread_join(t[i], NULL);

    semctl(semid, 0, IPC_RMID);
    return 0;
}

/* 1st use " touch semfile " to compile & run */

```
## Create a program with two processes that perform semaphore operations (wait and signal) on a semaphore set using semop.

```c
#include <stdio.h>
#include <sys/ipc.h>
#include <sys/sem.h>
#include <unistd.h>
#include <sys/wait.h>

union semun { int val; };
void wait_sem(int id)  { struct sembuf op = {0, -1, 0}; semop(id, &op, 1); }
void signal_sem(int id){ struct sembuf op = {0,  1, 0}; semop(id, &op, 1); }

int main() {
    int semid = semget(ftok("semfile", 1), 1, 0666 | IPC_CREAT);
    semctl(semid, 0, SETVAL, (union semun){1});

    if (fork() == 0) {
        wait_sem(semid);
        printf("Child in critical section\n");
        sleep(2);
        printf("Child out\n");
        signal_sem(semid);
    } else {
        sleep(1);
        wait_sem(semid);
        printf("Parent in critical section\n");
        signal_sem(semid);
        wait(NULL);
        semctl(semid, 0, IPC_RMID);
    }
    return 0;
}

/* 1st use " touch semfile " to compile & run */

```
## Write a program that performs control operations on the semaphore set using semctl.

```c
#include <stdio.h>
#include <sys/ipc.h>
#include <sys/sem.h>

union semun { int val; };

int main() {
    key_t key = ftok("semfile", 1);
    int semid = semget(key, 1, 0666 | IPC_CREAT);

    union semun u;
    u.val = 2;                          // Set initial value
    semctl(semid, 0, SETVAL, u);        // SETVAL

    int val = semctl(semid, 0, GETVAL); // GETVAL
    printf("Semaphore value: %d\n", val);

    semctl(semid, 0, IPC_RMID);         // Remove semaphore set
    return 0;
}

/* 1st use " touch semfile " to compile & run */

```
## Develop a C program that implements a simple dining philosophers problem using semaphores for synchronization.

```c
#include <stdio.h>
#include <sys/ipc.h>
#include <sys/sem.h>
#include <unistd.h>
#include <stdlib.h>
#include <sys/wait.h>

#define N 5  // Number of philosophers

union semun { int val; };
int semid;

void wait_fork(int i)  { struct sembuf op = {i, -1, 0}; semop(semid, &op, 1); }
void signal_fork(int i){ struct sembuf op = {i,  1, 0}; semop(semid, &op, 1); }

void philosopher(int id) {
    int left = id;
    int right = (id + 1) % N;

    wait_fork(left);
    wait_fork(right);

    printf("Philosopher %d is eating\n", id);
    sleep(1);

    signal_fork(left);
    signal_fork(right);

    printf("Philosopher %d is thinking\n", id);
    exit(0);
}

int main() {
    semid = semget(IPC_PRIVATE, N, 0666 | IPC_CREAT);
    union semun u = {1};
    for (int i = 0; i < N; i++) semctl(semid, i, SETVAL, u);

    for (int i = 0; i < N; i++)
        if (fork() == 0) philosopher(i);

    while (wait(NULL) > 0);
    semctl(semid, 0, IPC_RMID);
    return 0;
}

```
## Write a program where multiple processes compete for access to a critical section using semaphores to ensure mutual exclusion.

```c
#include <stdio.h>
#include <sys/ipc.h>
#include <sys/sem.h>
#include <unistd.h>
#include <sys/wait.h>

union semun { int val; };
void P(int id) { struct sembuf b = {0, -1, 0}; semop(id, &b, 1); }
void V(int id) { struct sembuf b = {0,  1, 0}; semop(id, &b, 1); }

int main() {
    int semid = semget(ftok("semfile", 1), 1, 0666 | IPC_CREAT);
    semctl(semid, 0, SETVAL, (union semun){1});

    for (int i = 0; i < 3; i++) {
        if (fork() == 0) {
            P(semid);
            printf("Process %d in critical section\n", getpid());
            sleep(1);
            printf("Process %d leaving\n", getpid());
            V(semid);
            return 0;
        }
    }

    while (wait(NULL) > 0);
    semctl(semid, 0, IPC_RMID);
    return 0;
}

```
## Write a C program to create a pipe and pass an array of integers from the parent process to the child process through the pipe.

```c
#include <stdio.h>
#include <unistd.h>
#include <sys/wait.h>

int main() {
    int pipefd[2], arr[] = {10, 20, 30, 40, 50};
    pipe(pipefd);

    if (fork() == 0) {
        close(pipefd[1]);  // Close write end
        int recv[5];
        read(pipefd[0], recv, sizeof(recv));
        close(pipefd[0]);
        for (int i = 0; i < 5; i++)
            printf("Child received: %d\n", recv[i]);
    } else {
        close(pipefd[0]);  // Close read end
        write(pipefd[1], arr, sizeof(arr));
        close(pipefd[1]);
        wait(NULL);
    }
    return 0;
}

```
## Implement a program where multiple child processes are created, and each child process communicates with the parent process using pipes.

```c
#include <stdio.h>
#include <unistd.h>
#include <sys/wait.h>
#include <string.h>

int main() {
    int n = 3;
    for (int i = 0; i < n; i++) {
        int pipefd[2];
        pipe(pipefd);

        if (fork() == 0) {
            close(pipefd[0]);  // Child closes read end
            char msg[32];
            sprintf(msg, "Hello from child %d", i + 1);
            write(pipefd[1], msg, strlen(msg) + 1);
            close(pipefd[1]);
            return 0;
        } else {
            close(pipefd[1]);  // Parent closes write end
            char buf[32];
            read(pipefd[0], buf, sizeof(buf));
            printf("Parent received: %s\n", buf);
            close(pipefd[0]);
            wait(NULL);
        }
    }
    return 0;
}

```
## Develop a program that uses pipes for bidirectional communication between two processes, where each process can send and receive messages.

```c
#include <stdio.h>
#include <unistd.h>
#include <string.h>
#include <sys/wait.h>

int main() {
    int p2c[2], c2p[2];
    pipe(p2c); pipe(c2p);

    if (fork() == 0) {
        char buf[20];
        close(p2c[1]); close(c2p[0]);
        read(p2c[0], buf, sizeof(buf));
        printf("Child got: %s\n", buf);
        write(c2p[1], "Hello", 6);
    } else {
        char reply[20];
        close(p2c[0]); close(c2p[1]);
        write(p2c[1], "Hi", 3);
        read(c2p[0], reply, sizeof(reply));
        printf("Parent got: %s\n", reply);
        wait(NULL);
    }
    return 0;
}

```
## Create a C program where multiple processes write data to a named pipe, and another process reads from the named pipe and displays the received data.

```c
#include <stdio.h>
#include <unistd.h>
#include <fcntl.h>
#include <sys/stat.h>
#include <string.h>
#include <sys/wait.h>

int main() {
    char *fifo = "myfifo";
    mkfifo(fifo, 0666);

    for (int i = 0; i < 3; i++) {
        if (fork() == 0) {
            int fd = open(fifo, O_WRONLY);
            char msg[30];
            sprintf(msg, "Message from child %d\n", i + 1);
            write(fd, msg, strlen(msg));
            close(fd);
            return 0;
        }
    }

    int fd = open(fifo, O_RDONLY);
    char buf[128];
    int n = read(fd, buf, sizeof(buf));
    write(1, buf, n);
    close(fd);

    while (wait(NULL) > 0);
    unlink(fifo);
    return 0;
}

```
## Develop a C program that acts as a server, continuously reading requests from a named pipe, and a client program that sends requests to the server through the same named pipe

```c
#include <stdio.h>
#include <unistd.h>
#include <fcntl.h>
#include <sys/stat.h>
#include <string.h>

int main() {
    char *fifo = "myfifo";
    mkfifo(fifo, 0666);

    if (fork() == 0) {
        char msg[50];
        while (1) {
            int fd = open(fifo, O_RDONLY);
            read(fd, msg, sizeof(msg));
            close(fd);
            if (strncmp(msg, "exit", 4) == 0) break;
            printf("Child got: %s", msg);
        }
    } else {
        char input[50];
        while (1) {
            printf("Parent: ");
            fgets(input, sizeof(input), stdin);
            int fd = open(fifo, O_WRONLY);
            write(fd, input, strlen(input));
            close(fd);
            if (strncmp(input, "exit", 4) == 0) break;
        }
    }

    unlink(fifo);
    return 0;
}

```
## Develop a program where multiple processes read from and write to a shared memory segment, synchronizing their access using semaphores.

```c
#include <stdio.h>
#include <sys/ipc.h>
#include <sys/shm.h>
#include <sys/sem.h>
#include <unistd.h>
#include <stdlib.h>
#include <sys/wait.h>

union semun { int val; };
void P(int id) { struct sembuf op = {0, -1, 0}; semop(id, &op, 1); }
void V(int id) { struct sembuf op = {0,  1, 0}; semop(id, &op, 1); }

int main() {
    int shmid = shmget(IPC_PRIVATE, sizeof(int), 0666 | IPC_CREAT);
    int *data = (int *)shmat(shmid, NULL, 0);
    *data = 0;

    int semid = semget(IPC_PRIVATE, 1, 0666 | IPC_CREAT);
    semctl(semid, 0, SETVAL, (union semun){1});

    for (int i = 0; i < 3; i++) {
        if (fork() == 0) {
            P(semid);
            (*data)++;
            printf("Child %d incremented: %d\n", i + 1, *data);
            V(semid);
            shmdt(data);
            return 0;
        }
    }

    while (wait(NULL) > 0);
    printf("Final value: %d\n", *data);
    shmdt(data);
    shmctl(shmid, IPC_RMID, NULL);
    semctl(semid, 0, IPC_RMID);
    return 0;
}

```
## Create a C program where the parent process creates a shared memory segment, writes data into it, and waits for the child process to read the data from the shared memory.

```c
#include <stdio.h>
#include <sys/ipc.h>
#include <sys/shm.h>
#include <unistd.h>
#include <string.h>
#include <sys/wait.h>

int main() {
    key_t key = ftok("shmfile", 65);
    int shmid = shmget(key, 1024, 0666 | IPC_CREAT);
    char *str = (char *)shmat(shmid, NULL, 0);

    if (fork() == 0) {
        sleep(1);  // Wait for parent to write
        printf("Child read: %s\n", str);
        shmdt(str);
    } else {
        strcpy(str, "Hello from parent");
        printf("Parent wrote: %s\n", str);
        wait(NULL);
        shmdt(str);
        shmctl(shmid, IPC_RMID, NULL);  // Cleanup
    }
    return 0;
}

```
## Write a program where multiple processes cooperate to calculate the sum of a large array of numbers stored in a shared memory segment.

```c
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>
#include <sys/ipc.h>
#include <sys/shm.h>
#include <sys/sem.h>
#include <sys/wait.h>

union semun { int val; };
void P(int s) { struct sembuf b = {0, -1, 0}; semop(s, &b, 1); }
void V(int s) { struct sembuf b = {0,  1, 0}; semop(s, &b, 1); }

int main() {
    int size = 100, procs = 4;
    int shmid = shmget(IPC_PRIVATE, (size + 1) * sizeof(int), 0666 | IPC_CREAT);
    int *mem = shmat(shmid, NULL, 0);
    int *arr = mem, *sum = &mem[size]; *sum = 0;

    for (int i = 0; i < size; i++) arr[i] = i + 1;

    int semid = semget(IPC_PRIVATE, 1, 0666 | IPC_CREAT);
    semctl(semid, 0, SETVAL, (union semun){1});

    for (int i = 0; i < procs; i++)
        if (fork() == 0) {
            int start = i * (size / procs);
            int end = (i == procs - 1) ? size : start + (size / procs);
            int part = 0;
            for (int j = start; j < end; j++) part += arr[j];
            P(semid); *sum += part; V(semid);
            return 0;
        }

    while (wait(NULL) > 0);
    printf("Total sum: %d\n", *sum);
    shmdt(mem); shmctl(shmid, IPC_RMID, 0); semctl(semid, 0, IPC_RMID);
    return 0;
}

```
## Write a program where two processes communicate through a message queue. One process sends a message containing a string to the other process, which receives and prints the message.

```c
#include <stdio.h>
#include <stdlib.h>
#include <sys/ipc.h>
#include <sys/msg.h>
#include <unistd.h>
#include <string.h>
#include <sys/wait.h>

struct msgbuf {
    long mtype;
    char mtext[100];
};

int main() {
    key_t key = ftok("msgfile", 65);
    int msgid = msgget(key, 0666 | IPC_CREAT);
    if (msgid < 0) {
        perror("msgget");
        return 1;
    }

    if (fork() == 0) {
        // Child: receive message
        struct msgbuf rmsg;
        msgrcv(msgid, &rmsg, sizeof(rmsg.mtext), 1, 0);
        printf("Child received: %s\n", rmsg.mtext);
    } else {
        // Parent: send message
        struct msgbuf smsg;
        smsg.mtype = 1;
        strcpy(smsg.mtext, "Hello from parent");
        msgsnd(msgid, &smsg, strlen(smsg.mtext) + 1, 0);
        wait(NULL);
        msgctl(msgid, IPC_RMID, NULL);  // Remove message queue
    }

    return 0;
}

```
## Create a program where multiple processes coordinate access to a shared resource using semaphores(system v). Simulate a scenario where processes request and release access to the resource with proper synchronization

```c
#include <stdio.h>
#include <unistd.h>
#include <sys/wait.h>
#include <sys/ipc.h>
#include <sys/sem.h>

union semun { int val; };
void wait_sem(int id)  { struct sembuf op = {0, -1, 0}; semop(id, &op, 1); }
void signal_sem(int id){ struct sembuf op = {0,  1, 0}; semop(id, &op, 1); }

int main() {
    key_t key = ftok("semfile", 1);
    int semid = semget(key, 1, 0666 | IPC_CREAT);
    semctl(semid, 0, SETVAL, (union semun){1});  // Binary semaphore

    for (int i = 0; i < 3; i++) {
        if (fork() == 0) {
            wait_sem(semid);
            printf("Process %d accessing shared resource\n", getpid());
            sleep(1);
            printf("Process %d done\n", getpid());
            signal_sem(semid);
            return 0;
        }
    }

    while (wait(NULL) > 0);
    semctl(semid, 0, IPC_RMID);
    return 0;
}

```
## Develop a program where two processes share an integer value stored in a shared memory segment. One process increments the value and the other process decrements it, with proper synchronization.

```c
#include <stdio.h>
#include <unistd.h>
#include <sys/ipc.h>
#include <sys/shm.h>
#include <sys/sem.h>
#include <sys/wait.h>

union semun { int val; };
void P(int s) { struct sembuf b = {0, -1, 0}; semop(s, &b, 1); }
void V(int s) { struct sembuf b = {0,  1, 0}; semop(s, &b, 1); }

int main() {
    int shmid = shmget(IPC_PRIVATE, sizeof(int), 0666 | IPC_CREAT);
    int *val = shmat(shmid, NULL, 0);
    *val = 0;

    int semid = semget(IPC_PRIVATE, 1, 0666 | IPC_CREAT);
    semctl(semid, 0, SETVAL, (union semun){1});

    if (fork() == 0) {
        for (int i = 0; i < 5; i++) { P(semid); (*val)++; printf("Child: %d\n", *val); V(semid); }
    } else {
        for (int i = 0; i < 5; i++) { P(semid); (*val)--; printf("Parent: %d\n", *val); V(semid); }
        wait(NULL);
        shmdt(val); shmctl(shmid, IPC_RMID, 0); semctl(semid, 0, IPC_RMID);
    }
    return 0;
}

```
