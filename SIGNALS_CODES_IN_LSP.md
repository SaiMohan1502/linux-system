## Write a C program to catch and handle the SIGINT signal.

```c
#include <stdio.h>
#include <signal.h>
#include <stdlib.h>
#include <unistd.h>

void handle_sigint(int sig) {
    printf("\nCaught signal %d (SIGINT). Exiting gracefully.\n", sig);
    exit(0);
}

int main() {
    signal(SIGINT, handle_sigint);
    printf("Running program... Press Ctrl+C to send SIGINT.\n");
    while (1) {
        sleep(1);
    }
    return 0;
}

```
## Implement a C program to send a custom signal to another process.

```c
#include <stdio.h>
#include <stdlib.h>
#include <signal.h>
#include <unistd.h>

int main() {
    pid_t pid;
    printf("Enter the PID of the process to send SIGUSR1: ");
    scanf("%d", &pid);

    if (kill(pid, SIGUSR1) == 0) {
        printf("SIGUSR1 sent to process %d successfully.\n", pid);
    } else {
        perror("Failed to send signal");
    }

    return 0;
}

```
## Create a C program to ignore the SIGCHLD signal temporarily.

```c
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>
#include <signal.h>

int main() {
    signal(SIGCHLD, SIG_IGN);
    if (fork() == 0) exit(0);
    sleep(1);
    printf("SIGCHLD ignored\n");

    signal(SIGCHLD, SIG_DFL);
    if (fork() == 0) exit(0);
    sleep(1);
    printf("SIGCHLD restored\n");

    return 0;
}

```
## Write a program to block the SIGTERM signal using sigprocmask().

```c
#include <stdio.h>
#include <stdlib.h>
#include <signal.h>
#include <unistd.h>

int main() {
    sigset_t block_set;
    sigemptyset(&block_set);
    sigaddset(&block_set, SIGTERM);

    sigprocmask(SIG_BLOCK, &block_set, NULL);
    printf("SIGTERM is now blocked. Try sending SIGTERM (kill -15 %d)\n", getpid());

    sleep(10);

    printf("Exiting program. SIGTERM was blocked during sleep.\n");
    return 0;
}

```
## Implement a C program to handle the SIGALRM signal using sigaction().

```c
#include <stdio.h>
#include <stdlib.h>
#include <signal.h>
#include <unistd.h>

void alarm_handler(int sig) {
    printf("Received SIGALRM (signal %d)\n", sig);
}

int main() {
    struct sigaction sa;
    sa.sa_handler = alarm_handler;
    sigemptyset(&sa.sa_mask);
    sa.sa_flags = 0;

    sigaction(SIGALRM, &sa, NULL);

    alarm(3);  // Set alarm to go off in 3 seconds
    printf("Waiting for SIGALRM...\n");

    pause();   // Wait for a signal

    return 0;
}

```
## Write a C program to install a custom signal handler for SIGTERM?

```c
#include <stdio.h>
#include <stdlib.h>
#include <signal.h>
#include <unistd.h>

void handle_sigterm(int sig) {
    printf("Caught SIGTERM (signal %d). Exiting safely.\n", sig);
    exit(0);
}

int main() {
    signal(SIGTERM, handle_sigterm);

    printf("Process PID: %d\n", getpid());
    printf("Waiting for SIGTERM...\n");

    while (1) {
        sleep(1);
    }

    return 0;
}

```
## Implement a program to handle the SIGSEGV signal (segmentation fault).

```c
#include <stdio.h>
#include <stdlib.h>
#include <signal.h>
#include <unistd.h>

void handle_sigsegv(int sig) {
    printf("Caught SIGSEGV (signal %d). Segmentation fault handled.\n", sig);
    exit(1);
}

int main() {
    signal(SIGSEGV, handle_sigsegv);

    printf("Triggering segmentation fault...\n");

    int *ptr = NULL;
    *ptr = 10;  // This will cause a segmentation fault

    return 0;
}

```
## Create a program to handle the SIGILL signal (illegal instruction).

```c
#include <stdio.h>
#include <stdlib.h>
#include <signal.h>
#include <unistd.h>

void handle_sigill(int sig) {
    printf("Caught SIGILL (signal %d). Illegal instruction handled.\n", sig);
    exit(1);
}

int main() {
    signal(SIGILL, handle_sigill);

    printf("Triggering illegal instruction...\n");

    // Use inline assembly to generate an illegal instruction
    __asm__("ud2");  // 'ud2' is guaranteed to be undefined on x86/x86_64

    return 0;
}

```
## Write a program to handle the SIGABRT signal (abort).

```c
#include <stdio.h>
#include <stdlib.h>
#include <signal.h>
#include <unistd.h>

void handle_sigabrt(int sig) {
    printf("Caught SIGABRT (signal %d). Abort signal handled.\n", sig);
    exit(1);
}

int main() {
    signal(SIGABRT, handle_sigabrt);

    printf("Triggering SIGABRT using abort()...\n");
    abort();  // Sends SIGABRT to this process

    return 0;
}

```
## Implement a C program to handle the SIGQUIT signal.

