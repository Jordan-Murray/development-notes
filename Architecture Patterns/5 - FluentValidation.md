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
    public CreateOrderDTOValidator()
    {
        RuleFor(p => p.Basket)
            .NotEmpty()
            .WithMessage("{PropertyName} is required")
            .NotNull();
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