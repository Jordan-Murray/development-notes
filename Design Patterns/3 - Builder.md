### **Course/Resource**  
[Design Patterns in C# and .NET](https://www.udemy.com/course/design-patterns-csharp-dotnet)

- **Overview**
  - When construction gets a little bit too complicated.  
  - When piecewise object construction is complicated, provide an API for doing it succinctly.
  - Below is an example of a FluentBuilder.


- **Motivation**
  - Some objects are simple and can be created in a single constructor call.
  - Other objects require a lot of ceremony to create
  - Having an object with 10 constructor arguments is not productive, you may mess up 
  - Opt for piecewise construction
  - Builder provides an API for constructing an object piece bny piece.

- **Without Builder**
    - As the number of properties increases, the constructor parameters pile up. Remembering the order and purpose of each parameter becomes a guessing game.
    - When someone reads the constructor call, it’s not immediately clear which value corresponds to which property.
    - If you need to add or change an option later, you must update the constructor (and possibly create new overloads), which can lead to more code churn and potential bugs.
  ```

    public class Pizza
    {
        public string Dough { get; set; }
        public string Sauce { get; set; }
        public string Topping { get; set; }
        public bool ExtraCheese { get; set; }
        public int Size { get; set; } // in inches

        public Pizza(string dough, string sauce, string topping, bool extraCheese, int size)
        {
            Dough = dough;
            Sauce = sauce;
            Topping = topping;
            ExtraCheese = extraCheese;
            Size = size;
        }

        public override string ToString() =>
            $"Pizza with {Dough} dough, {Sauce} sauce, {Topping} topping, " +
            $"{(ExtraCheese ? "extra cheese" : "no extra cheese")}, size {Size}\".";
    }

    //Usage
    var pizzaWithoutBuilder = new Pizza("thin crust", "tomato basil", "pepperoni", true, 12);
    Console.WriteLine(pizzaWithoutBuilder);


  ``` 

- **With Builder!**
  - This allows you to chain methods in a clear, readable way.
  - You assemble the final product gradually. No more guessing what 10 constructor parameters might mean.
  - The Pizza class doesn’t need to know about the construction details. The PizzaBuilder takes care of all the assembly logic, keeping your domain objects clean.
```

    public class Pizza
    {
        public string Dough { get; set; }
        public string Sauce { get; set; }
        public string Topping { get; set; }

        public override string ToString() =>
            $"Pizza with {Dough} dough, {Sauce} sauce, and {Topping} topping.";
    }

    public class PizzaBuilder
    {
        private readonly Pizza _pizza = new Pizza();

        public PizzaBuilder WithDough(string dough)
        {
            _pizza.Dough = dough;
            return this;
        }

        public PizzaBuilder WithSauce(string sauce)
        {
            _pizza.Sauce = sauce;
            return this;
        }

        public PizzaBuilder WithTopping(string topping)
        {
            _pizza.Topping = topping;
            return this;
        }

        public Pizza Build() => _pizza;
    }

    // Usage:
    var myPizza = new PizzaBuilder()
        .WithDough("thin crust")
        .WithSauce("tomato basil")
        .WithTopping("pepperoni")
        .Build();

    Console.WriteLine(myPizza);



```

- **Fluent Builder Inheritance with Recursive Generics**
  -  Give you type safety and a fluent API, ensuring that when you chain calls on a derived builder, you get back the right type.
  - Adhere to the open closed principle—by extending behavior without modifying the original builder.
  - As you add more layers of inheritance, your code can indeed become more convoluted and harder to read.
  - If your builder hierarchy gets too deep, you might be over-engineering a solution for a problem that could be solved by simply evolving your base builder.
```

    public class Pizza
    {
        public string Dough { get; set; }
        public string Sauce { get; set; }
        public string Topping { get; set; }
        public bool ExtraSpice { get; set; }
        public string Cheese { get; set; }  // New deluxe property

        public override string ToString() =>
            $"Pizza with {Dough} dough, {Sauce} sauce, {Topping} topping, " +
            $"{(ExtraSpice ? "extra spice" : "no extra spice")}, " +
            $"{(string.IsNullOrWhiteSpace(Cheese) ? "no cheese" : Cheese + " cheese")}.";
    }

    // Base builder using recursive generics for the fluent API.
    public abstract class PizzaBuilder<TSelf>
        where TSelf : PizzaBuilder<TSelf>
    {
        protected Pizza pizza = new Pizza();

        public TSelf WithDough(string dough)
        {
            pizza.Dough = dough;
            return (TSelf)this;
        }

        public TSelf WithSauce(string sauce)
        {
            pizza.Sauce = sauce;
            return (TSelf)this;
        }

        public TSelf WithTopping(string topping)
        {
            pizza.Topping = topping;
            return (TSelf)this;
        }

        public Pizza Build() => pizza;
    }

    // First level of inheritance: adds extra spice option.
    public abstract class SpicyPizzaBuilder<TSelf> : PizzaBuilder<TSelf>
        where TSelf : SpicyPizzaBuilder<TSelf>
    {
        public TSelf WithExtraSpice(bool extraSpice)
        {
            pizza.ExtraSpice = extraSpice;
            return (TSelf)this;
        }
    }

    // Second level of inheritance: adds a deluxe cheese option.
    public class DeluxeSpicyPizzaBuilder : SpicyPizzaBuilder<DeluxeSpicyPizzaBuilder>
    {
        public DeluxeSpicyPizzaBuilder WithCheese(string cheese)
        {
            pizza.Cheese = cheese;
            return this;
        }
    }

    // Usage example:
    var deluxePizza = new DeluxeSpicyPizzaBuilder()
        .WithDough("deep dish")
        .WithSauce("spicy tomato")
        .WithTopping("pepperoni")
        .WithExtraSpice(true)
        .WithCheese("mozzarella")
        .Build();

    Console.WriteLine(deluxePizza);


```
