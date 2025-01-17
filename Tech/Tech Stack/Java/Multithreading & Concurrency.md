# Multithreading & Concurrency
## Threading Fundamentals
### Context Switch
- Thrashing - If there's a lot of threads, the OS spends more time managing the threads than in the actual thread work
- Context switching between threads from the same process is cheaper than context switching between threads from different processes

### Thread Scheduling
- **First come first serve** - Problem is if there's long running threads coming first, there will be a starvation wherein other threads will just be waiting (worst for UI threads because it makes the UI unresponsive)
- **Shortest job first** - Problem is if we keep taking the shortest jobs first and they keep coming, the OS will never work on the large computation tasks (time consiming threads)
- **Epochs** - Common used way in recent OSs. Divides time into small pieces called epochs. Multiple threads run within each epoch, every thread will be assigned a time slice, but not all threads necessarily finish their execution in this time and maybe some might not even be picked. OS assigns a dynamic priority value to each thread. Usually giving the UI threads more priority

### Why Multi-Process Architecutre over Multithreaded?
- Security and stability are of higher importance
- Tasks are unrelated to each other

## Thread Coordination
### Two Ways of Creating a Thread
1. Inheriting Thread Class / Runnable Class
    ```java
    class MyThread extends Thread {
        @Override
        public void run() {
            // Do some work   
        }
    }
    ```
2. Implementing Runnable
    ```java
    class Example {
        public void main() {
            Thread thread = new Thread(new Runnable() {
                @Override
                public void run() {
                    // Do some work
                }
            });
        }
    }
    ```

### When can we interrupt a thread?
1. If the thread executing a method throws an InterruptedException
2. If the thread is handling the interrupted signal explicitly using `Thread.currentThread().isInterrupted()` method for example. Usually you would put these interrupt checks in the parts of the code that is taking long to execute

### Daemon Threads
- These are low priority threads (usually used as background threads)
- These threads do not block our main thread from terminating
- In case there's a daemon thread running and the main thread terminates, the daemon thread will be terminated automatically as well. Even if no interrupt handling was implemented
- To achieve this, you need to set the daemon property `thread.setDaemon(true)`

### Thread.join()
- `thread.join` waits for the read to finish executing
- You can also specify a timeout incase you have long running threads. Example: `thread.join(2000)`. This will wait for the thread for 2 seconds. If the thread does not complete in the given timeout, it will not wait for the thread and continue the main thread execution. However, this does not mean that the thread is terminated. It might still be running.
- If you need the thread to be terminated after the timeout and once the main thread is killed, we can just make it a daemon thread
- The `join` method throws `InterruptedException` which needs to be handled

## Performance Optimization
### Latency
- Splitting tasks and running them in different threads can improve latency
- When splitting tasks into separate threads, always keep in mind the cost of it
- Sometimes, single threaded executions are faster than multithreaded ones
- More threads than cores can be counterproductive
- Cost of parallelization and aggregation:
    - Breaking task into multiple tasks
    - Thread creation, passing tasks to threads
    - Time between `thread.start()` to thread getting scheduled by OS
    - Time until the last thread finishes and signals
    - Time until the aggregating thread runs
    - Aggregation of subresults into single result
- Summary: We get lower latency if we use the same number of threads as physical cores. After that, the increase in threads does not add any benefit

### Throughput
- Throughput measures the number of tasks completed in a given period
- Measured in tasks per time unit
- Can use Apache JMeter to test throughput or performance in general
- Summary: We get higher throughput if the number of threads is same as physical cores (and then a little more if the count of threads is same as number of virtual cores). After that, the increase in threads does not provide any improvement

## Data Sharing b/w Threads
### Stack
- Each thread has it's own stack
- When the execution gets into a method, a stack frame is created for that method
- The stack frame contains all the local variables to that method
- The data from a stack frame is not shared with another stack frame
- Once the method returns, the stack frame is deleted/popped - Follows last in first out principle of stacks
- If we go deep in the method calling hierarchy (like when we use recursion), we may get a StackOverflow exception

