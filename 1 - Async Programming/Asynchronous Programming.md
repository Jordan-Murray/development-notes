**Day 1 - 20/09/2024** 

https://app.pluralsight.com/ilx/video-courses/5ea19dbe-1a34-4df1-8320-5c3198bcabdf

# Basic

Asynchrony means we can offload the operation to a different thread. Allowing the work to occur in parallel.
The await keyword introduces a continuation, allowing you to get back to the original context (thread).
A continuation is introduced on every await. 

# More

- Threading (Low Level)
- Background Worker (Event-based async pattern)
- TPL - Async and Await (more modern)

Aynsc suited for I/O Operations

- Disk
- Memory
- Web/ API
- Database

Where as CPU bound operations youd use Parallel Programming.

.Result and .Wait will block your thread and perform the operatoin synchronously. May cause a deadlock
However in the continuation (after the await keyword) using the Result property is fine.

Async in .NET Apis allows the server to continue serving requests while the other thread deals with the async process.

**Suffixing asynchronous methods with "Async" is no longer a design guideline. GetStockPricesAsync() should be named GetStockpPrices()**
This is because modern code editors will tell you its async and needs to be awaited anyway!

**Avoid "async void" at all costs** - apart from event handlers!
Should be async Task
This is because it won't validate the success of the operation and you lose all exception handling.
