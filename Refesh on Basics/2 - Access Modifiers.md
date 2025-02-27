### **Course/Resource**  
[C# Access Modifiers (beyond public and private)](https://www.youtube.com/watch?v=jcn5uCZAk2w)

- **Access Modifiers**
  - Give the least amount of visibility/ permissions to objects as possible.
  - Access modifiers used well prevent bugs. 
  - They forbid developers from calling methods that only do part of a job. They force the dev to pick the only option, the full job.
    i.e Dev called CreateEmployeeActiveDirectory, but forgets or doesn't know to call GrantEmployeePermissions, these should be wrapped in one public method and both made a more private access modifier.
```
public class AccessExample
{
    private void PrivateExample()
    {
        // Only accessible within the area it was declared. 
        // The other things in the same class. Everything within the AccessExample braces{}.
    }

    private protected void PrivateProtectedExample()
    {
        // Available in this class AND any class that derives from this class ONLY if the class is within the same Project or assembly. 
    }

    protected void ProtectedExample()
    {
        // Used a lot in inheritance
        // Accessible in its own class OR in any class thats derived from this class regardless of the project/ assembly of the deriving class. 
    }

    protected internal void ProtectedInternalExample()
    {
        // Accessible from a different class in the same project/ assembly 
        // Accessible from a derived class in ANY project / assembly 
        
        // Protected in its own project / assembly  
        // Internal in any class that inherits from it 
    } 

    internal void InternalExample()
    {
        // A little broader in scope to Private but still similar. 
        // Only accessible within the project/ assembly it was called in.

        // For stuff you want to have accessible for everything in its own specific dll.
        // Helper methods, available in more than one class but not exposed outside the project/ dll. 

        // Not used a lot but should be used more. 
    }

    public void PublicExample()
    {
        // Public :D 
        // Accessible everywhere 
    }
}

```
