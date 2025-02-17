### **Course/Resource**  
[Design Patterns in C# and .NET](https://www.udemy.com/course/design-patterns-csharp-dotnet)

- **Overview**
  - Introduced by Robert C Martin.  

- **Single Responsibility Principle**
    - A class should have only 1 reason to change. 
    - Imagine a Journal class with several methods; AddEntry, RemoveEntry and a override ToString()
    - Now say you may want to save this Journal to the disk. Most would add a Save and a Load method to the Journal class.
    - This would add too much responsibility to the Journal class. 
    - Instead we would create a new class called Persistence for example with the SaveToFile and LoadFromFile methods.
    - This creates a separation of concerns.

- **Open-Closed Principle**
  - Systems should be open for extension but closed for modification. 
  - By implementing interfaces.
  - Imagine you have a product filter class that allows customers to filter products by size
    - You're then asked to allow customers to filter by colour. 
    - You're then asked to allow customer to filter by size AND colour. (Going back into the Product filter class and extending it again and again).
  - Instead implement the below
    ```
    public interface ISpecification<T>
    {
        bool IsSatisfied(T t);
    }

    public interface IFilter<T>
    {
        IEnumberable<T> Filter(IEnumerable<T> items, ISpecification<T> spec);
    }

    public class ColourSpecification : ISpecification<Product>
    {
        private Colour colour

        public ColourSpecification(Colour colour)
        {
            this.colour = colour;
        }

        public bool IsSatisfied(Product product)
        {
            return product.Colour == colour;
        }
    }

    public class SizeSpecification : ISpecification<Product>
    {
        //Same here
    }

    // Trivial implementation of ProductFilter : IFilter<Product>

    public class AndSpecification<T> : ISpecification<T>
    {
        private ISpecification first, second;

        ctor 

        public bool IsSatisfied(T t) 
        {
            return first.IsSatisfied(t) && second.IsSatisfied(t);
        }
    }

    ```
    - See we no longer have a class ProductFilter than we have to keep going back and changing every time requirements change.

- **Liskov Substitution Principle**
  - You should be able to substitute a base type for a subtype.   