```c
#include <stdio.h>
#include <stdlib.h>
#include <signal.h>
#include <unistd.h>

void handle_sigquit(int sig) {
    printf("Caught SIGQUIT (signal %d). Quitting gracefully.\n", sig);
    exit(0);
}

int main() {
    signal(SIGQUIT, handle_sigquit);

    printf("Process PID: %d\n", getpid());
    printf("Waiting for SIGQUIT...\n");

    while (1) {
        sleep(1);
    }

    return 0;
}

// to quit the signal we have to use " ctrl+\ " in compiler //

```
## Write a program to handle the SIGTERM signal (termination request).

```c
#include <stdio.h>
#include <stdlib.h>
#include <signal.h>
#include <unistd.h>

void handle_sigterm(int sig) {
    printf("Caught SIGTERM (signal %d). Terminating gracefully.\n", sig);
    exit(0);
}

int main() {
    signal(SIGTERM, handle_sigterm);

    printf("Process PID: %d\n", getpid());
    printf("Waiting for SIGTERM...\n");

    while (1) {
        sleep(1);
    }

    return 0;
}

// after entering " ./a.out ", open another terminal and enter cmd " kill -15 <enter pid > " then the signal will be terminated // 

```
## Write a program to handle the SIGTSTP signal (terminal stop).

```c
#include <stdio.h>
#include <stdlib.h>
#include <signal.h>
#include <unistd.h>

void handle_sigtstp(int sig) {
    printf("Caught SIGTSTP (signal %d). Ignoring terminal stop.\n", sig);
}

int main() {
    signal(SIGTSTP, handle_sigtstp);

    printf("Process PID: %d\n", getpid());
    printf("Press Ctrl+Z to send SIGTSTP...\n");

    while (1) {
        sleep(1);
    }

    return 0;
}

//after entering " ./a.out ", open another terminal and enter cmd " kill -15 <enter pid > " then the signal will be terminated //

```
## Write a program to handle the SIGVTALRM signal (virtual timer expired).

```c
#include <stdio.h>
#include <signal.h>
#include <sys/time.h>
#include <unistd.h>

void handler(int sig) {
    printf("Caught signal %d (SIGVTALRM)\n", sig);
    _exit(0);
}

int main() {
    signal(SIGVTALRM, handler);

    struct itimerval t = {{0,0}, {1,0}};
    setitimer(ITIMER_VIRTUAL, &t, NULL);

    while (1);  // Keep CPU busy
}

```
## Write a program to handle the SIGWINCH signal (window size change).

```c
#include <stdio.h>
#include <signal.h>
#include <unistd.h>
#include <sys/ioctl.h>

void handle_sigwinch(int sig) {
    struct winsize w;
    ioctl(STDOUT_FILENO, TIOCGWINSZ, &w);
    printf("Caught SIGWINCH (signal %d)\n", sig);
    printf("New terminal size: rows = %d, cols = %d\n", w.ws_row, w.ws_col);
}

int main() {
    signal(SIGWINCH, handle_sigwinch);

    printf("Resize the terminal window to trigger SIGWINCH.\n");

    while (1) {
        sleep(1);
    }

    return 0;
}

```
## Implement a C program to handle the SIGXFSZ signal (file size limit exceeded).

```c
#include <stdio.h>
#include <signal.h>
#include <unistd.h>
#include <fcntl.h>
#include <sys/resource.h>

void handler(int sig) {
    printf("Caught signal %d (SIGXFSZ)\n", sig);
    _exit(1);
}

int main() {
    signal(SIGXFSZ, handler);

    struct rlimit lim = {1024, 1024};
    setrlimit(RLIMIT_FSIZE, &lim);

    int fd = open("out.txt", O_WRONLY | O_CREAT | O_TRUNC, 0644);
    char buf[512];
    for (int i = 0; i < 3; i++) write(fd, buf, sizeof(buf));

    return 0;
}

```
## Create a program to handle the SIGPWR signal (power failure restart).

```c
#include <stdio.h>
#include <stdlib.h>
#include <signal.h>
#include <unistd.h>

void handle_sigpwr(int sig) {
    printf("Caught SIGPWR (signal %d). Power failure signal handled.\n", sig);
    exit(0);
}

int main() {
    signal(SIGPWR, handle_sigpwr);

    printf("Process PID: %d\n", getpid());
    printf("Waiting for SIGPWR (send using: kill -30 <PID>)\n");

    while (1) {
        sleep(1);
    }

    return 0;
}

// after entering " ./a.out ", open another terminal and enter cmd " kill -30 < pid > " then the popwer failure signal will be caught//

```
## Write a program to send a signal to itself (same process)?

