## Write a C program to create a thread that prints "Hello, World!"?

```c
#include <stdio.h>
#include <pthread.h>

void* say_hello(void* arg) {
    puts("Hello, World!");
    return NULL;
}

int main() {
    pthread_t t;
    pthread_create(&t, NULL, say_hello, NULL);
    pthread_join(t, NULL);
    return 0;
}

```

## Modify the above program to create multiple threads, each printing its own message?

```c
#include <stdio.h>
#include <pthread.h>

#define NUM_THREADS 5

void* print_msg(void* arg) {
    int id = *(int*)arg;
    printf("Hello from thread %d!\n", id);
    return NULL;
}

int main() {
    pthread_t threads[NUM_THREADS];
    int ids[NUM_THREADS];

    for (int i = 0; i < NUM_THREADS; ++i) {
        ids[i] = i + 1;
        pthread_create(&threads[i], NULL, print_msg, &ids[i]);
        pthread_join(threads[i], NULL);
    }

    return 0;
}

```

## Develop a C program to create two threads that print numbers from 1 to 10 concurrently?

```c
#include <stdio.h>
#include <pthread.h>
#include <unistd.h>

void* print_numbers(void* arg) {
    int id = *(int*)arg;
    for (int i = 1; i <= 10; ++i) {
        printf("Thread %d: %d\n", id, i);
        usleep(100000); // Sleep 0.1s for better interleaving
    }
    return NULL;
}

int main() {
    pthread_t t1, t2;
    int id1 = 1, id2 = 2;

    pthread_create(&t1, NULL, print_numbers, &id1);
    pthread_create(&t2, NULL, print_numbers, &id2);

    pthread_join(t1, NULL);
    pthread_join(t2, NULL);

    return 0;
}

```

## Implement a C program to create a thread that calculates the factorial of a given number?

```c
#include <stdio.h>
#include <pthread.h>

int num;
long long fact = 1;

void* factorial(void* arg) {
    for (int i = 1; i <= num; ++i) fact *= i;
    return NULL;
}

int main() {
    printf("Enter a number: ");
    scanf("%d", &num);

    pthread_t t;
    pthread_create(&t, NULL, factorial, NULL);
    pthread_join(t, NULL);

    printf("Factorial of %d is %lld\n", num, fact);
    return 0;
}


```

## Write a C program to create two threads that print their thread IDs?

```c
#include <stdio.h>
#include <pthread.h>

void* show_id(void* arg) {
    printf("Thread ID: %lu\n", pthread_self());
    return NULL;
}

int main() {
    pthread_t t1, t2;

    pthread_create(&t1, NULL, show_id, NULL);
    pthread_create(&t2, NULL, show_id, NULL);

    pthread_join(t1, NULL);
    pthread_join(t2, NULL);

    return 0;
}

```

## Develop a C program to create a thread that prints the sum of two numbers?

```c
#include <stdio.h>
#include <pthread.h>

int a, b;

void* add(void* arg) {
    printf("Sum = %d\n", a + b);
    return NULL;
}

int main() {
    printf("Enter two numbers: ");
    scanf("%d %d", &a, &b);

    pthread_t t;
    pthread_create(&t, NULL, add, NULL);
    pthread_join(t, NULL);

    return 0;
}

```

## Implement a C program to create a thread that calculates the square of a number?

```c
#include <stdio.h>
#include <pthread.h>

void* square(void* arg) {
    int num = *(int*)arg;
    int result = num * num;
    printf("Square of %d is %d\n", num, result);
    return NULL;
}

int main() {
    pthread_t thread;
    int num;

    printf("Enter a number: ");
    scanf("%d", &num);

    pthread_create(&thread, NULL, square, &num);
    pthread_join(thread, NULL);

    return 0;
}

```
## Write a C program to create a thread that prints the current date and time?

```c
#include <stdio.h>
#include <pthread.h>
#include <time.h>

void* print_datetime(void* arg) {
    time_t now;
    time(&now);

    struct tm *local = localtime(&now);
    printf("Current Date and Time: %s", asctime(local));
    return NULL;
}

int main() {
    pthread_t thread;

    pthread_create(&thread, NULL, print_datetime, NULL);
    pthread_join(thread, NULL);

    return 0;
}

```
## Develop a C program to create a thread that checks if a number is prime?

```c
#include <stdio.h>
#include <pthread.h>

void* is_prime(void* arg) {
    int n = *(int*)arg, i;
    for (i = 2; i < n; i++)
        if (n % i == 0) break;
    printf("%d is %sprime.\n", n, (i == n && n > 1) ? "" : "not ");
    return NULL;
}

int main() {
    pthread_t t;
    int num;
    printf("Enter a number: ");
    scanf("%d", &num);
    pthread_create(&t, NULL, is_prime, &num);
    pthread_join(t, NULL);
    return 0;
}

```
## Implement a C program to create a thread that checks if a given string is a palindrome?

```c
#include <stdio.h>
#include <string.h>
#include <pthread.h>

void* check_palindrome(void* arg) {
    char *str = (char*)arg;
    int start = 0, end = strlen(str) - 1;
    while (start < end) {
        if (str[start++] != str[end--]) {
            printf("Not a palindrome.\n");
            return NULL;
        }
    }
    printf("It's a palindrome.\n");
    return NULL;
}

int main() {
    pthread_t thread;
    char input[100];

    printf("Enter a string: ");
    scanf("%s", input);

    pthread_create(&thread, NULL, check_palindrome, input);
    pthread_join(thread, NULL);

    return 0;
}

```
## Write a C program to create a thread that prints "Hello, World!" with thread synchronization?

```c
#include <stdio.h>
#include <pthread.h>

pthread_mutex_t mtx;

void* hello(void* arg) {
    pthread_mutex_lock(&mtx);
    printf("Hello, World!\n");
    pthread_mutex_unlock(&mtx);
    return NULL;
}

int main() {
    pthread_t t;
    pthread_mutex_init(&mtx, NULL);
    pthread_create(&t, NULL, hello, NULL);
    pthread_join(t, NULL);
    pthread_mutex_destroy(&mtx);
    return 0;
}

```
## Develop a C program to create two threads that print their thread IDs and synchronize their output?

