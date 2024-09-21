**Day 2 - 21/09/2024** 

https://app.pluralsight.com/ilx/video-courses/5ea19dbe-1a34-4df1-8320-5c3198bcabdf

# Basic

**TPL** - Task Parallel Library
Allows for both asyncronous and parallel programming. Async requires you subscribe to that operation finishing

# More

When a method doesn't have an async alternative. We can introduce a Task.

Task.Run(() => { /*Heavy operation*/ });
- Queue this anonymous method on the thread pool for execution.

Task.Run(SomeMethod);
- Queue this method on the thread pool for execution.

**Updating UI elements can only be done from the UI thread. **

In WPF we can use the Dispatcher to queue work on the UI thread specifically.

If we don't want to use async/ await when trying to obtain a result from a Task. We can use:

Task.ContinueWith((justCompletedTask) => {
    var result = justCompletedTask.Result; //Fine to use here because we know the task has been completed!
});

- However async & await is much more readable and maintainable approach!