```c
#include <stdio.h>
#include <stdlib.h>
#include <signal.h>
#include <unistd.h>

void handler(int sig) {
    printf("Caught signal %d\n", sig);
}

int main() {
    signal(SIGUSR1, handler);  // Set handler for SIGUSR1

    pid_t pid = getpid();      // Get current process ID
    printf("Sending SIGUSR1 to self (PID: %d)\n", pid);

    kill(pid, SIGUSR1);        // Send SIGUSR1 to itself

    return 0;
}

```
## Write a program to handle the SIGSYS signal (bad system call).

```c
#include <stdio.h>
#include <stdlib.h>
#include <signal.h>
#include <unistd.h>

void handle_sigsys(int sig) {
    printf("Caught SIGSYS (signal %d). Bad system call detected.\n", sig);
    exit(1);
}

int main() {
    signal(SIGSYS, handle_sigsys);

    printf("Triggering bad system call (SIGSYS)...\n");

#if defined(__x86_64__)
    __asm__("int $0x80");  // Unsafe syscall method on 64-bit systems
#else
    raise(SIGSYS);         // Simulate SIGSYS if asm doesn't work
#endif

    return 0;
}

```
## Write a C program to handle the SIGIO signal (I/O is possible on a descriptor).

```c
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>
#include <signal.h>
#include <fcntl.h>

void handle_sigio(int sig) {
    printf("Caught SIGIO (signal %d). Input is ready on stdin.\n", sig);
}

int main() {
    signal(SIGIO, handle_sigio);

    fcntl(STDIN_FILENO, F_SETOWN, getpid());                      // Set owner of fd
    int flags = fcntl(STDIN_FILENO, F_GETFL);                     
    fcntl(STDIN_FILENO, F_SETFL, flags | O_ASYNC | O_NONBLOCK);   // Enable async I/O

    printf("Waiting for input (type and press Enter)...\n");

    while (1) {
        pause();  // Wait for signal
    }

    return 0;
}

```
## Implement a C program to handle the SIGINFO signal (status request from keyboard).

```c
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>
#include <signal.h>

void handle_status(int sig) {
    printf("Caught signal %d (simulating SIGINFO). Status requested.\n", sig);
}

int main() {
    signal(SIGINFO, handle_status);  // use SIGUSR1 or SIGUSR2 in linux // use SIGINFO if ur not using linux

    printf("PID: %d. Send SIGUSR1 using: kill -USR1 %d\n", getpid(), getpid());

    while (1) {
        sleep(1);
    }

    return 0;
}

```
## Create a C program to handle the SIGRTMIN signal (minimum real-time signal).

```c
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>
#include <signal.h>

void handle_rtmin(int sig) {
    printf("Caught SIGRTMIN (signal %d)\n", sig);
    exit(0);
}

int main() {
    signal(SIGRTMIN, handle_rtmin);

    printf("PID: %d\n", getpid());
    printf("Send signal using: kill -%d %d\n", SIGRTMIN, getpid());

    while (1) pause();  // Wait for signal
}

```
## Implement a program to handle the SIGRTMAX signal (maximum real-time signal).

```c
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>
#include <signal.h>

void handle_rtmax(int sig) {
    printf("Caught SIGRTMAX (signal %d)\n", sig);
    exit(0);
}

int main() {
    signal(SIGRTMAX, handle_rtmax);

    printf("PID: %d\n", getpid());
    printf("Send signal using: kill -%d %d\n", SIGRTMAX, getpid());

    while (1) pause();  // Wait for signal
}

```
## Implement a program to handle the SIGUSR1_SIGUSR2 signal (user-defined signal).

```c
#include <stdio.h>
#include <stdlib.h>
#include <signal.h>
#include <unistd.h>

void handle_signal(int sig) {
    if (sig == SIGUSR1)
        printf("Caught SIGUSR1 (signal %d)\n", sig);
    else if (sig == SIGUSR2)
        printf("Caught SIGUSR2 (signal %d)\n", sig);
}

int main() {
    signal(SIGUSR1, handle_signal);
    signal(SIGUSR2, handle_signal);

    printf("PID: %d\n", getpid());
    printf("Send SIGUSR1: kill -USR1 %d\n", getpid());
    printf("Send SIGUSR2: kill -USR2 %d\n", getpid());

    while (1) pause();  // Wait for signals
}

// use kill -USR1 <pid>
   use kill -USR2 <pid> 
   in another terminal then enter " ctrl+z " //

```
## Create a C program to handle the SIGCONT_SIGSTOP signal (continue or stop executing).

```c
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>
#include <signal.h>

void handle_sigcont(int sig) {
    printf("Caught SIGCONT (signal %d): Process resumed.\n", sig);
}

int main() {
    signal(SIGCONT, handle_sigcont);

    printf("PID: %d\n", getpid());
    printf("Send SIGSTOP: kill -STOP %d\n", getpid());
    printf("Send SIGCONT: kill -CONT %d\n", getpid());

    while (1) {
        printf("Running...\n");
        sleep(2);
    }

    return 0;
}

// for stopping the process -> kill -STOP <PID>
   for resuming the process -> kill -CONT <PID> //

```
## Write a program to demonstrate how to block signals using sigprocmask().

