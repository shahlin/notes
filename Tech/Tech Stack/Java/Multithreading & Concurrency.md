# Multithreading & Concurrency
## Threading Fundamentals
### Context Switch
- Thrashing - If there's a lot of threads, the OS spends more time managing the threads than in the actual thread work
- Context switching between threads from the same process is cheaper than context switching between threads from different processes

### Thread Scheduling
- First come first serve - Problem is if there's long running threads coming first, there will be a starvation wherein other threads will just be waiting (worst for UI threads because it makes the UI unresponsive)
- Shortest job first - Problem is if we keep taking the shortest jobs first and they keep coming, the OS will never work on the large computation tasks (time consiming threads)
- Epochs - Common used way in recent OSs. Divides time into small pieces called epochs. Multiple threads run within each epoch, every thread will be assigned a time slice, but not all threads necessarily finish their execution in this time and maybe some might not even be picked. OS assigns a dynamic priority value to each thread. Usually giving the UI threads more priority

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
