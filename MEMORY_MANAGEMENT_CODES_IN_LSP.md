## Write a C program to demonstrate dynamic memory allocation using malloc().

```c
#include <stdio.h>
#include <stdlib.h>

int main() {
    int *ptr;
    ptr = (int *)malloc(4*4); // allocate / updating memory for 4 integers

    if (ptr == NULL) 
    {
        printf("Memory not allocated.\n");
        return 1;
    }

     printf("Values using malloc:");
     for (int i = 0; i < 4; i++)
     {
      ptr[i] = i + 1; 
      printf("%d ", ptr[i]);// values are not initialized, you must assign
    }

    free(ptr);  // Always free allocated memory
return 0;
}
        ( OR )

#include <stdio.h>
#include <stdlib.h>

int main() {
    int *ptr;
    ptr = (int *)malloc(4); // allocate memory for 4 integers

    if (ptr == NULL) 
    {
        printf("Memory not allocated.\n");
        return 1;
    }

     printf("Values using malloc:");
     for (int i = 0; i < 4; i++)
     {
      ptr[i] = i + 1; 
      printf("%d ", ptr[i]);// values are not initialized, you must assign
    }

    free(ptr);  // Always free allocated memory
return 0;
}
```

## Implement a C program to allocate memory for an array dynamically using calloc().

```c
#include <stdio.h>
#include <stdlib.h>

int main() {
    int *ptr;
    ptr = (int *)calloc(4*4); // allocate / updating memory for 4 integers

    if (ptr == NULL) 
    {
        printf("Memory not allocated.\n");
        return 1;
    }

     printf("Values using calloc:");
     for (int i = 0; i < 4; i++)
     {
      ptr[i] = i + 1; 
      printf("%d ", ptr[i]);// values are not initialized, you must assign
    }

    free(ptr);  // Always free allocated memory
return 0;
}
```

## Write a C program to resize dynamically allocated memory using realloc().

```c
#include <stdio.h>
#include <stdlib.h>

int main() {
    int *ptr = realloc(NULL, 4 *sizeof(int)); // realloc works like malloc if first arg is NULL
    
 /* int *ptr = malloc(4*sizeof(int)); (or) int *ptr = calloc(4,sizeof(int)); */
 
    if (ptr == NULL) {
        printf("Memory allocation failed.\n");
        return 1;
    }

    for (int i = 0; i < 8; i++) {
        ptr[i] = (i + 1);
        printf("%d ", ptr[i]);
    }

    free(ptr);
    return 0;
}
```

## Develop a program in C to allocate memory for a linked list node dynamically.

```c
#include <stdio.h>
#include <stdlib.h>
struct Node {
    int data;
    struct Node *next;
};
void printList(struct Node *head) {
    while (head != NULL) {
        printf("%d -> ", head->data);
        head = head->next;
    }
    printf("NULL\n");
}
int main() {
    struct Node *head = NULL, *temp, *newNode;
    // Insert at beginning
    newNode = (struct Node *)malloc(sizeof(struct Node));
    newNode->data = 10;
    newNode->next = head;
    head = newNode;
    // Insert at end
    newNode = (struct Node *)malloc(sizeof(struct Node));
    newNode->data = 30;
    newNode->next = NULL;
    temp = head;
    while (temp->next != NULL) temp = temp->next;
    temp->next = newNode;
    // Insert in the middle (after first node)
    newNode = (struct Node *)malloc(sizeof(struct Node));
    newNode->data = 20;
    newNode->next = head->next;
    head->next = newNode;
    // Print final list
    printList(head);
    return 0;
}
```

## Implement a C program to simulate memory allocation using the first-fit algorithm.

```c
#include <stdio.h>
#include <stdlib.h>
#define MAX 10

int main() {
    int blockSize[MAX], processSize[MAX], blockAllocated[MAX];
    int i, j, blocks, processes;

    // Input number of blocks and their sizes
    printf("Enter number of memory blocks: ");
    scanf("%d", &blocks);
    printf("Enter sizes of %d memory blocks:\n", blocks);
    for(i = 0; i < blocks; i++) {
        printf("Block %d: ", i+1);
        scanf("%d", &blockSize[i]);
        blockAllocated[i] = 0; // 0 means block is free
    }
    // Input number of processes and their sizes
    printf("\nEnter number of processes: ");
    scanf("%d", &processes);
    for(i = 0; i < processes; i++) {
        printf("Enter size of process %d: ", i+1);
        scanf("%d", &processSize[i]);

        // First Fit Allocation
        for(j = 0; j < blocks; j++) {
            if(blockAllocated[j] == 0 && blockSize[j] >= processSize[i]) {
                printf("Process %d allocated to Block %d\n", i+1, j+1);
                blockAllocated[j] = 1; // Mark block as allocated
                break;
            }
        }

        if(j == blocks) {
            printf("Process %d cannot be allocated (No suitable block)\n", i+1);
        }
    }

    return 0;
}
```
## Write a C program to simulate memory allocation using the best-fit algorithm.