### Heap
- Process specific data is stored in the heap and is accessible by all threads
- Any object that is instantiated with the `new` keyword is stored in a heap
- Example: String, Object, Collection
- Members/Properties of a class are stored in the heap
- Static properties are also stored in the heap
- Heap is managed by garbage collector
- References to objects can be allocated in a Stack while the object can be present in the heap. However if the references are members of a class (class property), they can be allocated in a heap

### Resource Sharing
- What is a resouce?
    - Variables
    - Data structure
    - File or connection handles
    - Message or queues
    - Any objects
- Sharing resources can be important to efficiently/concurrently process the incoming requests. For examples, in an HTTP server, there might be _n_ requests coming in and all of them might be accessing the database (the shared resource). If each request had to wait for the other request to finish accessing the database, the application would become slow.
- One of the challenges in resource sharing:
    - If multiple threads access and modify a shared resource (like a class property), the value stored might be incorrect
    - For example, if a class property is being incremented (`var++`) by multiple threads, there might be inconsistent result
    - This is because, an increment process is 3 steps.
        1. Get the value of the variable
        2. Increment the value of the variable by 1
        3. Store the incremented value back in the variable
    - Threads might be moved to a sleeping state while others run at any point in the above 3 steps. Hence there might be inconsistent results.
    - This is an example of a non atomic operation

## Critical Section & Synchronization
### Critical Section
- Critical section is a block of code that we want to protect from threads other than the one that's currently executing it
- For example, an increment operation. If Thread A is executing the incrementing operation, we don't want any other threads executing it. Other threads must wait until Thread A is done executing. Once it's done, the next thread can access this critical section

### Synchronized - Monitor
- The `synchronized` keyword in Java allows us to make sure that a method is only accessed by a single thread at a given time
- If multiple methods in a class are `synchronized`, all of those methods can only be accessed by a single thread. That's because the `synchronized` is applied on an object. It's like a door to a room. If the room door is locked, all the doors inside that room are locked too. This way of locking is called a **Monitor**.
    ```java
    public class Example {
        private int items = 0;

        public synchronized void increment() {
            items++;
        }
        
        public synchronized void decrement() {
            items--;
        }
    }
    ```

### Synchronized - Lock
- We can also use synchronized on a specific block of code instead of an entire method
- The benefits of this is that, we're not locking the entire method. We're only locking specific blocks.
- For example, we can create 2 lock objects and use the `synchronized` block on each of them in different code blocks. This would mean that if Thread A is using the block locked by lock 1, Thread B can still access the block locked by lock 2
- Specifying `synchronized(this)` is same as the first way wherein the entire object is locked
    ```java
    public class Example {
        private int items = 0;
        private Object lock1 = new Object();
        private Object lock2 = new Object();

        public void increment() {
            synchronized(lock1) {
                items++;
            }
        }
        
        public void decrement() {
            synchronized(lock2) {
                items--;
            }
        }
    }
    ```

### Why not `synchronized` everything?
- Excessive synchronization can be bad as it's just as running the application in a single threaded way. Since only one thread can perform operations on a class if all of it's methods have `synchronized`
- Additionally, it's worse than single threaded applications because there's the cost of spinning up a thread and managing it (as seen in the above sections)

### Atomic Operations
1. All reference assignments are atomic (like setters)
2. All assignments to primitive types are safe except `long` and `double` - Read/Writes to int, short, byte, float, char, boolean are atomic
3. All assignments to `long` and `double` using the `volatile` keyword (Note: only assignments are atomic, operations on it are not)

## Three Common Issues in Multithreaded Applications
### Race Condition
- Condition where multiple threads are accessing a shared resource
- At least one of the thread is modifying the shared resource
- The timing of threads' scheduling may cause incorrect results
- The core of the problem is non atomic operations performed on the shared resource
- Using `synchronized` solves the issue of race condition but has a performance penalty
- For `long` and `double` assignments, we can use `volatile`

### Data Race
- When a program contains two conflicting accesses that are not ordered by a happens-before relationship, it is said to contain a data race
- Basically, compilers sometimes execute instructions in a non-linear order for performance reasons. For example, the code below:
    ```java
    public class Example {
        private int x = 0;
        private int y = 0;
    
        private void increment() {
            x++;
            y++;
        }
        
        private void checkDataRace() {
            if (y > x) {
                throw new Exception("Data race occurred");
            }
        }
    }
    ```
