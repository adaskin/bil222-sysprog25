# Multi-Thread Programming-II

## Review: Concurrency & Parallelism
- **Concurrency**: Ability of program parts to execute independently without affecting the outcome.
  - Operations can progress simultaneously (e.g., a word processor handling keyboard and mouse events).
- **Parallel Computing**: Splitting tasks into subtasks executed simultaneously across multiple processors.

---

## Threads vs Processes
### Process
- Own virtual address space.
- Created via `fork()` (expensive).
- Used for security (e.g., Chrome tabs) or running separate programs (e.g., `gcc`).

### Thread
- Lightweight, shares memory with parent process.
- Managed via `pthread` library.
- Useful for multi-core systems, low overhead, and shared memory communication.

---

## Creating Threads
### `pthread_create` Syntax
```c
#include <pthread.h>
int pthread_create(
    pthread_t *thread,
    const pthread_attr_t *attr,
    void *(*start_routine)(void*),
    void *arg
);
```

---

- **Example**:
```c
pthread_t thread;
int arg = 10;
int r = pthread_create(&thread, NULL, my_func, &arg);
if (r != 0) perror("pthread_create");
```

---

## Thread Termination
A thread terminates when:
1. Its start function returns.
2. It calls `pthread_exit(retval)`.
   - `retval` must point to heap memory (not stack).

**Warning**: `exit()` terminates the entire process.

---

## Race Conditions
- Occurs when threads modify shared data concurrently.
- Example:
```c
int count = 0;
void *increment(void *n) {
    for (int j = 0; j < *(int*)n; j++) {
        count++; // Non-atomic operation (3 steps: load, increment, store)
    }
}
```
- **Result**: Undefined behavior due to interleaving.

---

## Race Condition Example: Bank Transaction
- Example scenario:
  - Thread 1 reads a balance of $1000, deposits $200, and updates to $1200.
  - Thread 2 reads the same balance of $1000, deposits $200, and updates to $1200.
  
**Result**: Incorrect final balance due to concurrent updates.

---

## Race Condition Example
### Code
```c
void *thread_main(void *p) {
    int x = *(int*)p;
    x += x;
    *(int*)p = x;
    return NULL;
}
int main() {
    int data = 1;
    pthread_t one, two;
    pthread_create(&one, NULL, thread_main, &data);
    pthread_create(&two, NULL, thread_main, &data);
    pthread_join(one, NULL);
    pthread_join(two, NULL);
    printf("%d\n", data); // Output could be 2 or 4 (race condition)
}
```

---

## Thread-Unsafe Functions
Examples: `asctime`, `strtok`, `strerror`.
- **Unsafe Code**:
```c
char *to_message(int num) {
    static char result[256]; // Shared state
    sprintf(result, "%d : blah", num);
    return result;
}
```
- **Safe Code**:
```c
int to_message_r(int num, char *buf, size_t nbytes) {
    snprintf(buf, nbytes, "%d : blah", num); // Caller-managed buffer
}
```

---

## Parallel Computation: Parallelizing Programs
- **Parallel computation** occurs at different levels:
  - Bit-level parallelism.
  - Instruction-level parallelism.
  - Data parallelism.
  - Task parallelism.

---

### Types of Parallelism
1. **Data Parallelism**: Same operation on different data (e.g., sum of array).
2. **Task Parallelism**: Different operations on same/different data (e.g., merge sort).

---

### Embarrassingly Parallel Problems
- Minimal effort to parallelize (e.g., applying a function to an array).
- Example: Parallel `map` function.
```c
int *map(int (*func)(int), int *arr, size_t len) {
    int *ret = malloc(len * sizeof(int));
    for (size_t i = 0; i < len; i++) ret[i] = func(arr[i]);
    return ret;
}
```

---

## Amdahl's Law
- Predicts theoretical speedup from parallelization.  
  Formula: 
  \[
  S_{\text{latency}}(s) = \frac{1}{(1 - p) + \frac{p}{s}}
  \]
  - \( p \): Parallel portion of the program.  
  - \( s \): Number of cores.  
  [Wikipedia](https://en.wikipedia.org/wiki/Amdahl%27s_law)

---


## Thread Pools
### Implementation:
- **Task Queue**:
  - A queue of function pointers.
- **Thread Pool**:
  - A group of threads running a function to execute tasks from the queue.
  - If no tasks are present, threads wait.
- Requires synchronization (e.g., mutexes).

---

## Coroutines
- **User-level concurrency** (no parallelism).
- Cooperative multitasking with multiple entry points.
- Provides concurrency but not parallelism.
- Allow tasks to be executed out of order or in changeable order.

---

- **Python Example**:
```python
def print_name(prefix):
    print(f"Searching prefix: {prefix}")
    while True:
        name = (yield)
        if prefix in name: print(name)

corou = print_name("Dear")
corou.__next__()  # Start
corou.send("Dear Atul")  # Prints "Dear Atul"
```

---

## Goroutines (Green Threads)
**Go language feature**
- Go language user-level thread routines.
- Stackless and managed by the Go runtime.

---

- **Example**:
```go
package main
import ("fmt"; "time")

func say(s string) {
    for i := 0; i < 5; i++ {
        time.Sleep(100 * time.Millisecond)
        fmt.Println(s)
    }
}
func main() {
    go say("world") // Goroutine
    say("hello")    // Main thread
}
```

---

## Next: Synchronization
- Mutexes, semaphores, condition variables.