```c
#include <stdio.h>
#include <pthread.h>

pthread_mutex_t mtx;

void* show_id(void* arg) {
    pthread_mutex_lock(&mtx);
    printf("Thread ID: %lu\n", pthread_self());
    pthread_mutex_unlock(&mtx);
    return NULL;
}

int main() {
    pthread_t t1, t2;
    pthread_mutex_init(&mtx, NULL);

    pthread_create(&t1, NULL, show_id, NULL);
    pthread_create(&t2, NULL, show_id, NULL);

    pthread_join(t1, NULL);
    pthread_join(t2, NULL);

    pthread_mutex_destroy(&mtx);
    return 0;
}

```
## Implement a C program to create a thread that generates random numbers and synchronizes access to a shared buffer?

```c
#include <stdio.h>
#include <stdlib.h>
#include <pthread.h>
#include <time.h>

int buf[5], i = 0;
pthread_mutex_t mtx;

void* gen(void* arg) {
    while (i < 5) {
        int n = rand() % 100;
        pthread_mutex_lock(&mtx);
        buf[i++] = n;
        printf("%d\n", n);
        pthread_mutex_unlock(&mtx);
    }
    return NULL;
}

int main() {
    pthread_t t;
    srand(time(NULL));
    pthread_mutex_init(&mtx, NULL);
    pthread_create(&t, NULL, gen, NULL);
    pthread_join(t, NULL);
    pthread_mutex_destroy(&mtx);
    return 0;
}

```
## Write a C program to create a thread that performs addition of two numbers with mutex locks?

```c
#include <stdio.h>
#include <pthread.h>

int a, b, sum;
pthread_mutex_t mtx;

void* add(void* arg) {
    pthread_mutex_lock(&mtx);
    sum = a + b;
    printf("Sum: %d\n", sum);
    pthread_mutex_unlock(&mtx);
    return NULL;
}

int main() {
    pthread_t t;
    pthread_mutex_init(&mtx, NULL);

    printf("Enter two numbers: ");
    scanf("%d %d", &a, &b);

    pthread_create(&t, NULL, add, NULL);
    pthread_join(t, NULL);

    pthread_mutex_destroy(&mtx);
    return 0;
}

```
## Implement a C program to create two threads that increment and decrement a shared variable, respectively, using mutex locks?

```c
#include <stdio.h>
#include <pthread.h>

int val = 0, n;
pthread_mutex_t m;

void* inc(void* p) { while (n--) { pthread_mutex_lock(&m); val++; pthread_mutex_unlock(&m); } return NULL; }
void* dec(void* p) { while (++n < 0) { pthread_mutex_lock(&m); val--; pthread_mutex_unlock(&m); } return NULL; }

int main() {
    pthread_t t1, t2;
    pthread_mutex_init(&m, NULL);
    printf("Count: "); scanf("%d", &n);
    pthread_create(&t1, NULL, inc, NULL);
    pthread_create(&t2, NULL, dec, NULL);
    pthread_join(t1, NULL); pthread_join(t2, NULL);
    printf("Val: %d\n", val);
    return 0;
}

```
## Develop a C program to create a thread that reads input from the user and synchronizes access to shared resources?

```c
#include <stdio.h>
#include <pthread.h>

char msg[100];
pthread_mutex_t m;

void* get_input(void* arg) {
    pthread_mutex_lock(&m);
    printf("Enter message: ");
    fgets(msg, sizeof(msg), stdin);
    pthread_mutex_unlock(&m);
    return NULL;
}

int main() {
    pthread_t t;
    pthread_mutex_init(&m, NULL);
    pthread_create(&t, NULL, get_input, NULL);
    pthread_join(t, NULL);
    printf("You entered: %s", msg);
    return 0;
}

```
## Implement a C program to create a thread that prints prime numbers up to a given limit with mutex locks

```c
#include <stdio.h>
#include <pthread.h>

int n;
pthread_mutex_t m;

int prime(int x) {
  for (int i = 2; i*i <= x; i++)
    if (x % i == 0) return 0;
  return x > 1;
}

void* run(void* a) {
  pthread_mutex_lock(&m);
  for (int i = 2; i <= n; i++)
    if (prime(i)) printf("%d ", i);
  pthread_mutex_unlock(&m);
  return NULL;
}

int main() {
  pthread_t t;
  pthread_mutex_init(&m, NULL);
  printf("Limit: "); scanf("%d", &n);
  pthread_create(&t, NULL, run, NULL);
  pthread_join(t, NULL);
  return 0;
}

```
## Implement a C program to create a thread that calculates the sum of Fibonacci numbers up to a given limit using dynamic programming with mutex locks?

```c
#include <stdio.h>
#include <pthread.h>

int n, sum = 1;
pthread_mutex_t m;

void* calc_fib(void* arg) {
  int a = 0, b = 1;
  pthread_mutex_lock(&m);
  for (int i = 2; i < n; i++) {
    int c = a + b;
    sum += c;
    a = b; b = c;
  }
  pthread_mutex_unlock(&m);
  return NULL;
}

int main() {
  pthread_t t;
  pthread_mutex_init(&m, NULL);
  printf("Enter n: ");
  scanf("%d", &n);
  if (n <= 0) { printf("Sum: 0\n"); return 0; }
  if (n == 1) { printf("Sum: 0\n"); return 0; }

  pthread_create(&t, NULL, calc_fib, NULL);
  pthread_join(t, NULL);
  printf("Sum: %d\n", n == 2 ? 1 : sum);
  return 0;
}

```
## Write a C program to create a thread that checks if a given year is a leap year using dynamic programming with mutex locks?

```c
#include <stdio.h>
#include <pthread.h>

int year, isLeap = 0;
pthread_mutex_t lock;

void* check_leap(void* arg) {
    pthread_mutex_lock(&lock);
    if ((year % 4 == 0 && year % 100 != 0) || (year % 400 == 0)) {
        isLeap = 1;
    }
    pthread_mutex_unlock(&lock);
    return NULL;
}

int main() {
    pthread_t t;
    pthread_mutex_init(&lock, NULL);

    printf("Enter year: ");
    scanf("%d", &year);

    pthread_create(&t, NULL, check_leap, NULL);
    pthread_join(t, NULL);

    if (isLeap)
        printf("%d is a leap year\n", year);
    else
        printf("%d is not a leap year\n", year);

    return 0;
}

```
## Write a C program to create a thread that checks if a given string is a palindrome using dynamic programming with mutex locks?

