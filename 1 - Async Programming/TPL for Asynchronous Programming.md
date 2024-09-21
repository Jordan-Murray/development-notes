## **Day 2 - 21/09/2024**

### **Course/Resource**  
[Asynchronous Programming in .NET](https://app.pluralsight.com/ilx/video-courses/5ea19dbe-1a34-4df1-8320-5c3198bcabdf)

### **Key Concepts**
- The **Task Parallel Library (TPL)** allows for both asynchronous and parallel programming.
- Async programming requires subscribing to the operation finishing (`await`).

### **Detailed Notes**
- **Task.Run**:
  - When a method doesn’t have an async alternative, you can introduce a `Task`.
  - `Task.Run(() => { /*Heavy operation*/ });` queues the anonymous method for execution on the thread pool.
  - `Task.Run(SomeMethod);` queues a method on the thread pool for execution.

- **Updating UI Elements**:
  - UI elements can only be updated from the **UI thread**.
  - In **WPF**, the `Dispatcher` can be used to queue work on the UI thread specifically.

- **Task Continuations**:
  - To avoid using `async/await` when obtaining a result from a `Task`, you can use `Task.ContinueWith()`.
  - Example:
  ```csharp
  Task.ContinueWith((justCompletedTask) => {
      var result = justCompletedTask.Result; // Safe to use here because we know the task has completed!
  });
    ```
  - ContinueWith executes when the Task completes, regardless of whether the task was successful, failed, or canceled.


### Best Practices

- Task.ContinueWith can be useful, but async/await is much more readable and maintainable.