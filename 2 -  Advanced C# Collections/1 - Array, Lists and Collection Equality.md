### **Course/Resource**  
[Advanced C# Collections](https://app.pluralsight.com/library/courses/csharp-collections-advanced/table-of-contents)

### **Key Concepts**
- When doing == we're testing is the collection **the same instance**. This applies to almost all collections in C#!
- Arrays store elements in a sequential block of memory, making index-based access extremely fast. However, they have a fixed size because the continuous memory block can't be expanded if adjacent memory is occupied.
- `List<T>` uses an internal array to store its elements. When adding more items than its current capacity, it creates a new, larger array and copies existing elements over. This resizing operation can be costly if it happens frequently.

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
  - Arrays size cannot be resized because the block of the memory used to store the array is sequential and there may not be space/ occupied by other memory. 
    - Why do they do this? Because its simple and quick to lookup!
  - Most collections use arrays under the hood!
- **Lists under the hood**
  - `List<T>` contains a reference to an internal array that holds the actual data.
  - When adding an element that would be larger than the allocated memory, the internal array gets copied and put into somewhere in memory that is big enough and abandons the old array.
    - Doesn't always happen because `List<T>` usually creates the initial array with more space than it needs to allow for growth.
  - Repeatedly adding items is slow.

### Best Practices

- When you need to compare the contents of arrays or collections, use methods like `SequenceEqual` instead of the `==` operator, which only checks for reference equality.
- If you know the approximate number of elements beforehand, initialize your `List<T>` with that capacity to minimize performance overhead from frequent resizing.