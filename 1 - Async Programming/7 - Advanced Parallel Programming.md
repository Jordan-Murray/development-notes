## **Day 6 - 27/09/2024**

### **Course/Resource**  
[Asynchronous Programming in .NET](https://app.pluralsight.com/ilx/video-courses/5ea19dbe-1a34-4df1-8320-5c3198bcabdf)

### **Key Concepts**
- Break 

### **Detailed Notes**
- **Working with Shared Variables**:
  - Be careful when sharing variable and resources in a parallel process as you will run into race conditions.
  - Always prefer atomic operations over lock when possible as its less overhead and performs faster.
    ```
    Parallel.For(0, 100, (i) => {
      lock(syncRoot) {
        //If another thread has locked the "door" the thread 
        //will be blocked until the "door" is opened.
        
        total += Compute(i)
      }
    });
    ```
  - **Only lock for as short of a time as possible. Do as little work as possible inside the lock** i.e Better implementation would be:
    ```
    Parallel.For(0, 100, (i) => {
      var result = Compute(i)
      lock(syncRoot) {
        total += result;
      }
    });
    ```
  - Be careful when adding a lock - may lead to deadlocks, especially when dealing with nested locks. 

- **Performing Atomic Operations**
  - Using Interlocked
    - Interlocked.Increment(ref int thisValue);
    - Interlocked.Decrement(ref int thisValue);
    - Interlocked.Add(ref int toBeUpdated, int withValue);
    - Interlocked.Add(ref long toBeUpdated, long withValue);
    - T Interlocked.Exchange<T>(ref T source, T newValue);
  - You cannot perform atomic operations with decimals, only integers.
  - Interlocked uses less instructions and is faster than a lock.

- **Deadlocks with Nested Locks**
  - Deadlock - Multiple threads fighting over the same resource
  - Avoiding a Deadlock
    - Don't share lock objects for multiple shared resources
    - Use one lock object for each shared resources
    - Give your lock object a meaningful name
    - Avoid using `string`, `typeof()`,`this` as a lock!

- **Cancel Parallel Operations**
  -  

### Best Practices
- Do as little work as possible within a lock!
- Use one lock object for each shared resources.
- Avoid nested locks and shared locks!