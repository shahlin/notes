# Methods Common to All Objects
## Item 11: Always override hashCode when you override equals

You must override hashCode in every class that overrides equals. If you don't, it will not function properly in collections such as `HashMap` and `HashSet`. General contract:
1. Repeated calls to `hashCode` must return the same value (provided that no field in the `equals` method is modified)
2. If 2 objects are equal as per the `equals` method, then their `hashCode` must be same as well
3. If 2 objects are not equal as per the `equals` method, they can have same `hashCode` but its better to have distinct values for improving performance

### Example of a typical hashCode method
```java
@Override
public int hashCode() {
    int result = Short.hashCode(areaCode);
    result = 31 * result + Short.hashCode(prefix);
    result = 31 * result + Short.hashCode(lineNum);
    return result;
}
```

### Alternatives
- The above is an example of a good `hashCode` function. But if you need a really strong hash function that is less likely to produce collisions, see Guava's `com.google.common.hash.Hashing`
- If performance is not a concern, you can use `Objects.hash(...)` method to generate the same hash codes as the above example. The reason that it is not performance friendly is because it takes varargs for its parameter which requires array creation and it also does boxing and unboxing if any of the arguments are of primitive type. Example:

    ```java
    @Override
    public int hashCode() {
        return Objects.hash(lineNum, prefix, areaCode);
    }
    ```
- Hash codes can be cached if computing the hash code is heavy and the class is immutable instead of recalculating it each time. **Also do not be tempted to exclude significant class fields from the hash code function to improve performance, it might result in worser performance**
    
    ```java
    private int hashCode; // Automatically initialized to 0

    @Override
    public int hashCode() {
        int result = hashCode;
        if (result == 0) {
            int result = Short.hashCode(areaCode);
            result = 31 * result + Short.hashCode(prefix);
            result = 31 * result + Short.hashCode(lineNum);
            hashCode = result;
        }
        
        return result;
    }
    ```
