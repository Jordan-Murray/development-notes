### **Course/Resource**  
[Delegates in C# - A practical demonstration](https://www.youtube.com/watch?v=R8Blt5c-Vi4)

- **Func**
  - `decimal subTotal - Items.Sum(x => x.Price);` .Sum actually takes in a Func<ProductModel, decimal>. e.g `.Sum<ProductModel>(Func<ProductModel,decimal> selector);`
  - `x => x.Price` is a delegate called Func.
  - Func is a delegate type.
  - The last parameter of the Func is always the output type
  - Func is a method that has a return value other than void.
  - Difference between the implementation of the Delegate and Func is that we had to declare the Delegate in the class, whereas the Func signiture is defined in the target method.
    ```
        //Class Library Project
        public class ShoppingBasketModel{

            public delegate void MentionDiscount(decimal subTotal); 

            public List<Product> Items{get;set;} = new List<Product>();

            // This method should probably do more
            // Check if item is stock etc 
            public deciaml GenerateTotal(MentionDiscount mentionDiscount, 
                Func<List<ProductModel>, decimal, decimal> caclulateDiscountedTotal) //T1 input, T2 input, T3 output
            {
                decimal subTotal - Items.Sum(x => x.Price);

                mentionDiscount(subTotal);

                return caclulateDiscountedTotal(Items, subTotal);
            }
        }

        //UI Project
        class Program{

            static ShoppingBasketModel basket = new ShoppingBasketModel();

            static void Main(string[] args)
            {
                PopulateBasketWithDemoDate();
                Console.WriteLine($"Total: £{basket.GenerateTotal(SubTotalAlert, CalculateLeveledDiscount)}")
            }

            private static void SubTotalAlert(decimal subTotal)
            {
                Console.WriteLine($"Subtotal is £{subTotal}");
            }
            
            privatye static decimal CalculateLeveledDiscount(List<ProductModel> items, deciaml subTotal) // Matches our func param signiture
            {
                if(subTotal > 100)
                {
                    returen subTotal * 0.80M;
                }
                else if ...
                else{
                    return subTotal;
                }
            }
        }

    ``` 
    - Creating an anonymous Func; 
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
            });

    ```