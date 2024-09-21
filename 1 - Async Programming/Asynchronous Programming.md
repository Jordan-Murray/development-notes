## **Day 1 - 20/09/2024**

### **Course/Resource**  
[Asynchronous Programming in .NET](https://app.pluralsight.com/ilx/video-courses/5ea19dbe-1a34-4df1-8320-5c3198bcabdf)

### **Key Concepts**
- Asynchronous programming allows offloading work to different threads to enable parallel execution.
- The `await` keyword introduces a continuation, allowing the operation to return to the original context (thread).
- `Async` methods are suited for I/O-bound operations (disk, memory, web, database), while **parallel programming** is better for CPU-bound tasks.

### **Detailed Notes**
- **Asynchronous Programming Basics**:
  - The `await` keyword creates a continuation that resumes on the original thread.
  - `.Result` and `.Wait()` should be avoided in asynchronous operations as they block the calling thread and could lead to deadlocks. After the `await`, using `.Result` is safe because the task has completed.
  
- **I/O vs. CPU-bound operations**:
  - I/O-bound tasks should use asynchronous programming (`async/await`), while CPU-bound tasks should use parallel programming.

- **Best Practices**:
  - Avoid **`async void`** except in event handlers. Use **`async Task`** to ensure proper exception handling.
  - Suffixing methods with `Async` is no longer necessary as modern IDEs indicate asynchronous operations.

### **Code Examples**
```csharp
public async Task FetchDataAsync()
{
    // Offload I/O-bound work (e.g., fetching from a web API)
    await GetWebDataAsync();
    // Continue after the async operation is complete
}
```

### Best Practices

- Avoid .Result and .Wait() to prevent blocking the thread and causing deadlocks.
- Use async Task instead of async void for exception handling.
- Async methods should not require the Async suffix in method names as IDEs show the asynchronous nature.