# Strave-free-readers-writers-problem

# Explanation
```py
In the readers-writers problem, multiple readers can access the shared resource simultaneously, but only one writer can access it at a time. This is because a writer may modify the resource, and if multiple writers access it concurrently, they may overwrite each other's changes or create inconsistent data.

To ensure mutual exclusion between readers and writers, a solution is implemented using semaphores, a type of synchronization mechanism that controls access to shared resources. In this case, two semaphores are used: one to keep track of the number of active readers, and one to indicate whether a writer is currently accessing the resource.

When a writer wants to access the resource, it first acquires the writer semaphore, which prevents any new readers from starting. If there are any active readers, the writer waits until they all finish and release their locks. Then the writer can access the resource exclusively to perform its task.

When a reader wants to access the resource, it first acquires the reader semaphore. If no writers are currently accessing the resource (i.e., the writer semaphore is available), the reader can proceed to access the resource. If a writer is waiting to access the resource, the reader must wait until the writer completes its task and releases the writer semaphore. This ensures that the writer has exclusive access to the resource and that readers do not interfere with the writer's task.

When a reader finishes accessing the resource, it releases the reader semaphore, indicating that it is no longer accessing the resource. If there are any writers waiting, the last reader to leave signals the writer that it is safe to proceed. Upon completing its task, the writer releases the writer semaphore, allowing waiting readers and writers to access the resource again.

Overall, the readers-writers problem is a complex synchronization challenge that requires careful management of semaphores to ensure that readers and writers can access shared resources without interfering with each other's work.
```

# Initialisation :- 
```cpp
Semaphore in = 1
Semaphore out = 1
Semaphore wrt = 0
ctrin = 0
ctrout = 0
boolean wait = false 
```
# Working segment of readers part will be :- 

Reader :- 
```cpp
while(1){
Wait(in)
ctrin++
Signal(in)
//Critical section
Wait(out)
ctrout++
if(wait==1 && ctrin==ctrout)
    then Signal(wrt)
Signal(out)
}
```
# Working segment of writers part will be :- 

Writers :-
```cpp
while(1){
Wait(in)
Wait(out)
if(ctrin==ctrout)
   then Signal(out)
else{
   wait=1
   Signal(out)
   Wait(wrt)
   wait=0
}
//Critical section
Signal(in) 
}
```
# Algorithm :- 

This is a possible implementation of the reader-writer problem using semaphores in the context of multi-threaded programming. Here is an explanation of the algorithm step by step:

1.)Initialize the necessary semaphores and counters.

2.)A reader thread enters and waits for the "in" semaphore to be available. This ensures that only one reader thread can enter the next critical section at a time.

3.)Once the "in" semaphore is available, the reader thread increments the "ctrin" counter to record the number of reader threads waiting to enter the critical section.

4.)The reader thread signals the "in" semaphore to allow other reader threads to enter the next critical section.

5.)The reader thread enters the critical section.

6.)The reader thread waits for the "out" semaphore to be available. This ensures that multiple reader threads can execute the next critical section simultaneously.

7.)Once the "out" semaphore is available, the reader thread increments the "ctrout" counter to record the number of reader threads that have completed the critical section.

8.)If there are waiting writer threads and all reader threads have completed the critical section (i.e., "ctrin" equals "ctrout"), then the reader thread signals the "wrt" semaphore to allow a waiting writer thread to enter the next critical section.

9.)The reader thread signals the "out" semaphore to allow other reader threads to enter the next critical section.

10.)The reader thread waits for the "in" semaphore to be available to re-enter the critical section.

11.)The reader thread also waits for the "out" semaphore to be available to ensure that no reader thread can enter the next critical section until all previous reader threads have completed it.

12.)If all reader threads have completed the critical section (i.e., "ctrin" equals "ctrout"), then the reader thread signals the "out" semaphore to allow other reader threads to enter the next critical section.

13.)If not, the reader thread sets the "wait" flag to 1 and signals the "out" semaphore to allow other reader threads to enter the next critical section.

14.)The reader thread waits for the "wrt" semaphore to be available to allow a waiting writer thread to enter the next critical section.

15.)Once the "wrt" semaphore is available, the reader thread sets the "wait" flag to 0 and signals the "wrt" semaphore to allow the waiting writer thread to enter the next critical section.

16.)The reader thread exits the critical section and signals the "in" semaphore to allow other reader threads to enter the next critical section.

