### **Course/Resource**  
[ASP.NET Core - Clean Architecture - Full Course](https://www.youtube.com/watch?v=gGa7SLk1-0Q)

- **Fluent Validation**
  - Its our DTOs that need validation.
  ```
      Core
      └── Application
          └── DTOs
              └── {Feature}
                  ├── Validators
                      └── FeatureDTOValidator
  ```

  ```
  //example

  public class CreateOrderDTOValidator : AbstractValidator<CreateOrderDTO>
  {
    private readonly IOrderRepository _orderRepository;
    public CreateOrderDTOValidator(IOrderRepository orderRepository)
    {
        RuleFor(p => p.Basket)
            .NotEmpty()
            .WithMessage("{PropertyName} is required")
            .NotNull();
        
        RuleFor(p => p.StartDate)
            .LessThan(p => p.EndDate).WithMessage("{PropertyName} must be before {ComparisonValue}"); //This doesn't make sense for our use case but here just to illustrate
        
        RuleFor(p => p.Basket.ItemId)
            .MustAsync(async (id,token) =>
            {
                var itemExists = await _orderRepository.Exists(id);
                return itemExists;
            }).WithMessage("{PropertyName} does not exist")
    }
  }
  ```

    ```
    // Done in the Handler in CQRS
    var validator = mew CreateOrderDtoValidator();
    var validationResult = await validator.ValidateAsync(request.orderDTO);
    if(validationResult.IsValid == false)
    {
        throw new Exception()
    }
    ```

- If we have the same rules repeated across multiple DTOs we can create an InterfaceDTO and then create an IDTOValidator : AbstractValidator<IDTO>
- If we want a class to apply all validation rules from another validator AND some more we can do the following:
```

public class OrderDTOValidator : AbstractValidator<OrderDTO>
{
    public OrderDTOValidator()
    {
        Include(new CreateOrderDtoValidator());
    }
} 

```