```c
#include <stdio.h>
#include <string.h>
#include <pthread.h>

char str[100];
int isPal = 1;
pthread_mutex_t lock;

void* check(void* arg) {
    pthread_mutex_lock(&lock);
    int l = strlen(str);
    for (int i = 0; i < l / 2; i++) {
        if (str[i] != str[l - i - 1]) {
            isPal = 0;
            break;
        }
    }
    pthread_mutex_unlock(&lock);
    return NULL;
}

int main() {
    pthread_t t;
    pthread_mutex_init(&lock, NULL);
    printf("Enter a string: ");
    scanf("%s", str);
    pthread_create(&t, NULL, check, NULL);
    pthread_join(t, NULL);
    printf("%s\n", isPal ? "Palindrome" : "Not Palindrome");
    return 0;
}

```
## Implement a C program to create a thread that performs selection sort on an array of integers?

```c
#include <stdio.h>
#include <pthread.h>

int a[100], n;

void* sort(void* arg) {
    for (int i = 0; i < n - 1; i++)
        for (int j = i + 1; j < n; j++)
            if (a[j] < a[i]) {
                int t = a[i]; a[i] = a[j]; a[j] = t;
            }
    return NULL;
}

int main() {
    pthread_t t;
    scanf("%d", &n);
    for (int i = 0; i < n; i++) scanf("%d", &a[i]);
    pthread_create(&t, NULL, sort, NULL);
    pthread_join(t, NULL);
    for (int i = 0; i < n; i++) printf("%d ", a[i]);
    return 0;
}

```
## Develop a C program to create a thread that calculates the area of a triangle?

```c
#include <stdio.h>
#include <pthread.h>

float base, height, area;

void* calc_area(void* arg) {
    area = 0.5 * base * height;
    return NULL;
}

int main() {
    pthread_t t;
    printf("Enter base and height: ");
    scanf("%f %f", &base, &height);

    pthread_create(&t, NULL, calc_area, NULL);
    pthread_join(t, NULL);

    printf("Area of triangle: %.2f\n", area);
    return 0;
}

```
## Write a C program to create a thread that calculates the sum of squares of numbers from 1 to 100?

```c
#include <stdio.h>
#include <pthread.h>

int sum = 0;

void* calc_squares(void* arg) {
    for (int i = 1; i <= 100; i++)
        sum += i * i;
    return NULL;
}

int main() {
    pthread_t t;
    pthread_create(&t, NULL, calc_squares, NULL);
    pthread_join(t, NULL);
    printf("Sum of squares from 1 to 100: %d\n", sum);
    return 0;
}

```
## Write a C program to create a thread that generates a random array of integers?

```c

#include <stdio.h>
#include <pthread.h>
#include <stdlib.h>
#include <time.h>

int a[100], n;

void* gen(void* p) {
    for (int i = 0; i < n; i++) a[i] = rand() % 100;
    return NULL;
}

int main() {
    pthread_t t;
    srand(time(0));
    scanf("%d", &n);
    pthread_create(&t, NULL, gen, NULL);
    pthread_join(t, NULL);
    for (int i = 0; i < n; i++) printf("%d ", a[i]);
}

```
## Implement a C program to create a thread that performs bubble sort on an array of integers?

```c

#include <stdio.h>
#include <pthread.h>

int a[100], n;

void* bubble_sort(void* arg) {
    for (int i = 0; i < n-1; i++)
        for (int j = 0; j < n-i-1; j++)
            if (a[j] > a[j+1]) {
                int t = a[j]; a[j] = a[j+1]; a[j+1] = t;
            }
    return NULL;
}

int main() {
    pthread_t t;
    scanf("%d", &n);
    for (int i = 0; i < n; i++) scanf("%d", &a[i]);
    pthread_create(&t, NULL, bubble_sort, NULL);
    pthread_join(t, NULL);
    for (int i = 0; i < n; i++) printf("%d ", a[i]);
}

```
## Develop a C program to create a thread that calculates the greatest common divisor (GCD) of two numbers?

```c
#include <stdio.h>
#include <pthread.h>

int a, b, gcd;

void* find_gcd(void* arg) {
    int x = a, y = b;
    while (y) {
        int temp = y;
        y = x % y;
        x = temp;
    }
    gcd = x;
    return NULL;
}

int main() {
    pthread_t t;
    printf("Enter two numbers: ");
    scanf("%d %d", &a, &b);

    pthread_create(&t, NULL, find_gcd, NULL);
    pthread_join(t, NULL);

    printf("GCD: %d\n", gcd);
    return 0;
}

```
## Write a C program to create a thread that calculates the sum of Fibonacci numbers up to a given limit?

```c

#include <stdio.h>
#include <pthread.h>

int limit, sum = 0;

void* fib_sum(void* arg) {
    int a = 0, b = 1;
    while (a <= limit) {
        sum += a;
        int temp = a + b;
        a = b;
        b = temp;
    }
    return NULL;
}

int main() {
    pthread_t t;
    printf("Enter limit: ");
    scanf("%d", &limit);

    pthread_create(&t, NULL, fib_sum, NULL);
    pthread_join(t, NULL);

    printf("Sum of Fibonacci numbers up to %d: %d\n", limit, sum);
    return 0;
}

```
## Implement a C program to create a thread that calculates the sum of even numbers from 1 to 100

```c

#include <stdio.h>
#include <pthread.h>

int sum = 0;

void* sum_even(void* arg) {
    for (int i = 2; i <= 100; i += 2)
        sum += i;
    return NULL;
}

int main() {
    pthread_t t;
    pthread_create(&t, NULL, sum_even, NULL);
    pthread_join(t, NULL);
    printf("Sum of even numbers from 1 to 100: %d\n", sum);
    return 0;
}

```
## Develop a C program to create a thread that calculates the factorial of a given number using iteration?

```c

#include <stdio.h>
#include <pthread.h>

int num;
long long fact = 1;

void* factorial(void* arg) {
    for (int i = 1; i <= num; i++)
        fact *= i;
    return NULL;
}

int main() {
    pthread_t t;
    printf("Enter a number: ");
    scanf("%d", &num);

    pthread_create(&t, NULL, factorial, NULL);
    pthread_join(t, NULL);

    printf("Factorial of %d = %lld\n", num, fact);
    return 0;
}

```
## Write a C program to create a thread that checks if a given year is a leap year?

