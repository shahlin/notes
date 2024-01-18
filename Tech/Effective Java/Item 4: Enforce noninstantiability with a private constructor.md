# Creating & Destroying Objects
## Item 4: Enforce noninstantiability with a private constructor
- Sometimes we want classes which just have a group of static methods and fields
- These classes have gained a bad reputation because it stops you from thinking in terms of objects, but they have valid use cases
- For example, they can be used as utility classes to work on related functionality. Example: Performing array operations
- It is **important** for these classes to be non-instantiable because objects of these classes don't make sense
- We can't make them abstract classes because it can still be subclassed and gives an idea that it was developed for inheritance
### Solution: Make the constructor private
```java
// Noninstantiable utility class
public class UtilityClass {
    // Suppress default constructor for noninstantiability
    private UtilityClass() {
        throw new AssertionError();
    }
}
```
- This also prevents the class from being subclassed because a subclass always calls the parent's constructor
