### **Course/Resource**  
[ASP.NET Core - Clean Architecture - Full Course](https://www.youtube.com/watch?v=gGa7SLk1-0Q)

#### **AutoMapper**
  - Convert from one data type to another
  - Usually used here in conjuction with Mediator acting as messageing between what is coming from the client and what is going to the database.
  - Generally speaking bad practise to interact directly with the domain objects/ entities.
  - We create abstractions (DTOs). Classes that look like domain objects but have restrictions on operations.
  - Setup within the Application project.
  - When setting up in DI instead of doing
    ```
        services.AddAutoMapper(typeof(MappingProfile1));
        services.AddAutoMapper(typeof(MappingProfile2));
        services.AddAutoMapper(typeof(MappingProfile3));
    
    //DO THIS INSTEAD
        services.AddAutoMapper(Assembly.GetExecutingAssembly()); //This will traverse every mappingprofile that inherits from Automapper.Profile

    ```
- Can project one object onto another. e.g UpdateOrderDTO{ Title = Order1, Cancelled = Yes}, inside DB OrderDTO (Title = Order1, Cancelled = No).
    ```
        var order = db.Get(request.UpdateOrderCommand.OrderDTO.Id)
         _mapper.Map(request.OrderDTO, order);
        await db.Update(order)

        //this will take all values of order and update them with any different values from request.OrderDTO
    ```