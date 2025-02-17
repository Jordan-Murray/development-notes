### **Course/Resource**  
[Delegates in C# - A practical demonstration](https://www.youtube.com/watch?v=R8Blt5c-Vi4)

- **Action**
  - Action returns a void
  - Wrapped deligate
  - Func returns a value, Action returns void.
  - Cannot support OUT params/ variables
  ```

    public class ShoppingBasketModel{

        public delegate void MentionDiscount(decimal subTotal); 

        public List<Product> Items{get;set;} = new List<Product>();

        public decimal GenerateTotal(MentionDiscount mentionDiscount, 
            Func<List<ProductModel>, decimal, decimal> calculateDiscountedTotal,
            Action<string> tellUserDiscounting)
        {
            decimal subTotal - Items.Sum(x => x.Price);

            mentionDiscount(subTotal);

            tellUserDiscounting("We are applying your discount.");

            return calculateDiscountedTotal(Items, subTotal);
        }
    }

    class Program{

            static ShoppingBasketModel basket = new ShoppingBasketModel();

            static void Main(string[] args)
            {
                PopulateBasketWithDemoDate();
                Console.WriteLine($"Total: £{basket.GenerateTotal(SubTotalAlert, CalculateLeveledDiscount, AlertUser)}")
            }

            private static void SubTotalAlert(decimal subTotal)
            {
                Console.WriteLine($"Subtotal is £{subTotal}");
            }
            
            private static decimal CalculateLeveledDiscount(List<ProductModel> items, decimal subTotal) 
            {
                if(subTotal > 100)
                {
                    return subTotal * 0.80M;
                }
                else if ...
                else{
                    return subTotal;
                }
            }

            private static void AlertUser(string message)
            {
                Console.WriteLine(message);
            }
        }
  ```

    - Creating an anonymous Action; 
    ```

        decimal total = basket.GenerateTotal((subTotal) => Console.WriteLine($"SubTotal is {subTotal}"),
            (products, subTotal) => 
            {
                if(products.Count > 3)
                {
                    return subTotal * 0.5M;
                }
                else
                {
                    return subTotal;
                }
            },
            (message) => Console.WriteLine($"Alert basket: {message}"));

    ```