```c
#include <stdio.h>
#include <pthread.h>

int year;

void* check_leap_year(void* arg) {
    if ((year % 4 == 0 && year % 100 != 0) || (year % 400 == 0))
        printf("%d is a leap year.\n", year);
    else
        printf("%d is not a leap year.\n", year);
    return NULL;
}

int main() {
    pthread_t t;
    printf("Enter a year: ");
    scanf("%d", &year);

    pthread_create(&t, NULL, check_leap_year, NULL);
    pthread_join(t, NULL);

    return 0;
}

```
## Implement a C program to create a thread that performs multiplication of two matrices?

```c
#include <stdio.h>
#include <pthread.h>

int A[2][2] = {{1, 2}, {3, 4}};
int B[2][2] = {{5, 6}, {7, 8}};
int C[2][2];

void* multiply(void* arg) {
    for (int i = 0; i < 2; i++)
        for (int j = 0; j < 2; j++)
            C[i][j] = A[i][0] * B[0][j] + A[i][1] * B[1][j];
    return NULL;
}

int main() {
    pthread_t t;
    pthread_create(&t, NULL, multiply, NULL);
    pthread_join(t, NULL);

    printf("Result:\n");
    for (int i = 0; i < 2; i++)
        printf("%d %d\n", C[i][0], C[i][1]);
    return 0;
}

```
## Develop a C program to create a thread that calculates the average of numbers from 1 to 100?

```c
#include <stdio.h>
#include <pthread.h>

void* calc_avg(void* arg) {
    int sum = 0;
    for (int i = 1; i <= 100; i++)
        sum += i;
    float avg = sum / 100.0;
    printf("Average = %.2f\n", avg);
    return NULL;
}

int main() {
    pthread_t t;
    pthread_create(&t, NULL, calc_avg, NULL);
    pthread_join(t, NULL);
    return 0;
}

```
## Implement a C program to create a thread that generates a random string?

```c
#include <stdio.h>
#include <stdlib.h>
#include <pthread.h>
#include <time.h>

void* generate_string(void* arg) {
    int len = 10;
    char str[len + 1];
    for (int i = 0; i < len; i++)
        str[i] = 'A' + rand() % 26;
    str[len] = '\0';
    printf("Random String: %s\n", str);
    return NULL;
}

int main() {
    srand(time(NULL));
    pthread_t t;
    pthread_create(&t, NULL, generate_string, NULL);
    pthread_join(t, NULL);
    return 0;
}

```
## Write a C program to create a thread that checks if a given number is a perfect square?

```c

#include <stdio.h>
#include <pthread.h>

void* square(void* arg) {
    int num = *(int*)arg;
    int result = num * num;
    printf("Square of %d is %d\n", num, result);
    return NULL;
}

int main() {
    pthread_t thread;
    int num = 5;

    pthread_create(&thread, NULL, square, &num);
    pthread_join(thread, NULL);

    return 0;
}

```
## Write a C program to create a thread that calculates the sum of digits of a given number?

```c
#include <stdio.h>
#include <pthread.h>

int number;

void* sum_of_digits(void* arg) {
    int n = number, sum = 0;
    while (n > 0) {
        sum += n % 10;
        n /= 10;
    }
    printf("Sum of digits = %d\n", sum);
    return NULL;
}

int main() {
    printf("Enter a number: ");
    scanf("%d", &number);

    pthread_t t;
    pthread_create(&t, NULL, sum_of_digits, NULL);
    pthread_join(t, NULL);
    return 0;
}

```
## Implement a C program to create a thread that calculates the factorial of a given number using recursion?

```c
#include <stdio.h>
#include <pthread.h>

int number;

long long factorial(int n) {
    if (n == 0 || n == 1) return 1;
    return n * factorial(n - 1);
}

void* compute_factorial(void* arg) {
    long long result = factorial(number);
    printf("Factorial of %d = %lld\n", number, result);
    return NULL;
}

int main() {
    printf("Enter a number: ");
    scanf("%d", &number);

    pthread_t t;
    pthread_create(&t, NULL, compute_factorial, NULL);
    pthread_join(t, NULL);
    return 0;
}

```
## Develop a C program to create a thread that finds the maximum element in an array?

```c
#include <stdio.h>
#include <pthread.h>

int arr[100], n;

void* find_max(void* arg) {
    int max = arr[0];
    for (int i = 1; i < n; i++)
        if (arr[i] > max)
            max = arr[i];
    printf("Maximum element = %d\n", max);
    return NULL;
}

int main() {
    printf("Enter number of elements: ");
    scanf("%d", &n);

    printf("Enter %d integers:\n", n);
    for (int i = 0; i < n; i++)
        scanf("%d", &arr[i]);

    pthread_t t;
    pthread_create(&t, NULL, find_max, NULL);
    pthread_join(t, NULL);

    return 0;
}

```
## Write a C program to create a thread that sorts an array of strings?

```c
#include <stdio.h>
#include <string.h>
#include <pthread.h>

char *arr[] = {"banana", "apple", "mango", "grape", "peach"};

void* sort_strings(void* arg) {
    char *temp;
    for (int i = 0; i < 4; i++)
        for (int j = i + 1; j < 5; j++)
            if (strcmp(arr[i], arr[j]) > 0) {
                temp = arr[i];
                arr[i] = arr[j];
                arr[j] = temp;
            }

    for (int i = 0; i < 5; i++)
        printf("%s\n", arr[i]);
    return NULL;
}

int main() {
    pthread_t t;
    pthread_create(&t, NULL, sort_strings, NULL);
    pthread_join(t, NULL);
    return 0;
}

```
## Implement a C program to create a thread that calculates the square root of a number?

```c

#include <stdio.h>
#include <pthread.h>
#include <math.h>

double num;

void* calc_sqrt(void* arg) {
    printf("Square root = %.2f\n", sqrt(num));
    return NULL;
}

int main() {
    printf("Enter a number: ");
    scanf("%lf", &num);

    pthread_t t;
    pthread_create(&t, NULL, calc_sqrt, NULL);
    pthread_join(t, NULL);
    return 0;
}

```
## Develop a C program to create a thread that calculates the average of numbers in an array?