```c
#include <stdio.h>
#include <stdlib.h>

int main() {
    int blockSize[10], processSize[10], blockUsed[10];
    int blocks, processes;
    // Input number of blocks and their sizes
    printf("How many memory blocks? ");
    scanf("%d", &blocks);
    for(int i = 0; i < blocks; i++) {
        printf("Enter size of block %d: ", i + 1);
        scanf("%d", &blockSize[i]);
        blockUsed[i] = 0; // All blocks are free initially
    }
    // Input number of processes and their sizes
    printf("\nHow many processes? ");
    scanf("%d", &processes);
    for(int i = 0; i < processes; i++) {
        printf("Enter size of process %d: ", i + 1);
        scanf("%d", &processSize[i]);
    }
    // Best-Fit Allocation
    printf("\n--- Best Fit Allocation ---\n");
    for(int i = 0; i < processes; i++) {
        int best = -1;
        for(int j = 0; j < blocks; j++) {
            if(!blockUsed[j] && blockSize[j] >= processSize[i]) {
                if(best == -1 || blockSize[j] < blockSize[best]) {
                    best = j;
                }
            }
        }
        if(best != -1) {
            printf("Process %d is placed in Block %d\n", i + 1, best + 1);
            blockUsed[best] = 1; // Mark block as used
        } else {
            printf("Process %d can't be placed\n", i + 1);
        }
    }
    return 0;
}
```

## Develop a C program to simulate memory allocation using the worst-fit algorithm.

```c
#include <stdio.h>
#include<stdlib.h>

int main() {
    int blocks[5] = {100, 500, 200, 300, 600};
    int processes[3] = {212, 417, 112};
    int allocation[3] = {-1, -1, -1};
    for (int i = 0; i < 3; i++) 
    {
        int worst = -1;
        for (int j = 0; j < 5; j++) {
            if (blocks[j] >= processes[i]) {
                if (worst == -1 || blocks[j] > blocks[worst])
                    worst = j;
            }
        }
        if (worst != -1) {
            allocation[i] = worst;
            blocks[worst] -= processes[i];
        }
    }
    for (int i = 0; i < 3; i++) {
        printf("Process %d of size %d --> ", i + 1, processes[i]);
        if (allocation[i] != -1)
            printf("Allocated to Block %d\n", allocation[i] + 1);
        else
            printf("Not Allocated\n");
    }
    return 0;
}
```

## Implement a C program to simulate memory allocation using the next-fit algorithm.

```c
#include <stdio.h>

#define BLOCKS 8
#define PROCESSES 5

int main() {
    int blockSize[BLOCKS] = {100, 500, 200, 300, 600, 350, 700, 250};
    int processSize[PROCESSES] = {212, 417, 112, 426, 100};
    int allocation[PROCESSES];

    int lastAllocIndex = 0;

    for (int i = 0; i < PROCESSES; i++) {
        int allocated = 0;
        for (int j = 0; j < BLOCKS; j++) {
            int index = (lastAllocIndex + j) % BLOCKS;
            if (blockSize[index] >= processSize[i]) {
                allocation[i] = index;
                blockSize[index] -= processSize[i];
                lastAllocIndex = index;
                allocated = 1;
                break;
            }
        }
        if (!allocated)
            allocation[i] = -1;
    }

    printf("Process No.\tProcess Size\tBlock No.\n");
    for (int i = 0; i < PROCESSES; i++) {
        printf("%d\t\t%d\t\t", i + 1, processSize[i]);
        if (allocation[i] != -1)
            printf("%d\n", allocation[i] + 1);
        else
            printf("Not Allocated\n");
    }

    return 0;
}
```

## Write a C program to implement a simple memory allocator using the buddy system.

```c
#include <stdio.h>
#include<stdlib.h>

int next_Power(int size) {
int power = 1;
while (power < size) power *= 2;
return power;
}
void aloc_Bud(int size) {
int blockSize = next_Power(size);
printf("Allocated block of size %d bytes (buddy system)\n", blockSize);
}
int main() {
aloc_Bud(8);
aloc_Bud(64); 
return 0;
}
```

