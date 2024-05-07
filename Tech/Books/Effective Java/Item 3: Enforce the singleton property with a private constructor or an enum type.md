# Creating & Destroying Objects
## Item 3: Enforce the singleton property with a private constructor or an enum type
A singleton is simply a class that is instantiated exactly once
### 2 Ways to Implement Singletons
#### Public field
```java
// Singleton with public final field
public class Elvis {
    public static final Elvis INSTANCE = new Elvis();

    private Elvis() { ... }
        public void leaveTheBuilding() { ... }
    }
}
```
The private constructor is called only once, to initialize the public static final field `Elvis.INSTANCE`. Since there is no public constructor, it is guaranteed that we will only have one instance of this class. Note that there is a workaround to invoke the private constructor using Reflection API.
#### Static Factory Method
```java
// Singleton with static factory
public class Elvis {
    private static final Elvis INSTANCE = new Elvis();

    private Elvis() { ... }

    public static Elvis getInstance() { return INSTANCE; }

    public void leaveTheBuilding() { ... }
}
```
All calls to `Elvis.getInstance` return the same object reference, and no other Elvis instance will ever be created (with the same caveat mentioned earlier). One advantage of this method is that it doesn't always have to be a singleton. The factory method can conditionally create new objects of the class.

Note: To make a singleton class `Serializable`, it is not enough to just implement the interface. You need to provide a `readResolve` method which will return the instance. Like this:
```java
private Object readResolve() {
    // Return the one true Elvis and let the garbage collector
    // take care of the Elvis impersonator.
    return INSTANCE;
}
```

### Single Element Enum
There is a third way to create singletons and is a recommended approach. Using `ENUMS`:
```java
// Enum singleton - the preferred approach
public enum Elvis {
    INSTANCE;

    public void leaveTheBuilding() { ... }
}
```
It is unnatural at first but it's concise and comes with serialization functionality by default.
