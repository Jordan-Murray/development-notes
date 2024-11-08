### **Course/Resource**  
[Advanced C# Collections](https://app.pluralsight.com/library/courses/csharp-collections-advanced/table-of-contents)

### **Key Concepts**
- Write robust code, optimize for performance **only if** you have issues, but for collections the situation is a bit more nuanced.
-  

### **Detailed Notes**
- **O(n) Operations and Scalability**:
  - Removing the an item in an array `someArray.RemoveAt(x)` will leave an empty space in memory in the array, which can't happen so the array 
    will need to move all its elements *up* one space. 
    - Moving a small list (8 items) would be almost instantaneous. 
    - Moving a huge list (8 million items) would take a million times longer
    - This will be an O(n) operation! Where n is (Count - index). The time this operation takes is directly linked to the size of the collection.
- **O(1)**
  - It doesn't matter how big the collection is, it will still take the same amount of time to look up an element for example.
- **O(n^2)**
  - 

### Best Practices

- W