```c
#include <stdio.h>
#include <stdlib.h>
#include <signal.h>
#include <unistd.h>

void handle_sigint(int sig) {
    printf("Caught SIGINT (signal %d)\n", sig);
}

int main() {
    sigset_t block_set;

    signal(SIGINT, handle_sigint);

    sigemptyset(&block_set);
    sigaddset(&block_set, SIGINT);

    sigprocmask(SIG_BLOCK, &block_set, NULL);
    printf("SIGINT is now blocked. Try pressing Ctrl+C...\n");
    sleep(5);

    sigprocmask(SIG_UNBLOCK, &block_set, NULL);
    printf("SIGINT is now unblocked. Press Ctrl+C again.\n");

    while (1) pause();  // Wait for signal
}

```
## Write a program to implement a timer using signals.

```c
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>
#include <signal.h>

void timer_handler(int sig) {
    printf("Timer expired! Caught signal %d\n", sig);
    exit(0);
}

int main() {
    signal(SIGALRM, timer_handler);

    printf("Timer started for 5 seconds...\n");
    alarm(5);  // Set a 5-second timer

    while (1) {
        pause();  // Wait for signal
    }

    return 0;
}

```
## Write a program to handle a real-time signal using sigqueue().

```c
//user1//
#include <stdio.h>
#include <stdlib.h>
#include <signal.h>
#include <unistd.h>

void handler(int sig, siginfo_t *info, void *u) {
    printf("Received SIGRTMIN (%d), value = %d\n", sig, info->si_value.sival_int);
    exit(0);
}

int main() {
    struct sigaction sa = { .sa_sigaction = handler, .sa_flags = SA_SIGINFO };
    sigaction(SIGRTMIN, &sa, NULL);

    printf("PID: %d\n", getpid());
    while (1) pause();
}

//user2//
#include <signal.h>
#include <stdlib.h>

int main() {
    union sigval val = { .sival_int = 123 };
    sigqueue(/*target_pid*/, SIGRTMIN, val); // replace the /*target_pid*/ with output of user1 pid.
    return 0;
}

```
## Why we use raise system call explain it programmatically.

```c
#include <stdio.h>
#include <signal.h>
#include <stdlib.h>
#include <unistd.h>

void handle_usr1(int sig) {
    printf("Caught SIGUSR1 (signal %d)\n", sig);
}

int main() {
    signal(SIGUSR1, handle_usr1);

    printf("Raising SIGUSR1 using raise()...\n");
    raise(SIGUSR1);

    printf("Continuing after handling SIGUSR1...\n");
    return 0;
}

```
## Write a program to handle SIGALRM (alarm clock) signal for implementing a timeout mechanism in system programming.

```c
#include <stdio.h>
#include <stdlib.h>
#include <signal.h>
#include <unistd.h>

void timeout_handler(int sig) {
    printf("\nTimeout! Operation took too long (signal %d)\n", sig);
    exit(1);
}

int main() {
    signal(SIGALRM, timeout_handler);

    printf("You have 5 seconds to enter something: ");
    fflush(stdout);

    alarm(5);  // Set a 5-second timeout

    char input[100];
    if (fgets(input, sizeof(input), stdin)) {
        alarm(0);  // Cancel alarm if input is received
        printf("Input received: %s", input);
    }

    return 0;
}

```
## Write a c program on pause system call

```c
#include <stdio.h>
#include <unistd.h>
#include <signal.h>

// Signal handler function
void handle_sigint(int sig) {
    printf("Received signal %d. Exiting now.\n", sig);
}

int main() {
    // Register signal handler for SIGINT
    signal(SIGINT, handle_sigint);

    printf("Process is paused.\n");

    pause();

    printf("Process resumed after receiving a signal.\n");

    return 0;
}

// press " ctrl+c " to send the signal ( SIGINT ) //

```
## Create a C program to handle the SIGTRAP signal (trace/breakpoint trap).

```c
#include <stdio.h>
#include <signal.h>
#include <unistd.h>

// Signal handler for SIGTRAP
void handle_sigtrap(int sig) {
    printf("Caught SIGTRAP (signal number %d)\n", sig);
}

int main() {
    // Register the signal handler for SIGTRAP
    signal(SIGTRAP, handle_sigtrap);

    printf("Raising SIGTRAP signal...\n");

    // Raise the SIGTRAP signal
    raise(SIGTRAP);

    printf("Signal handled. Program continues...\n");

    return 0;
}

```
## Create a C program to handle the SIGWINCH_SIGINFO signal (window size change or status request from keyboard).