```c
#include <stdio.h>
#include <pthread.h>

int arr[] = {10, 20, 30, 40, 50};
int n = 5;
double avg;

void* calculate_average(void* arg) {
    int sum = 0;
    for(int i = 0; i < n; i++)
        sum += arr[i];
    avg = (double)sum / n;
    return NULL;
}

int main() {
    pthread_t t;

    pthread_create(&t, NULL, calculate_average, NULL);
    pthread_join(t, NULL);

    printf("Average = %.2f\n", avg);
    return 0;
}

```
## Write a C program to create a thread that generates a random password?

```c
#include <stdio.h>
#include <stdlib.h>
#include <pthread.h>
#include <time.h>

#define PASSWORD_LENGTH 10

void* generate_password(void* arg) {
    char charset[] = "abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!@#$%^&*";
    char password[PASSWORD_LENGTH + 1];

    for (int i = 0; i < PASSWORD_LENGTH; i++) {
        int key = rand() % (sizeof(charset) - 1);
        password[i] = charset[key];
    }
    password[PASSWORD_LENGTH] = '\0';

    printf("Generated Password: %s\n", password);
    return NULL;
}

int main() {
    pthread_t tid;
    srand(time(NULL)); // Seed random generator

    pthread_create(&tid, NULL, generate_password, NULL);
    pthread_join(tid, NULL);

    return 0;
}

```
## Implement a C program to create a thread that checks if a number is even or odd?

```c
#include <stdio.h>
#include <pthread.h>

int num;

void* check_even_odd(void* arg) {
    if (num % 2 == 0)
        printf("%d is Even\n", num);
    else
        printf("%d is Odd\n", num);
    return NULL;
}

int main() {
    pthread_t t;
    printf("Enter a number: ");
    scanf("%d", &num);

    pthread_create(&t, NULL, check_even_odd, NULL);
    pthread_join(t, NULL);
    return 0;
}

```
## Develop a C program to create a thread that calculates the sum of elements in an array?

```c
#include <stdio.h>
#include <pthread.h>

int arr[100], n, sum = 0;

void* calc_sum(void* arg) {
    for (int i = 0; i < n; i++)
        sum += arr[i];
    return NULL;
}

int main() {
    printf("Enter number of elements: ");
    scanf("%d", &n);
    printf("Enter %d elements:\n", n);
    for (int i = 0; i < n; i++)
        scanf("%d", &arr[i]);

    pthread_t t;
    pthread_create(&t, NULL, calc_sum, NULL);
    pthread_join(t, NULL);

    printf("Sum = %d\n", sum);
    return 0;
}

```
## Write a C program to create a thread that calculates the factorial of numbers from 1 to 10?

```c
#include <stdio.h>
#include <pthread.h>

void* calculate_factorials(void* arg) {
    for (int i = 1; i <= 10; i++) {
        long long factorial = 1;
        for (int j = 1; j <= i; j++) {
            factorial *= j;
        }
        printf("Factorial of %d = %lld\n", i, factorial);
    }
    return NULL;
}

int main() {
    pthread_t thread;
    pthread_create(&thread, NULL, calculate_factorials, NULL);
    pthread_join(thread, NULL);
    return 0;
}

```
## Implement a C program to create a thread that calculates the area of a rectangle?

```c
#include <stdio.h>
#include <pthread.h>

float length, breadth;

void* calc_area(void* arg) {
    float area = length * breadth;
    printf("Area of Rectangle = %.2f\n", area);
    return NULL;
}

int main() {
    printf("Enter length: ");
    scanf("%f", &length);
    printf("Enter breadth: ");
    scanf("%f", &breadth);

    pthread_t t;
    pthread_create(&t, NULL, calc_area, NULL);
    pthread_join(t, NULL);
    return 0;
}

```
## Develop a C program to create a thread that generates random numbers?

```c
#include <stdio.h>
#include <stdlib.h>
#include <pthread.h>
#include <time.h>

void* generate_random(void* arg) {
    srand(time(NULL));
    printf("Random numbers: ");
    for (int i = 0; i < 5; i++)
        printf("%d ", rand() % 100);
    printf("\n");
    return NULL;
}

int main() {
    pthread_t t;
    pthread_create(&t, NULL, generate_random, NULL);
    pthread_join(t, NULL);
    return 0;
}

```
## Implement a C program to create a thread that sorts an array of integers?

```c
#include <stdio.h>
#include <pthread.h>

int a[] = {4, 2, 5, 1, 3};

void* sort(void* arg) {
    for (int i = 0; i < 5; i++) {
        for (int j = i + 1; j < 5; j++) {
            if (a[i] > a[j]) {
                int t = a[i];
                a[i] = a[j];
                a[j] = t;
            }
        }
    }
    return NULL;
}

int main() {
    pthread_t t;
    pthread_create(&t, NULL, sort, NULL);
    pthread_join(t, NULL);

    for (int i = 0; i < 5; i++) {
        printf("%d ", a[i]);
    }
    return 0;
}

```
## Write a C program to create a thread that searches for a given number in an array?

```c
#include <stdio.h>
#include <pthread.h>

int a[100], n, key, found = 0;

void* search(void* arg) {
    for (int i = 0; i < n; i++)
        if (a[i] == key) found = 1;
    return NULL;
}

int main() {
    pthread_t t;
    scanf("%d", &n);
    for (int i = 0; i < n; i++) scanf("%d", &a[i]);
    scanf("%d", &key);

    pthread_create(&t, NULL, search, NULL);
    pthread_join(t, NULL);

    printf(found ? "Found\n" : "Not Found\n");
    return 0;
}

```
## Develop a C program to create a thread that reverses a given string?

```c
#include <stdio.h>
#include <pthread.h>

int a[100], n, key, found = 0;

void* search(void* arg) {
    for (int i = 0; i < n; i++)
        if (a[i] == key) found = 1;
    return NULL;
}

int main() {
    pthread_t t;
    scanf("%d", &n);
    for (int i = 0; i < n; i++) scanf("%d", &a[i]);
    scanf("%d", &key);

    pthread_create(&t, NULL, search, NULL);
    pthread_join(t, NULL);

    printf(found ? "Found\n" : "Not Found\n");
    return 0;
}

```
## Implement a C program to create a thread that reads input from the user?

