### **Course/Resource**  
[Advanced C# Collections](https://app.pluralsight.com/library/courses/csharp-collections-advanced/table-of-contents)

### **Key Concepts**
- When doing == we're testing is the collection **the same instance**. This applies to almost all collections in C#!
- 

### **Detailed Notes**
- **Array Equality & Value/ Reference Types**:
  - Two arrays with exactly the same values in are **not** (==) equivalent!
  - Most collections are reference types.
  - `DateTime` - Value Type, `Array` - Reference Type (Value of the type doesn't actually get stored in memory, the variable declared will hold a reference type to the actual location of the value assigned). **So array1 (addressX in memory) != array2 (addressY in memory) despite containing the same values.**
  - When doing == we're testing is the collection **the same instance**. This applies to almost all collections in C#!
  - When you actually **want** to test the equality of arrays and their contents, we can use `array1.SequenceEqual(array2)`. This is quite an expensive operation thought!
  - Assigning `array3` to be equal to `array1` and testing equality `==` will produce true. (because they're both pointing to the same place in memory)
  - These are normal results for all reference types in C#!
- **Structure and Purpose of Arrays**
  - Size is fixed.
  - Can only look up by index.
  - Why did Microsoft seemingly make the most fundamental data structure so useless?
  - Arrays under the hood store each value in the array sequentially in a single bloc of memory. (Extremely easy and efficient to retrieve an item by its index)

### Best Practices

- 