- Technically, the `checkDataRace` should never throw an error because `x` should always be greater than `y`. But since the order of execution is not guaranteed, it is possible that we get an exception
- This is called a Data Race
- Using `synchronized` solves the issue of race condition but has a performance penalty
- Instead, we can use the `volatile` keyword to avoid Data Races

> Note: As a rule of thumb, every shared variable (modified by at least one thread) should be either (a) guarded by a `synchronized` block or any type of lock (b) or declared `volatile`

### Deadlock
- In a simple scenario, a deadlock occurs when Thread A has locked Resource A and is waiting to use Resource B but Thread B has locked Resource B and is waiting to use Resource A
- So both threads are waiting on each other and will always be stuck in this state
- This is applicable with more threads too

![Screenshot 2025-01-08 at 9 01 04 PM](https://github.com/user-attachments/assets/210683a5-613d-4e54-86b0-b4a6946cb75c)

- Conditions for a deadlock to occur:
    - **Mutual Exclusion** - Only one thread can have exclusive access to a resource
    - **Hold and Wait** - At least one thread is holding a resource and is waiting on another resource
    - **Non-preemptive allocation** - A resource is released only after the thread is done using it
    - **Circular wait** - A chain of at least two threads each one is holding one resource and waiting for another resource
- If the above conditions are all true, it's a deadlock situation
- One of the solutions is to avoid the circular wait. Which means that locks need to be acquired in the same order in every operation. For example, if an `add` method locks Resource A and Resource B sequentially, the `subtract` method should lock them in the same order so that, if Thread A has locked the Resource A, then Thread B can not continue until it has acquired Resource A

## Advanced Locking
### Reentrant Lock
- Similar to the `synchronized` block but we have to lock and unlock the block explicitly
- Disadvantage: We might forget to unlock a certain block of code once locked. Or there might be an unhandled exception that occurs which makes the unlock code unreachable
- Solution to the above disadvantage: Make sure to surround the critical section in a `try` block and unlock the lock in the `finally` block
    ```java
    lockObject.lock();
    try {
        useSharedResource();
    } finally {
        lockObject.unlock();
    }
    ```

#### Benefits
1. Provides more control over locking
2. Provides more operations like:
    a. `getQueuedThreads()`
    b. `getOwner()`
    c. `isHeldByCurrentThread()`
    d. `isLocked()`
3. By default, both the `ReentrantLock` as well as `synchronized` keyword do not guarantee fairness. But with `ReentrantLock` we can enable fairness explicitly (though it comes with a cost)
4. Provides the `lockInterruptibly()` method, which allows us to interrupt a thread (that is suspended) waiting to acquire a lock
5. Provides the `tryLock()` method, which allows us to try and acquire the lock. If the lock is available, it returns `true`. If not, it returns `false` and does not get suspended, instead it moves on to the next instruction. The `tryLock()` method will never block execution
    ```java
    if (lockObject.tryLock()) {
        try {
            useSharedResource();
        } finally {
            lockObject.unlock();
        }
    } else { ... }
    ```
### ReentrantReadWriteLock
- As long as they're not modifying the data, multiple reader threads should be able to access a shared resource at a given time
- `synchronized` and `ReentrantLock` do not allow multiple readers to access a shared resource concurrently
- The ReadWriteLocks are mostly not required if the read operations are small and quick
- But if there are way too many read operations and they are not as fast, using `synchronized` or `ReentrantLock` can impact performance
- Multiple reader threads can acquire a `readLock` but only a single writer thread can acquire a `writeLock`
- If a `writeLock` is acquired, no thread can acquire a `readLock`
- If at least one thread holds a `readLock`, no thread can acquire a `writeLock`

## Inter-thread Communication
### Semaphore
- Can be used to restrict the number of 'users' to a particular resource or a group of resources
- Unlike the locks that allow only one 'user' per resource
- The semaphore can restrict any given number of users
- Example: A parking lot with 8 spaces. We can allow up to 8 cars to enter the lot and park their cars. If all 8 spaces are taken, the next car to come needs to wait until one of the space is free
- `Semaphore` is initialized with number of permits
- A thread can acquire/release more than 1 permit at a time
- Any thread can release a semaphore (no ownership)
- Calling acquire on a semaphore that has already given all of its permits will result in blocking the thread until a semaphore is released by another thread

### Condition Variable
- Check condition, if it is not met, suspend the thread until the condition is met (mostly modified by another thread to meet the condition)
    ```java
    ReentrantLock lock = new ReentrantLock();
    Condition condition = lock.newCondition();
    ```
- We can use the `await()` method on the condition to unlock a lock and wait until it is signalled. Basically, the await() method will unlock and suspend the thread until another thread signals it to run again (then it acquires the lock and runs again)
- The `signal()` method on the condition wakes up a thread that's waiting on it. Note that, if multiple conditions are waiting for the signal, only one thread will process the signal and continue execution. To signal all threads, we can use `signalAll()`

### Object Signalling
- This is similar to the Condition Variable
- Instead of `await()`, `signal()` and `signalAll()`, in case of Object, we use `wait()`, `notify()` and `notifyAll()`
- We can use this on any object as all classes in Java inherit from the `Object` class

## Lock-Free Algorithms
### Issues with locks
1. Deadlocks are generally unrecoverable. More locks in the application, higher chances of deadlocks
2. Multiple threads using the same lock. If one thread holds the lock for very long, it will slow down all other threads
3. Priority Inversion - Two threads sharing a single lock. If low priority thread acquires the lock, then the high priority thread will be blocked until the low priority thread releases the lock
4. Thread not releasing a lock. If a thread dies, gets interrupted or forgets to release a lock. Blocks all threads forever. Usually solved with additional code like wrapping code around try/finally blocks
5. Performance overhead over threads contending to acquire a lock
    - Thread A acquires a lock
    - Thread B tries to acquire a lock and gets blocked
    - Thread B is scheduled out (context switch)
    - Thread B is scheduled back (context switch)

### CompareAndSet
- Available in all Atomic classes
- Compilesd into an atomic hardware operation
- Basically, it's a 2 step process - `compareAndSet(T expectedValue, T newValue)`:
    - Assigns new value if `current value` == `expected value`
    - Ignores the new value if the `current value` != `expected value`

### AtomicInteger
- Allows handling integer operations like `counter++` in an atomic thread-safe manner
- The `AtomicInteger` class provides methods to perform such operations. Example: `incrementAndGet()`
- It's simpler than using locks/synchronization
- No need to worry about race condition or data races
- Note that, **only the operation itself is atomic, there's still race condition between 2 separate atomic operations**
- `AtomicInteger` should only be used when atomic operations are needed. If used only by a single thread, it might add additional performance overhead

## Virtual Threads
- Introduced in JDK 21
- Just like any other Java object on the heap
- Platform threads in java (`Thread` class) are directly mapped to an OS thread. One-to-one relationship
- The JVM creates a pool of platform threads which the virtual threads can mount onto. For example, if there are 4 virtual threads, the JVM might create 2 platform threads and mount the one performing the task to the platform thread. Once the virtual thread execution is blocked, JVM will unmount the virtual thread from the platform thread and save the stack snapshot on the  heap. Meanwhile, it will mount another virtual thread and continue execution. Once the blocked thread is unblocked, it will be mounted to the platform thread again
- The platform thread containing a virtual thread is called a carrier thread
- The developers have very little control over the carrier threads and scheduling them
- If the virtual threads contains only CPU operations, it provides no performance benefit
- If the virtual threads contains operations that are blocking, it improves performance
- It's important to note that, if call took T seconds to execute before, it will still take the same time with virtual threads. There will be no impact on the latency. The only performance benefit to using virtual threads is gaining throughput
- Thread per task with platform threads introduces context switches
- Thread per task with virtual threads have only mounting/unmounting overhead

### Best Practices
- Never create fixed size pools of virtual threads (prefer Executors.newVirtualThreadPerTaskExecutor)
- Virtual threads are always **daemon threads**. An attempt to set them as non daemon threads will throw an exception
- Priorities cannot be set for virtual threads. The `setPriority(...)` doesn't do anything

# References
- [Java Multithreading, Concurrency & Performance Optimization Course on Udemy](https://www.udemy.com/course/java-multithreading-concurrency-performance-optimization/)
- [Deadlock example image](https://jojozhuang.github.io/assets/images/programming/2412/deadlock.png)
