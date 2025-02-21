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
  - If a function works with a base type, it should work with any subclass without any unexpected side effects.

- **Interface Segregation Principle**
  - Your interfaces should be segregated so that nobody who implements your interface has to implement functions they don't need.
  ```
    public interface IMachine
    {
        void Print(Document d);
        void Scan(Document d);
        void Fax(Document d); 
    }

    public class MultiFunctionPrinter : IMachine
    {
        //Each method has its proper implementation and everything is cool
    }

    public class DumbPrinter : IMachine 
    {
        public void Print(Document d)
        {
            //Implementation
        }

        public void Scan(Document d)
        {
            throw new NotImplementedException();
        }

        public void Fax(Document d)
        {
            throw new NotImplementedException();
        }

        //Having these unnecessary not implemented methods is bad! 
    }

  ```

- **Dependency Inversion Principle**
    - High level parts of a system should not depend on low level parts directly. They should depend on some kind of abstraction.
    - Low level class like `Business` with a list of `employees` maybe a depended of another class `Auditor`.
      - This Auditor class may want to preform some research on a businesses employees, so it accesses the employees property directly and preforms its research
      - This is a difficult situation if the business goes through a restructuring of its data and changes its processes, it means the Auditor has to start again and recompile all its research 
      - Instead the business should implement a HR department (interface) to do the sort of research third parties might want. 
      - The auditor would then inject this HR class into its own and use the abstraction to get its data.
    ```

    public interface IHR
    {
        IEnumerable<Employee> GetEmployees();
    }

    // Business now exposes employee data through the IHR abstraction
    public class BusinessWithHR : IHR
    {
        private List<Employee> _employees = new List<Employee>();

        public void AddEmployee(Employee employee)
        {
            _employees.Add(employee);
        }

        public IEnumerable<Employee> GetEmployees()
        {
            return _employees;
        }
    }

    // Auditor now depends on the IHR abstraction rather than on Business directly
    public class AuditorWithDIP
    {
        private readonly IHR _hr;

        public AuditorWithDIP(IHR hr)
        {
            _hr = hr;
        }

        public void Audit()
        {
            foreach (var employee in _hr.GetEmployees())
            {
                Console.WriteLine($"Auditing employee: {employee.Name}");
            }
        }
    }


    ```