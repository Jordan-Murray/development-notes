## **Day 5 - 24/09/2024**

### **Course/Resource**  
[Asynchronous Programming in .NET](https://app.pluralsight.com/ilx/video-courses/5ea19dbe-1a34-4df1-8320-5c3198bcabdf)

### **Key Concepts**
- Use `IProgress<T>` to report and track progress in asynchronous operations.
- `TaskCompletionSource<T>` provides control over task completion, allowing for manual task finalization.

### **Detailed Notes**
- **Report on the Progress of a Task**:
  - No out of the box implementation!
  - `IProgress<T>` provides an implmentation that invokes callbacks for each reported progress value.
  ```
  var progress = new Progress<string>();
  progress.ProgressChanged += (_, string progressValue) => {
    //Use the "progressValue" here!
    //e.g update UI with progressValue
  }

  ...

  progress.Report(completedTask.Result)
  ```
- **Using Task Completion Source**
  - `TaskCompletionSource<T>` reoresents the producer side of a `Task<T>` unbound to a delegate, providing access to the consumer side through the `Task` property.
  ```
  var tcs = new TaskCompletionSource<string>();
  Task<string> task = tcs.Task;
  //do some work
  //When finished -> tcs.SetResult(result) //Sets task to RanToCompletion
  return tcs.Task;

  ...

  await tcs.Task
  ``` 

- **Working with Attached and Detached Tasks**
  - Child tasks are tasks created within a parent task:
    ```
    Task.Run(() => { //Parent
      Task.Run()     //Child
      Task.Run()     //Child
    })
    ```
  - Using `Task.Run` is in most situation the best option when taking about `Task.Factory.StartNew`

### Best Practices

- For long running tasks use `Task.Factory.StartNew` together with the `TaskCreationOptions.LongRunning`. However using `Task.Run` is in most situation the best option.