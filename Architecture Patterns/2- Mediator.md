### **Course/Resource**  
[ASP.NET Core - Clean Architecture - Full Course](https://www.youtube.com/watch?v=gGa7SLk1-0Q)

### **Key Concepts**
- The Mediator pattern is a behavioral design pattern that manages communication between objects, ensuring clear interaction rules and streamlined logic. 
- It abstracts the logic of handling requests away from the application that initiates them, allowing for cleaner and more modular code.
- Encourages loose coupling between components, enhancing flexibility and maintainability.
- Frequently used alongside the CQRS (Command and Query Responsibility Segregation) pattern for better separation of concerns.

### **Detailed Notes**
- **Registering the Mediator**:
  - `services.AddMediatR(Assembly.GetExecutingAssembly())`
- **Reuqests, Commands and Handlers**
  - Naming conventions should imply what the request is for. (i.e GetOrderSummaryRequest.cs)
  - Keep everything at the DTO level and the request is simply a mechanism to transfer data.
  - Requests/Command
    ```
    public class GetOrderSummaryRequest : IRequest<List<OrderSummaryDTO>>{
        public OrderSummaryDTO OrderSummaryDTO {get;set;}
    }

    or

    public class CreateOrderCommand : IRequest<int>{
        public CreateOrderDTO CreateOrderDTO {get;set;}
    }

    or

    public class UpdateOrderCommand : IRequest<Unit>{ //Unit is just a void type
        public OrderDTO OrderDTO {get;set;}
    }
    ```
  - Handler (For the above would be a Query because it GETs data)
    ```
    public class GetOrderSummaryRequestHandler : IRequestHandler<GetOrderSummaryRequest, List<OrderSummaryDTO>>
    {
      //Inject IRepository references to be able to communicate with the database WITHOUT directly communicating to the Domain layer
      public Task<List<OrderSummaryDTO>> Handle(GetOrderSummaryRequest request, CancellationToken cancellationToken)
      {
        //Implementation
      }
    }

    //Similar implementations for CommandHandlers
    ``` 
  - Should only have one handler for one request/ command.

### **Best Practices**
- 
