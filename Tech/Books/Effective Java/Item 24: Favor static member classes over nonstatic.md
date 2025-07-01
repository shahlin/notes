# Classes and Interfaces
## Item 24: Favor static member classes over nonstatic

- A nested class (class within a class) must exist only to serve it's enclosing class
- If a nested class is useful in another context, it should be a top level class
- 4 kinds of nested classes:
  - Static member classes
  - Non static member classes
  - Anonymous classes
  - Local class

### Static Member Class
- A nested class has access to all of the enclosing class's members, even the private ones
- One common use of a static member class is a public helper class. For example, `Operation` enum should be a public static member of a `Calculator` class. So clients can refer to operations like this: `Calculator.Operation.PLUS`
- One common use of a private static member class is to represent properties of the enclosing class. Like, a Map implementation can have `Entry` as a private static member class
- 
### Non Static Member Class
- Alternatively, a non static member class can only be instantiated from the enclosing class that is instantiated. Within the nested class, it can refer to the enclosing class members using `this`
- One common use of a non static member class is to define an _Adapter_
- If you declare a member class that does not require access to an enclosing instance, **always** make it a static member class. If you don't, each instance will have an unwanted reference to it's enclosing instance - Waste of time and space

### Anonymous Class
- Anonymous classes are classes with no name. It is not a member of an enclosing class. It is declared and instantiated at the point of use
- Anonymous classes can be used wherever expressions are legal
- Anonymous classes can't be instantiated except at the point they're declared
- Before Lambdas, anonymous classes were commonly used to create small function objects and process objects on the fly

### Local Class
- Local classes are the least used of the 4 kinds of nested classes
