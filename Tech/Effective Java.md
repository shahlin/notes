# Creating & Destroying Objects
## Item 1: Static Factory Method
A static factory method simply returns an instance of the class.
Example Code:
```java
public static Boolean valueOf(boolean b) {
    return b ? Boolean.TRUE : Boolean.FALSE;
}
```
### Advantages
1. Has a name unlike constructors
    a. Makes it clear in cases where there are multiple constructors with different parameter types in their arguments
2. Unlike constructors, they are not required to create a new object each time they’re invoked.
    a. Classes that use a static factory method to instantiate their objects once and reuse them are said to be instance-controlled.
    b. Instance control allows a class to guarantee that it is a singleton or noninstantiable
    c. It also allows an immutable value class to make the guarantee that no two equal instances exist: `a.equals(b)` if and only if `a == b`
3. Unlike constructors, they can return an object of any subtype of their return type. For example, if the return type is of class `Animal`, the method can also return the class `Cow`
4. The class of the returned object can vary from call to call as a function of the input parameters. Any subtype of the declared return type is permissible
    a. Example: The `EnumSet` class has no public constructors, only static factories
5. The class of the returned object need not exist when the class containing the method is written
    a. Meaning, your return type can be an interface which can have different implementations in the future
    b. Such flexible static factory methods form the basis of service provider frameworks, like the Java Database Connectivity API (JDBC). More on the implementation details [here](https://stackoverflow.com/a/26584242)
    d. A service provider framework is a system in which providers implement a service, and the system makes the implementations available to clients, decoupling the clients from the implementations
    d. Java has `ServiceLoader` to ease the discovery of these implementations. More on this [here](https://dzone.com/articles/play-with-java-serviceloader-forget-about-dependen)
### Disadvantages
1. The main limitation of providing only static factory methods is that classes without public or protected constructors cannot be subclassed.
2. Static factory methods are hard for programmers to find
    a. They do not stand out like constructors do