```c
#include <stdio.h>
#include <signal.h>
#include <unistd.h>
#include <sys/ioctl.h>

void handler(int sig) {
    if (sig == SIGWINCH) {
        struct winsize ws;
        ioctl(1, TIOCGWINSZ, &ws);
        printf("Window resized: %dx%d\n", ws.ws_col, ws.ws_row);
    } else if (sig == SIGUSR1) {
        printf("Received SIGUSR1 (custom status request)\n");
    }
}

int main() {
    signal(SIGWINCH, handler);
    signal(SIGUSR1, handler);
    printf("Resize window or run: kill -USR1 %d\n", getpid());
    while (1) pause();
}

```
## Implement a program to handle the SIGSYS_SIGPIPE signal (bad system call or write on a pipe with no one to read it).

```c
#include <stdio.h>
#include <signal.h>
#include <unistd.h>
#include <stdlib.h>
#include <sys/types.h>
#include <sys/wait.h>

void handler(int sig) {
    if (sig == SIGPIPE)
        printf("Caught SIGPIPE: No reader on the pipe\n");
    else if (sig == SIGSYS)
        printf("Caught SIGSYS: Bad system call\n");
}

int main() {
    signal(SIGPIPE, handler);
    signal(SIGSYS, handler);

    int pipefd[2];
    pipe(pipefd);

    pid_t pid = fork();
    if (pid == 0) {
        // Child: Close read end, exit immediately
        close(pipefd[0]);
        exit(0);
    } else {
        // Parent: Close read end to trigger SIGPIPE
        close(pipefd[0]);
        sleep(1); // Give child time to exit

        printf("Writing to pipe with no reader...\n");
        write(pipefd[1], "Hello", 5);  // Triggers SIGPIPE

        wait(NULL); // Wait for child
    }

    return 0;
}

```
## Implement a program to handle the SIGTTIN_SIGTTOU signal (background read or write attempted from control terminal).

```c
#include <stdio.h>
#include <unistd.h>
#include <signal.h>

void sig_handler(int sig) {
    if (sig == SIGTTIN)
        printf("Caught SIGTTIN: background read from terminal\n");
    else if (sig == SIGTTOU)
        printf("Caught SIGTTOU: background write to terminal\n");
}

int main() {
    // Set handlers
    signal(SIGTTIN, sig_handler);
    signal(SIGTTOU, sig_handler);

    printf("Running in foreground. Send to background and try I/O.\n");
    printf("Press Ctrl+Z, then run: 'bg' and try input/output\n");

    // Infinite loop with I/O attempt
    while (1) {
        char buf[10];
        write(STDOUT_FILENO, ">> ", 3);     // Will cause SIGTTOU if in background
        read(STDIN_FILENO, buf, 10);        // Will cause SIGTTIN if in background
    }

    return 0;
}

```
## Create a C program to handle the SIGXCPU_SIGXFSZ signal (CPU time limit exceeded or file size limit exceeded).

```c
#include <stdio.h>
#include <signal.h>
#include <unistd.h>
#include <sys/resource.h>
#include <fcntl.h>

void handler(int sig) {
    if (sig == SIGXCPU) printf("CPU time limit exceeded\n");
    if (sig == SIGXFSZ) printf("File size limit exceeded\n");
    _exit(1);
}

int main() {
    signal(SIGXCPU, handler);
    signal(SIGXFSZ, handler);

    struct rlimit r;
    r.rlim_cur = 1; r.rlim_max = 2;
    setrlimit(RLIMIT_CPU, &r);
    r.rlim_cur = 1024; r.rlim_max = 2048;
    setrlimit(RLIMIT_FSIZE, &r);

    // Uncomment one to test

    // while(1);  // Triggers SIGXCPU

    int fd = open("test.txt", O_CREAT | O_WRONLY, 0644);
    char buf[600] = {0};
    write(fd, buf, 600); write(fd, buf, 600); // Triggers SIGXFSZ
    close(fd);

    return 0;
}

```
## Implement a program to handle the SIGVTALRM_SIGWINCH signal (virtual timer expired or window size change).

```c
#include <stdio.h>
#include <signal.h>
#include <unistd.h>
#include <sys/time.h>

void handler(int sig) {
    if (sig == SIGVTALRM)
        printf("SIGVTALRM: Virtual timer expired\n");
    else if (sig == SIGWINCH)
        printf("SIGWINCH: Window size changed\n");
}

int main() {
    signal(SIGVTALRM, handler);
    signal(SIGWINCH, handler);

    struct itimerval timer = {
        .it_value.tv_sec = 1, .it_value.tv_usec = 0,      // First trigger after 1 sec
        .it_interval.tv_sec = 1, .it_interval.tv_usec = 0 // Repeat every 1 sec
    };
    setitimer(ITIMER_VIRTUAL, &timer, NULL); // Set virtual timer

    while (1); // Loop to consume CPU time (to trigger SIGVTALRM)
    return 0;
}

```
## Write a program on watch dog timer and it explain its working.

