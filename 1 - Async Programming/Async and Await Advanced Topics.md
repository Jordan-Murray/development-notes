## **Day 4 - 23/09/2024**

### **Course/Resource**  
[Asynchronous Programming in .NET](https://app.pluralsight.com/ilx/video-courses/5ea19dbe-1a34-4df1-8320-5c3198bcabdf)

### **Key Concepts**
- `IAsyncEnumerable<T>` enables asynchronous iteration, making each item available as it’s processed.
- Using `yield return` in combination with `IAsyncEnumerable<T>` allows for streaming of data to the consumer as it becomes available.
- `ObservableCollection<T>` can be used to consume `IAsyncEnumerable<T>` by iterating over the stream with `await foreach`.


### **Detailed Notes**
- **Async Streams**:
  - ```IAsyncEnumerable<T>``` allows us to define we now have a asynchronous iteration. Making each item available as its processed.
  - Using ```yield return``` with the IAsyncEnumerable will signal to the iterator using this enumerator that it has an item to process.
  - Consuming an IAsyncEnumerable can be done by using an ```ObservableCollection<T>```
   ```
    var data = new ObservableCollection<Data>();
    var service = new DataServer();
    var enumerator = service.GetAllData();
    await foreach(var price in enumerator)
    {
      data.add(price);
    }
    ```
  - If a class derives from IAsyncDisposable it means we can use the await keyword when using the service. Allow the garbage collection to dispose of the service once we're done with our asynchronous work.
  ```await using var service = new Service()```

- **Async and Await Implications**
  - Introducing the async keyword generates a state machine.
    - Keeps track of tasks
    - Executes the continuation
    - Provides the continuation with a result
    - Handles context(thread) switching
    - Reports errors
  - In really high performance applications it may be an idea to look at reducing the number of state machines.
  - Can be done by removing async await key words and having methods return a Task and have the top level caller be the one to do the awaiting.
  
- **Deadlocking**
  - A deadlock may occur if two threads depend on each other and one is blocked.
  - **The state machine runs on the calling thread**


### Best Practices

- Don't use .Wait or Result properly when not awaiting asynchronous tasks!