```c
#include <stdio.h>
#include <pthread.h>

char name[100];

void* read_input(void* arg) {
    printf("Enter your name: ");
    scanf("%s", name);
    return NULL;
}

int main() {
    pthread_t t;
    pthread_create(&t, NULL, read_input, NULL);
    pthread_join(t, NULL);
    printf("Hello, %s\n", name);
    return 0;
}

```
## Write a C program to create a thread that performs addition of two matrices?

```c
#include <stdio.h>
#include <pthread.h>

int a[10][10], b[10][10], sum[10][10];
int r, c;

void* add(void* arg) {
    for (int i = 0; i < r; i++)
        for (int j = 0; j < c; j++)
            sum[i][j] = a[i][j] + b[i][j];
    return NULL;
}

int main() {
    pthread_t t;

    printf("Enter rows and columns: ");
    scanf("%d %d", &r, &c);

    printf("Enter elements of Matrix A:\n");
    for (int i = 0; i < r; i++)
        for (int j = 0; j < c; j++)
            scanf("%d", &a[i][j]);

    printf("Enter elements of Matrix B:\n");
    for (int i = 0; i < r; i++)
        for (int j = 0; j < c; j++)
            scanf("%d", &b[i][j]);

    pthread_create(&t, NULL, add, NULL);
    pthread_join(t, NULL);

    printf("Sum of matrices:\n");
    for (int i = 0; i < r; i++) {
        for (int j = 0; j < c; j++)
            printf("%d ", sum[i][j]);
        printf("\n");
    }

    return 0;
}

```
## Develop a C program to create a thread that calculates the length of a given string?

```c
#include <stdio.h>
#include <pthread.h>
#include <string.h>

char str[100];
int len = 0;

void* find_length(void* arg) {
    len = strlen(str);
    return NULL;
}

int main() {
    pthread_t t;

    printf("Enter a string: ");
    scanf("%s", str);  // reads a single word

    pthread_create(&t, NULL, find_length, NULL);
    pthread_join(t, NULL);

    printf("Length of string: %d\n", len);
    return 0;
}

```
## Write a C program to create two threads using pthreads library. Each thread should print "Hello, World!" along with its thread ID?

```c
#include <stdio.h>
#include <pthread.h>

void* print_message(void* arg) {
    pthread_t tid = pthread_self();
    printf("Hello, World! From thread ID: %lu\n", tid);
    return NULL;
}

int main() {
    pthread_t t1, t2;

    pthread_create(&t1, NULL, print_message, NULL);
    pthread_create(&t2, NULL, print_message, NULL);

    pthread_join(t1, NULL);
    pthread_join(t2, NULL);

    return 0;
}

```
## Modify the previous program to pass arguments to the threads and print those arguments along with the thread ID

```c
#include <stdio.h>
#include <pthread.h>

void* print_message(void* arg) {
    char* message = (char*)arg;
    pthread_t tid = pthread_self();
    printf("%s From thread ID: %lu\n", message, tid);
    return NULL;
}

int main() {
    pthread_t t1, t2;
    char* msg1 = "Hello from Thread 1!";
    char* msg2 = "Hello from Thread 2!";

    pthread_create(&t1, NULL, print_message, msg1);
    pthread_create(&t2, NULL, print_message, msg2);

    pthread_join(t1, NULL);
    pthread_join(t2, NULL);

    return 0;
}

```
## Write a C program to demonstrate thread synchronization using mutex locks. Create two threads that increment a shared variable using mutex locks to ensure proper synchronization?


```c
#include <stdio.h>
#include <pthread.h>

int count = 0;
pthread_mutex_t lock;

void* inc(void* arg) {
    for (int i = 0; i < 10000; i++) {
        pthread_mutex_lock(&lock);
        count++;
        pthread_mutex_unlock(&lock);
    }
    return NULL;
}

int main() {
    pthread_t t1, t2;
    pthread_mutex_init(&lock, NULL);

    pthread_create(&t1, NULL, inc, NULL);
    pthread_create(&t2, NULL, inc, NULL);

    pthread_join(t1, NULL);
    pthread_join(t2, NULL);

    pthread_mutex_destroy(&lock);
    printf("Final count: %d\n", count);
    return 0;
}

```
## Extend the previous program to use semaphore instead of mutex locks for thread synchronization?

```c
#include <stdio.h>
#include <pthread.h>
#include <semaphore.h>

int count = 0;
sem_t s;

void* inc(void* arg) {
    for (int i = 0; i < 10000; i++) {
        sem_wait(&s);
        count++;
        sem_post(&s);
    }
    return NULL;
}

int main() {
    pthread_t t1, t2;
    sem_init(&s, 0, 1);

    pthread_create(&t1, NULL, inc, NULL);
    pthread_create(&t2, NULL, inc, NULL);

    pthread_join(t1, NULL);
    pthread_join(t2, NULL);

    sem_destroy(&s);
    printf("%d\n", count);
    return 0;
}

```
## Write a C program to implement the producer-consumer problem using pthreads. Create two threads - one for producing items and another for consuming items from a shared buffer?

```c
#include <stdio.h>
#include <pthread.h>

int item = 0, full = 0;
pthread_mutex_t lock = PTHREAD_MUTEX_INITIALIZER;
pthread_cond_t cond = PTHREAD_COND_INITIALIZER;

void* producer(void* arg) {
    pthread_mutex_lock(&lock);
    while (full) pthread_cond_wait(&cond, &lock);
    item = 1;
    printf("Produced: %d\n", item);
    full = 1;
    pthread_cond_signal(&cond);
    pthread_mutex_unlock(&lock);
    return NULL;
}

void* consumer(void* arg) {
    pthread_mutex_lock(&lock);
    while (!full) pthread_cond_wait(&cond, &lock);
    printf("Consumed: %d\n", item);
    full = 0;
    pthread_cond_signal(&cond);
    pthread_mutex_unlock(&lock);
    return NULL;
}

int main() {
    pthread_t p, c;
    pthread_create(&p, NULL, producer, NULL);
    pthread_create(&c, NULL, consumer, NULL);
    pthread_join(p, NULL);
    pthread_join(c, NULL);
    return 0;
}

````
## Implement a multithreaded file copy program in C. Create multiple threads to read from one file and write to another file concurrently?

```c
#include <stdio.h>
#include <pthread.h>
#include <fcntl.h>
#include <unistd.h>