```c
// A watchdog timer is a timer used to detect and recover from software malfunctions:

If your program hangs or stops responding, the watchdog timer will reset the system or take corrective action.

Your program must "kick" or "reset" the watchdog regularly. //

#include <stdio.h>
#include <signal.h>
#include <unistd.h>

void handler(int sig) {
    printf(" Watchdog expired!\n");
    _exit(1);
}

int main() {
    signal(SIGALRM, handler);

    alarm(3); // watchdog set for 3 seconds

    for (int i = 0; i < 5; i++) {
        printf(" Kicking watchdog\n");
        alarm(3); // reset watchdog
        sleep(1);
    }

    printf(" No more kicks. Letting it expire...\n");
    while (1); // simulate hang

    return 0;
}

```
## Write a program to demonstrate how to handle and recover from a segmentation fault (SIGSEGV) in a system programming scenario.

```c
#include <stdio.h>
#include <signal.h>
#include <setjmp.h>
#include <stdlib.h>

jmp_buf jump_buffer;

// Custom handler for SIGSEGV
void segfault_handler(int sig) {
    printf(" Caught SIGSEGV: Segmentation fault occurred!\n");
    longjmp(jump_buffer, 1); // Jump back to safe point
}

int main() {
    signal(SIGSEGV, segfault_handler); // Register handler

    if (setjmp(jump_buffer) == 0) {
        // Safe checkpoint set, now cause segmentation fault
        printf(" About to cause segmentation fault...\n");
        int *ptr = NULL;
        *ptr = 42; // This will cause SIGSEGV
    } else {
        // We jump back here after handling the SIGSEGV
        printf(" Recovered from segmentation fault. Continuing program...\n");
    }

    printf(" Program still running normally.\n");
    return 0;
}

```
## Write a program to demonstrate the use of sigaction() for handling signals.

```c
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>
#include <signal.h>

// Signal handler function
void handler(int sig) {
    printf(" Caught signal %d using sigaction()\n", sig);
}

int main() {
    struct sigaction sa;
    sa.sa_handler = handler;     // Set the handler function
    sigemptyset(&sa.sa_mask);    // No signals blocked during handler
    sa.sa_flags = 0;             // No special options

    // Register handler for SIGINT (Ctrl+C)
    sigaction(SIGINT, &sa, NULL);

    printf(" Waiting for Ctrl+C (SIGINT)...\n");

    while (1) {
        sleep(1);
    }

    return 0;
}

```
## Write a program to demonstrate signal handling in a multithreaded environment.

```c
#include <stdio.h>
#include <pthread.h>
#include <signal.h>
#include <unistd.h>

void* signal_thread(void* arg) {
    sigset_t *set = arg;
    int sig;
    sigwait(set, &sig);
    printf("Signal %d caught in thread\n", sig);
    return NULL;
}

int main() {
    pthread_t tid;
    sigset_t set;

    sigemptyset(&set);
    sigaddset(&set, SIGINT); // Ctrl+C

    pthread_sigmask(SIG_BLOCK, &set, NULL); // Block in main thread

    pthread_create(&tid, NULL, signal_thread, &set);
    pthread_join(tid, NULL);

    return 0;
}

// press ctrl+c //

```
## Write a program to handle the SIGTSTP signal and suspend the process.

```c
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>
#include <signal.h>

// Signal handler for SIGTSTP
void handle_sigtstp(int sig) {
    printf("Caught SIGTSTP (Ctrl+Z). Suspending process...\n");
    raise(SIGSTOP);  // Manually stop the process
}

int main() {
    signal(SIGTSTP, handle_sigtstp);

    printf("Running... Press Ctrl+Z to test SIGTSTP handling\n");

    while (1) {
        pause();  // Wait for signals
    }

    return 0;
}

```
## Write a program to demonstrate handling multiple signals using sigaction().

```c
#include <stdio.h>
#include <signal.h>
#include <unistd.h>

void handler(int sig) {
    if (sig == SIGINT)  
	printf("Handled SIGINT\n");
    if (sig == SIGTERM)
	 printf("Handled SIGTERM\n");
    if (sig == SIGUSR1) 
	printf("Handled SIGUSR1\n");
}

int main() {
    struct sigaction sa;
    sa.sa_handler = handler;
    sigemptyset(&sa.sa_mask);
    sa.sa_flags = 0;

    sigaction(SIGINT, &sa, NULL);
    sigaction(SIGTERM, &sa, NULL);
    sigaction(SIGUSR1, &sa, NULL);

    printf("PID: %d. Waiting for signals...\n", getpid());

    while (1) pause();

    return 0;
}

// from another teminal 
	kill -SIGUSR1 <pid>
	kill -SIGTERM <pid> //

```
## Write a program to demonstrate IPC using signals.

