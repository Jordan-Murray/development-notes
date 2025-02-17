### **Course/Resource**  
[Delegates in C# - A practical demonstration](https://www.youtube.com/watch?v=R8Blt5c-Vi4)

- **Delegates**
  - A delegate at its core; You pass in a method instead of a variable. 
  - Shouldn't do all the work in the delegate, should only be doing a `tweak`.
    ```
        //Class Library Project
        public class ShoppingBasketModel{

            public delegate void MentionDiscount(decimal subTotal); 
            // Think of this as an interface as a frame of reference.
            // A contract to say whenevr you use this, its going to returen void and its going to take in a decimal. 
            // Class library doesn't care about the implementation of the UI library, it just knows that it can accept a delegate method

            public List<Product> Items{get;set;} = new List<Product>();

            public deciaml GenerateTotal(MentionDiscount mentionDiscount)
            {
                decimal subTotal - Items.Sum(x => x.Price);

                mentionDiscount(subTotal);
                //This class has no idea what mentionDiscount does, just the contract.

                if(subTotal > 100)
                {
                    returen subTotal * 0.80M;
                }
                else{
                    return subTotal;
                }
            }
        }

        //UI Project
        class Program{

            static ShoppingBasketModel basket = new ShoppingBasketModel();

            static void Main(string[] args)
            {
                PopulateBasketWithDemoDate();
                Console.WriteLine($"Total: £{basket.GenerateTotal(SubTotalAlert)}")
                //We don't pass in the parameter as we don't know what it is yet!
                // That happens in the delegate class (ShoppingBasketModel)
            }

            private static void SubTotalAlert(decimal subTotal)
            {
                Console.WriteLine($"Subtotal is £{subTotal}");
            }
        }

    ``` 
  - Creating an anonymous delegate; 
    ```

        decimal total = basket.GenerateTotal((subTotal) => Console.WriteLine($"SubTotal is {subTotal}"));

    ```