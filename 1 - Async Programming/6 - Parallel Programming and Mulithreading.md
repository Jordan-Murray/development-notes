## **Day 5 - 24/09/2024 - 26/09/2024**

### **Course/Resource**  
[Asynchronous Programming in .NET](https://app.pluralsight.com/ilx/video-courses/5ea19dbe-1a34-4df1-8320-5c3198bcabdf)

### **Key Concepts**
- Break down a large problem and compute each piece independently
- TPL provides a way to write PLINQ.
- Using Parallel will use all of the available cores/threads for concurrent processing operations. **Execution order is not guaranteed and should not matter.**

### **Detailed Notes**
- **Parallelism**:
  - Parallel will ensure that work is distributed efficiently on the system that runs the application.
  - 12 cores/threads means 12 concurrent operations
  - Parallel programming is CPU bound and perfect for independent chunks of data
  - Non-parallel
   ```
      var stocks = new Dictionary<string, IEnumerable<StockPrice>>();

      var msft = Calculate(stocks["MSFT"]);
      var googl = Calculate(stocks["GOOGL"]);
      var aapl = Calculate(stocks["AAPL"]);
      var cat = Calculate(stocks["CAT"]);
    ```
  - Using Task.Run will move the work to **one** other thread
   ```
      var stocks = new Dictionary<string, IEnumerable<StockPrice>>();
      Task.Run(() =>

        var msft = Calculate(stocks["MSFT"]);
        var googl = Calculate(stocks["GOOGL"]);
        var aapl = Calculate(stocks["AAPL"]);
        var cat = Calculate(stocks["CAT"]);
      
      )
    ```
  - Using Parallel will use all of the available cores/threads for concurrent processing operations. **Execution order is not guaranteed and should not matter.**
  ```
      var stocks = new Dictionary<string, IEnumerable<StockPrice>>();
      var bag = new ConcurrentBag<StockCalculation>(); //We need a thread safe collection.
      Parallel.Invoke( //We could use Parallel.For or ForEach here for a better implementation!
        () => {
          var msft = Calculate(stocks["MSFT"]);
          bag.Add(msft);
        },
        () => {
          var googl = Calculate(stocks["GOOGL"]);
          bag.Add(googl);
        },
        () => {
          var aapl = Calculate(stocks["AAPL"]);
          bag.Add(aapl);
        },
        () => {
          var cat = Calculate(stocks["CAT"]);
          bag.Add(cat);
        }
      )
    ```
    - Using ParallelOptions MaxDegreeOfParallelism can be used to avoid maxing out the usage of the machine.

  - **Using Parallel and Async Principles Together**
    - Parallel blocks the calling thread until all the parallel operations are completed.
    - We can wrap our `Parallel.Invoke` call in an anonymous method of `Task.Run` to free up the calling thread. Allowing us to await the Task. However this does reduce the number of available threads for parallel processing.
  
  - **Error Handling**
    - Parallel will through an aggregate exception. (Will be caught when you wrap it in a try catch)
    - Even if one parallel operation failed, the other scheduled ones will still start. An exception isn't thrown until all operations are complete.
  
  - **Processing a Collection of Data in Parallel**
    - Break; won't stop other values in the operation from completing
    - We should use the ParallelLoopState using state.Break();
    - ```
      Parallel.For(0,100,(i,state) => {
        if(i == 50){
          state.Break;        
          //Scheduled iterations for indices lower than 50 will start.
          //Only operations for indices over 50 won't be scheduled to start.
        }
      });
      ```
    - state.Stop() will tell Parallel loop should cease execution at the systems earliest convenience.
    - state.ShouldExitCurrentIteration will be set to true if the previous iteration requested a break.


### Best Practices
- TPL and its `Task` should be the preferred way to introduce parallel programming.
- Misusing Parallel in ASP.NET can cause bad performance for everyone (e.g one user has an ongoing parallel operation, using up all cores on the server, killing everyone elses application)