```c
#include <stdio.h>
#include <unistd.h>
#include <signal.h>
#include <stdlib.h>

void handler(int sig) {
    printf("Parent received signal %d\n", sig);
}

int main() {
    signal(SIGUSR1, handler);

    pid_t pid = fork();

    if (pid == 0) {
        sleep(1); // Let parent set handler
        kill(getppid(), SIGUSR1);
    } else {
        pause(); // Wait for signal
    }

    return 0;
}

```
## Write a program to demonstrate error handling using signals in system calls.

```c
#include <stdio.h>
#include <unistd.h>
#include <signal.h>
#include <errno.h>

void handler(int sig) {
    write(1, "Signal received\n", 16);
}

int main() {
    signal(SIGUSR1, handler);

    char buf[10];
    int n = read(0, buf, sizeof(buf)); // Wait for input

    if (n == -1 && errno == EINTR)
        write(1, "read() interrupted by signal\n", 30);
    else if (n > 0)
        write(1, "Data received\n", 14);

    return 0;
}

```
## Write a program to demonstrate signal handling in a client-server architecture.

```c
#include <stdio.h>
#include <unistd.h>
#include <signal.h>
#include <stdlib.h>

void handler(int sig) {
    if (sig == SIGUSR1) printf("Server: Got SIGUSR1 from client\n");
    if (sig == SIGUSR2) printf("Server: Got SIGUSR2 from client\n");
}

int main() {
    pid_t pid;
    signal(SIGUSR1, handler);
    signal(SIGUSR2, handler);

    pid = fork();

    if (pid == 0) {  // Client
        sleep(1);
        kill(getppid(), SIGUSR1);
        sleep(1);
        kill(getppid(), SIGUSR2);
    } else {         // Server
        printf("Server (PID: %d) waiting for signals...\n", getpid());
        while (1) pause();
    }

    return 0;
}

```
## Write a program to demonstrate signal handling during fork() and exec().

```c
 #include <stdio.h>
#include <unistd.h>
#include <signal.h>
#include <stdlib.h>
#include <sys/wait.h>

void handler(int sig) {
    printf("Signal %d caught in parent or before exec\n", sig);
}

int main() {
    signal(SIGUSR1, handler);

    pid_t pid = fork();

    if (pid == 0) {  // Child
        printf("Child: Before exec, PID = %d\n", getpid());
        sleep(1);  // Let parent send signal here to test
        char *args[] = {"/bin/sleep", "5", NULL};  // exec to simple binary
        execvp(args[0], args);
        perror("exec failed");
    } else {
        sleep(2);  // Give child time to run
        kill(pid, SIGUSR1);
        wait(NULL);
    }

    return 0;
}

/* to compile:-
    gcc filename.c -o run
    ./run
*/

```
## Write a program to demonstrate how to block and unblock signals using sigprocmask() in a system programming context.

```c
#include <stdio.h>
#include <signal.h>
#include <unistd.h>

void handler(int sig) {
    printf("Caught signal %d\n", sig);
}

int main() {
    signal(SIGINT, handler);

    sigset_t set;
    sigemptyset(&set);
    sigaddset(&set, SIGINT);

    printf("Blocking SIGINT for 5 seconds...\n");
    sigprocmask(SIG_BLOCK, &set, NULL);
    sleep(5);

    printf("Unblocking SIGINT. Press Ctrl+C now...\n");
    sigprocmask(SIG_UNBLOCK, &set, NULL);

    while (1) pause();
    return 0;
}

```
## Write a program to demonstrate how to handle SIGBUS (bus error) signal in a system programming context.

```c
#include <stdio.h>
#include <stdlib.h>
#include <signal.h>
#include <fcntl.h>
#include <unistd.h>
#include <sys/mman.h>

void handler(int sig) {
    printf("Caught SIGBUS (signal %d)\n", sig);
    exit(1);
}

int main() {
    signal(SIGBUS, handler);

    int fd = open("file.txt", O_RDWR | O_CREAT, 0666);
    write(fd, "123", 3);  // file size = 3 bytes

    char *ptr = mmap(0, 4096, PROT_READ | PROT_WRITE, MAP_SHARED, fd, 0);
    close(fd);

    ptr[1000] = 'X'; // access beyond file size → SIGBUS
    return 0;
}

/* to c the text file enter " cat file.txt " */

```
## Write a program to demonstrate the usage of sigaction() for handling SIGUSR1 and SIGUSR2 signals in a system programming scenario.

```c
#include <stdio.h>
#include <stdlib.h>
#include <signal.h>
#include <unistd.h>

void handle_signal(int sig) {
    if (sig == SIGUSR1)
        printf("Received SIGUSR1 (User-defined signal 1)\n");
    else if (sig == SIGUSR2)
        printf("Received SIGUSR2 (User-defined signal 2)\n");
}

int main() {
    struct sigaction sa;
    sa.sa_handler = handle_signal;
    sigemptyset(&sa.sa_mask);
    sa.sa_flags = 0;

    sigaction(SIGUSR1, &sa, NULL);
    sigaction(SIGUSR2, &sa, NULL);

    printf("PID: %d\n", getpid());
    printf("Send SIGUSR1 or SIGUSR2 using `kill -USR1 %d` or `kill -USR2 %d`\n", getpid(), getpid());

    while (1) {
        pause(); // Wait for signals
    }

    return 0;
}

```
## Create a C program to handle the SIGCHLD_SIGCONT signal (child process terminated or continue executing).

