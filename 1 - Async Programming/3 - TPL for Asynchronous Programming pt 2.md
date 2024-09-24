## **Day 3 - 22/09/2024**

### **Course/Resource**  
[Asynchronous Programming in .NET](https://app.pluralsight.com/ilx/video-courses/5ea19dbe-1a34-4df1-8320-5c3198bcabdf)

### **Key Concepts**

Knowing when a list of tasks or one are completed can be found used TPLs ```Task.WhenAny``` or ```Task.WhenAll```. This allows tasks to operate in parralel and not be awaited one by one and run synchronously. 

### **Detailed Notes**
- **Precumputed Results of a Task**:
  - Mostly used for mocking a asynchronous operation without actually wanting to call it.
  - ```Task.CompletedTask```
  - ```Task.FromResult```
- **Process Task as They Complete**
  - **List<T> is not thread safe.** Consider using ConcurrentBag<T> instead.
  - Using a continuation within the loop of generating the parallel tasks to grab data and add it to our concurrent bag.
- **ConfigureAwait**
  - Lets the awaiter know whether we want to come back to the captured conetxt/ thread.
  - Only configures the awaiter in the method its used in!
  - Could slightly imrpove performance as it doesnt have to switch context.
  - Thread static values from the original context won't be available!
  - ASP.NET Core doesn't use synchronisation context which means **Configure Await is useless**.
  - However when building libraries, always use ConfigureAwait(false) as we never know what kind of application will be consuming and we don't care about the original context within our library methods.

### Best Practices

- **List<T> is not thread safe.** Consider using ConcurrentBag<T> instead.
- ASP.NET Core doesn't use synchronisation context which means **Configure Await is useless**.