int src, dest;
off_t size;

void* copy_half(void* arg) {
    int part = *(int*)arg;
    char buf[1024];
    off_t offset = part * (size / 2);
    lseek(src, offset, SEEK_SET);
    lseek(dest, offset, SEEK_SET);
    int n = read(src, buf, size / 2);
    write(dest, buf, n);
    return NULL;
}

int main() {
    src = open("source.txt", O_RDONLY);
    dest = open("dest.txt", O_CREAT | O_WRONLY | O_TRUNC, 0644);
    size = lseek(src, 0, SEEK_END);

    pthread_t t1, t2;
    int p1 = 0, p2 = 1;

    pthread_create(&t1, NULL, copy_half, &p1);
    pthread_create(&t2, NULL, copy_half, &p2);

    pthread_join(t1, NULL);
    pthread_join(t2, NULL);

    close(src);
    close(dest);
    return 0;
}

```
## Write a C program to demonstrate thread cancellation. Create a thread that runs an infinite loop and cancels it after a certain condition is met from the mmain thread?

```c
#include <stdio.h>
#include <pthread.h>
#include <unistd.h>

void* run(void* arg) {
    while (1) {
        printf("Thread running...\n");
        sleep(1);
    }
    return NULL;
}

int main() {
    pthread_t t;
    pthread_create(&t, NULL, run, NULL);

    sleep(5);  // Let the thread run for 5 seconds

    pthread_cancel(t);
    pthread_join(t, NULL);

    printf("Thread cancelled.\n");
    return 0;
}

```
## Write a C program to implement a reader-writer lock using pthreads. Create multiple reader threads and writer threads and ensure proper synchronization using the reader-writer lock?

```c
#include <stdio.h>
#include <pthread.h>
#include <unistd.h>

void* run(void* arg) {
    while (1) {
        printf("Thread running...\n");
        sleep(1);
    }
    return NULL;
}

int main() {
    pthread_t t;
    pthread_create(&t, NULL, run, NULL);

    sleep(5);  // Let the thread run for 5 seconds

    pthread_cancel(t);
    pthread_join(t, NULL);

    printf("Thread cancelled.\n");
    return 0;
}

```
## Write a C program to create a thread that prints the even numbers between 1 and 20?

```c
#include <stdio.h>
#include <pthread.h>

void* print_even(void* arg) {
    for (int i = 2; i <= 20; i += 2)
        printf("%d ", i);
    return NULL;
}

int main() {
    pthread_t t;
    pthread_create(&t, NULL, print_even, NULL);
    pthread_join(t, NULL);
    return 0;
}

```
## Develop a C program to create two threads that print odd and even numbers alternately

```c
#include <stdio.h>
#include <pthread.h>

void* print_even(void* arg) {
    for (int i = 0; i <= 20; i += 2)
        printf("%d ", i);
    return NULL;
}

int main() {
    pthread_t t;
    pthread_create(&t, NULL, print_even, NULL);
    pthread_join(t, NULL);
    return 0;
}

```
## Implement a C program to create a thread that calculates the sum of squares of numbers from 1 to 10?

```c
#include <stdio.h>
#include <pthread.h>

int sum = 0;

void* calc_squares(void* arg) {
    for (int i = 1; i <= 10; i++)
        sum += i * i;
    return NULL;
}

int main() {
    pthread_t t;
    pthread_create(&t, NULL, calc_squares, NULL);
    pthread_join(t, NULL);
    printf("Sum of squares: %d\n", sum);
    return 0;
}

```
## Write a C program to create a thread that calculates the product of numbers from 1 to 5?

```c
#include <stdio.h>
#include <pthread.h>

int product = 1;

void* calc_product(void* arg) {
    for (int i = 1; i <= 5; i++)
        product *= i;
    return NULL;
}

int main() {
    pthread_t t;
    pthread_create(&t, NULL, calc_product, NULL);
    pthread_join(t, NULL);
    printf("Product: %d\n", product);
    return 0;
}

```
## Develop a C program to create a thread that prints the first 10 terms of the Fibonacci sequence?

```c
#include <stdio.h>
#include <pthread.h>

void* print_fibonacci(void* arg) {
    int a = 0, b = 1, c;
    for (int i = 0; i < 10; i++) {
        printf("%d ", a);
        c = a + b;
        a = b;
        b = c;
    }
    return NULL;
}

int main() {
    pthread_t t;
    pthread_create(&t, NULL, print_fibonacci, NULL);
    pthread_join(t, NULL);
    return 0;
}

```
## Write a C program to create a thread that finds the maximum element in a given array?

```c
#include <stdio.h>
#include <pthread.h>

int arr[100], n, max;

void* find_max(void* arg) {
    max = arr[0];
    for (int i = 1; i < n; i++)
        if (arr[i] > max)
            max = arr[i];
    return NULL;
}

int main() {
    pthread_t t;
    printf("Enter size of array: ");
    scanf("%d", &n);

    printf("Enter %d elements: ", n);
    for (int i = 0; i < n; i++)
        scanf("%d", &arr[i]);

    pthread_create(&t, NULL, find_max, NULL);
    pthread_join(t, NULL);

    printf("Maximum element: %d\n", max);
    return 0;
}

```
## Implement a C program to create a thread that prints the ASCII values of characters in a given string?

```c
#include <stdio.h>
#include <pthread.h>

char str[100];

void* print_ascii(void* arg) {
    for (int i = 0; str[i] != '\0'; i++)
        printf("%c: %d\n", str[i], str[i]);
    return NULL;
}

int main() {
    pthread_t t;

    printf("Enter a string: ");
    scanf("%s", str);  // reads one word without spaces

    pthread_create(&t, NULL, print_ascii, NULL);
    pthread_join(t, NULL);

    return 0;
}

```
## Develop a C program to create a thread that calculates the sum of all prime numbers up to a given limit?

```c
#include <stdio.h>
#include <pthread.h>

int limit, sum = 0;