```c
#include <stdio.h>
#include <signal.h>
#include <unistd.h>
#include <stdlib.h>
#include <sys/wait.h>

void handler(int sig) {
    if (sig == SIGCHLD) { printf("SIGCHLD: Child exited\n"); wait(NULL); }
    if (sig == SIGCONT)  printf("SIGCONT: Process continued\n");
}

int main() {
    signal(SIGCHLD, handler);
    signal(SIGCONT, handler);

    if (fork() == 0) {
        sleep(1);
        exit(0);
    }

    while (1) pause();
}

```
## Write a program to handle the SIGBUS_SIGCHLD signal (bus error or child process terminated).

```c
#include <stdio.h>
#include <stdlib.h>
#include <signal.h>
#include <unistd.h>
#include <sys/mman.h>
#include <fcntl.h>
#include <sys/wait.h>

void handler(int sig) {
    if (sig == SIGBUS)  { printf("Caught SIGBUS\n"); exit(1); }
    if (sig == SIGCHLD) { printf("Caught SIGCHLD: Child exited\n"); wait(NULL); }
}

int main() {
    signal(SIGBUS, handler);
    signal(SIGCHLD, handler);

    if (fork() == 0) { sleep(1); exit(0); } // child exits

    int fd = open("file.txt", O_RDWR | O_CREAT, 0666);
    write(fd, "abc", 3); // file size = 3 bytes
    char *p = mmap(0, 4096, PROT_WRITE, MAP_SHARED, fd, 0);
    close(fd);

    p[1000] = 'X'; // trigger SIGBUS

    return 0;
}

/* to compile
	gcc filename.c -o sigdemo
	./sigdemo
*/

```
## Implement a program to handle the SIGPWR_SIGSYS signal (power failure restart or bad system call).

```c
#include <stdio.h>
#include <stdlib.h>
#include <signal.h>
#include <unistd.h>

void handler(int sig) {
    if (sig == SIGPWR)
        printf("Caught SIGPWR: Power failure signal\n");
    else if (sig == SIGSYS)
        printf("Caught SIGSYS: Bad system call\n");
}

int main() {
    signal(SIGPWR, handler);
    signal(SIGSYS, handler);

    printf("Raising SIGSYS...\n");
    raise(SIGSYS);  // Simulate bad system call

    printf("Raising SIGPWR...\n");
    raise(SIGPWR);  // Simulate power failure

    return 0;
}

```
## Write a program to handle the SIGPIPE_SIGQUIT signal (write on a pipe with no one to read it or quit signal).

```c
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>
#include <signal.h>

void handler(int sig) {
    if (sig == SIGPIPE)
        printf("Caught SIGPIPE: Broken pipe\n");
    else if (sig == SIGQUIT)
        printf("Caught SIGQUIT: Quit signal\n");
}

int main() {
    signal(SIGPIPE, handler);
    signal(SIGQUIT, handler);

    int fd[2];
    pipe(fd);

    close(fd[0]); // Close read end

    write(fd[1], "hello", 5); // Writing with no reader → SIGPIPE

    while (1) pause(); // Press Ctrl+\ to send SIGQUIT

    return 0;
}

/* press " ctrl+\ " for full compilatiom */

```
## Write a C program that forks a child process. Implement signal handlers in both the parent and child processes to handle the SIGUSR1 signal. When the parent process receives SIGUSR1, it should send the signal to the child process, which then prints a message indicating the signal reception

```c
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>
#include <signal.h>

pid_t child_pid;  // global to access in parent signal handler

void parent_handler(int sig) {
    if (sig == SIGUSR1) {
        printf("Parent: Received SIGUSR1, forwarding to child...\n");
        kill(child_pid, SIGUSR1);
    }
}

void child_handler(int sig) {
    if (sig == SIGUSR1) {
        printf("Child: Received SIGUSR1 from parent\n");
    }
}

int main() {
    signal(SIGUSR1, parent_handler);  // Parent installs handler

    child_pid = fork();

    if (child_pid == 0) {
        // Child process
        signal(SIGUSR1, child_handler);
        while (1) pause();  // Wait for signal
    } else {
        // Parent process
        printf("Parent PID: %d, Child PID: %d\n", getpid(), child_pid);
        printf("Send SIGUSR1 to parent using: kill -USR1 %d\n", getpid());
        while (1) pause();  // Wait for signal
    }

    return 0;
}

```