## Develop a C program to implement a memory allocator using a custom memory management algorithm.

```c
#include <stdio.h>
int memory[10] = {0};  // 0 = free, 1 = used
void allocate(int size) {
    for (int i = 0; i <= 10 - size; i++) {
        int fit = 1;
        for (int j = 0; j < size; j++)
            if (memory[i + j]) fit = 0;
        if (fit) {
            for (int j = 0; j < size; j++)
                memory[i + j] = 1;
            printf("Allocated %d units at %d\n", size, i);
            return;
        }
    }
    printf("Allocation failed for %d units\n", size);
}
void show() {
    for (int i = 0; i < 10; i++) printf("%d ", memory[i]);
    printf("\n");
}
int main() {
    show();
    allocate(3);
    allocate(4);
    show();
    return 0;
}
```

## Write a C program to demonstrate memory mapping using mmap().

```c
#include <stdio.h>
#include <stdlib.h>

int main() {
    FILE *tmp = tmpfile();  // Create a temporary file

    if (tmp == NULL) {
        perror("tmpfile failed");
        return 1;
    }

    fputs("Temporary data written to file.\n", tmp);
    rewind(tmp);  // Go back to the beginning

    char buffer[100];
    fgets(buffer, sizeof(buffer), tmp);  // Read back the data

    printf("Read from temp file: %s", buffer);

    // No need to manually delete — temp file auto-deleted when closed
    fclose(tmp);
    return 0;
}
```

## Implement a C program to read from and write to a memory-mapped file.

```c
#include <stdio.h>
#include <fcntl.h>
#include <sys/mman.h>
#include <string.h>
#include <unistd.h>
#include <sys/stat.h>

int main() {
    int fd = open("test.txt", O_RDWR | O_CREAT, 0666);
    ftruncate(fd, 100);  // Set file size
    char *data = mmap(NULL, 100, PROT_READ | PROT_WRITE, MAP_SHARED, fd, 0);
    if (data == MAP_FAILED) {
        perror("mmap");
        return 1;
    }
    strcpy(data, "Hello from mmap!");
    printf("File content written using mmap.\n");
    munmap(data, 100);
    close(fd);
    return 0;
}
```

## Develop a C program to demonstrate shared memory usage using shmget() and shmat().

```c
#include <stdio.h>
#include <sys/ipc.h>
#include <sys/shm.h>
#include <string.h>

int main() {
    key_t key = ftok("shmfile", 65); // generate unique key
    int shmid = shmget(key, 1024, 0666 | IPC_CREAT); // create shared memory
    char *str = (char *) shmat(shmid, NULL, 0); // attach

    strcpy(str, "Shared memory message");
    printf("Data written to shared memory: %s\n", str);

    shmdt(str); // detach
    return 0;
}
```

## Write a C program to create a shared memory segment and synchronize access using semaphores.

```c
#include <stdio.h>
#include <stdlib.h>
#include <sys/ipc.h>
#include <sys/shm.h>
#include <sys/sem.h>
#include <unistd.h>
#include <string.h>
#include <sys/wait.h>

int main() {
    key_t key = 5678;
    int shmid = shmget(key, 100, IPC_CREAT | 0666);
    int semid = semget(key, 1, IPC_CREAT | 0666);

    semctl(semid, 0, SETVAL, 1);  // Semaphore value set to 1
    char *msg = (char *)shmat(shmid, NULL, 0);

    struct sembuf lock = {0, -1, 0};  // Lock
    struct sembuf unlock = {0, 1, 0}; // Unlock

    if (fork() == 0) {
        // Child process
        sleep(1);  // Let parent write first
        semop(semid, &lock, 1);       // Wait
        printf("Child: %s\n", msg);   // Read
        semop(semid, &unlock, 1);     // Release
        shmdt(msg);
        exit(0);
    } else {
        // Parent process
        semop(semid, &lock, 1);       // Lock
        strcpy(msg, "Message from Parent!");
        printf("Parent: Message written.\n");
        semop(semid, &unlock, 1);     // Unlock

        wait(NULL);  // Wait for child
        shmdt(msg);                  // Detach
        shmctl(shmid, IPC_RMID, NULL); // Remove shared memory
        semctl(semid, 0, IPC_RMID);    // Remove semaphore
    }

    return 0;
}
```