int is_prime(int n) {
    if (n < 2) return 0;
    for (int i = 2; i*i <= n; i++)
        if (n % i == 0)
            return 0;
    return 1;
}

void* calc_prime_sum(void* arg) {
    for (int i = 2; i <= limit; i++)
        if (is_prime(i))
            sum += i;
    return NULL;
}

int main() {
    pthread_t t;

    printf("Enter limit: ");
    scanf("%d", &limit);

    pthread_create(&t, NULL, calc_prime_sum, NULL);
    pthread_join(t, NULL);

    printf("Sum of primes up to %d: %d\n", limit, sum);
    return 0;
}

```
## Write a C program to create a thread that calculates the area of a circle using a given radius

```c
#include <stdio.h>
#include <pthread.h>

float radius, area;

void* calc_area(void* arg) {
    area = 3.14 * radius * radius;
    return NULL;
}

int main() {
    pthread_t t;
    printf("Enter radius: ");
    scanf("%f", &radius);

    pthread_create(&t, NULL, calc_area, NULL);
    pthread_join(t, NULL);

    printf("Area: %.2f\n", area);
    return 0;
}

```
## Develop a C program to create a thread that calculates the average of a given array of floating-point numbers?

```c

#include <stdio.h>
#include <pthread.h>

float a[100], avg;
int n;

void* avg_calc(void* arg) {
    float sum = 0;
    for (int i = 0; i < n; i++) sum += a[i];
    avg = sum / n;
    return NULL;
}

int main() {
    pthread_t t;
    scanf("%d", &n);
    for (int i = 0; i < n; i++) scanf("%f", &a[i]);

    pthread_create(&t, NULL, avg_calc, NULL);
    pthread_join(t, NULL);

    printf("%.2f\n", avg);
    return 0;
}

```
## Implement a C program to create a thread that prints the factors of a given number?

```c
#include <stdio.h>
#include <pthread.h>

int num;

void* print_factors(void* arg) {
    for (int i = 1; i <= num; i++)
        if (num % i == 0)
            printf("%d ", i);
    return NULL;
}

int main() {
    pthread_t t;
    scanf("%d", &num);

    pthread_create(&t, NULL, print_factors, NULL);
    pthread_join(t, NULL);
    return 0;
}

```
## Develop a C program to create a thread that prints the English alphabet in uppercase?

```c
#include <stdio.h>
#include <pthread.h>

void* print_uppercase(void* arg) {
    for (char c = 'A'; c <= 'Z'; c++)
        printf("%c ", c);
    return NULL;
}

int main() {
    pthread_t t;
    pthread_create(&t, NULL, print_uppercase, NULL);
    pthread_join(t, NULL);
    return 0;
}

```
## Implement a C program to create a thread that checks if a given number is a perfect square?

```c
#include <stdio.h>
#include <pthread.h>
#include <math.h>

int num;

void* check_square(void* arg) {
    int root = sqrt(num);
    if (root * root == num)
        printf("%d is a perfect square\n", num);
    else
        printf("%d is not a perfect square\n", num);
    return NULL;
}

int main() {
    pthread_t t;
    scanf("%d", &num);
    pthread_create(&t, NULL, check_square, NULL);
    pthread_join(t, NULL);
    return 0;
}

```
## Implement a C program to create a thread that checks if a given number is divisible by another given number?

```c
#include <stdio.h>
#include <pthread.h>

int a, b;

void* check_divisible(void* arg) {
    if (b == 0)
        printf("Division by zero not allowed\n");
    else if (a % b == 0)
        printf("%d is divisible by %d\n", a, b);
    else
        printf("%d is not divisible by %d\n", a, b);
    return NULL;
}

int main() {
    pthread_t t;
    scanf("%d%d", &a, &b);
    pthread_create(&t, NULL, check_divisible, NULL);
    pthread_join(t, NULL);
    return 0;
}

```
## Develop a C program to create a thread that prints the multiplication table of a given number up to a given limit?

```c
#include <stdio.h>
#include <pthread.h>

int num, limit;

void* print_table(void* arg) {
    for (int i = 1; i <= limit; i++)
        printf("%d x %d = %d\n", num, i, num * i);
    return NULL;
}

int main() {
    pthread_t t;
    scanf("%d%d", &num, &limit);  // Input: number and limit

    pthread_create(&t, NULL, print_table, NULL);
    pthread_join(t, NULL);

    return 0;
}

```
## Develop a C program to create a thread that prints the current system time?

```c
#include <stdio.h>
#include <pthread.h>
#include <time.h>

void* print_time(void* arg) {
    time_t t;
    time(&t);
    printf("Current time: %s", ctime(&t));
    return NULL;
}

int main() {
    pthread_t t;
    pthread_create(&t, NULL, print_time, NULL);
    pthread_join(t, NULL);
    return 0;
}

```
## Write a C program to create two threads that increment and decrement a shared variable, respectively, using mutex locks?

```c
#include <stdio.h>
#include <pthread.h>

int count = 0;
pthread_mutex_t lock;

void* inc(void* a) {
    pthread_mutex_lock(&lock);
    count++;
    pthread_mutex_unlock(&lock);
    return NULL;
}

void* dec(void* a) {
    pthread_mutex_lock(&lock);
    count--;
    pthread_mutex_unlock(&lock);
    return NULL;
}

int main() {
    pthread_t t1, t2;
    pthread_mutex_init(&lock, NULL);

    pthread_create(&t1, NULL, inc, NULL);
    pthread_create(&t2, NULL, dec, NULL);

    pthread_join(t1, NULL);
    pthread_join(t2, NULL);

    printf("%d\n", count);
    return 0;
}

```
## Develop a C program to create a thread that prints numbers from 10 to 1 in descending order using mutex locks

```c
#include <stdio.h>
#include <pthread.h>

pthread_mutex_t lock;

void* print_desc(void* arg) {
    pthread_mutex_lock(&lock);
    for (int i = 10; i >= 1; i--)
        printf("%d ", i);
    pthread_mutex_unlock(&lock);
    return NULL;
}

int main() {
    pthread_t t;
    pthread_mutex_init(&lock, NULL);

    pthread_create(&t, NULL, print_desc, NULL);
    pthread_join(t, NULL);

    pthread_mutex_destroy(&lock);
    return 0;
}

```


