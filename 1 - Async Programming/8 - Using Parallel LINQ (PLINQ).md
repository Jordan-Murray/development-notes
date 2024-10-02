## **Day 7 - 02/10/2024**

### **Course/Resource**  
[Asynchronous Programming in .NET](https://app.pluralsight.com/ilx/video-courses/5ea19dbe-1a34-4df1-8320-5c3198bcabdf)

### **Key Concepts**
- Parallelize your LINQ to speed up the execution but don't overuse AsParallel() as it adds a fair bit of overhead.
- Don't assume queries will automatically run faster. Performance improvements will be more noticeable on large 

### **Detailed Notes**
- **Introducing Parallel LINQ and How to Best Use It**:
  - Parallelize your LINQ to speed up the execution.
  - "Parallel implementation of the Language-Integrated Query (LINQ) pattern"
  ```
    IEnumerable<T> source = ...
    ParallelQuery<T> = source.asParallel(); //Uses all available resources to process the query
  ```
  ```
    var query = source
        .AsParallel()
        .WithCancellation(token)
        .WithDegree0fParallelism(2)
        .WithExecutionMode(ParallelExecutionMode. ForceParallelism) //Forces Parallelism (only do this is you're absolutely certain it will run faster.)
        .WithMergeOptions(ParallelMergeOptions.Default)
        .Select(Compute);
  ```
  - All your LINQ operations can't be parallelized. You can use .AsSequential to go from a Parallel IEnumberable to a sequential one mid method chain.
- **Creating a Parallel Language Integrated Query**
  - Good free VS extension for Concurrency Visualization! (Concurrency Visualizer for Visual Studio 2022)
  - Use AsOrdered() to enforce we'll be persisting the order after processing, but it will add some overhead.

### Best Practices
- Don't overuse AsParallel() as it adds overhead.
- Really you should never use ForceParallelism.