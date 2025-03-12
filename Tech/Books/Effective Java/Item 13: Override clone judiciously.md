# Methods Common to All Objects
## Item 13: Override clone judiciously

- There is a `Cloneable` interface in Java which has a sole purpose of informing that that class allows cloning. It doesn't even have a `clone()` method in the interface
- The `Object`'s  `clone()` method is protected so it can't be publicly called
- All classes that implement `Cloneable` should override `clone()` with a public method whose return type is the class itself. This method should first call `super.clone()`
- Immutable classes should never provide a clone method. It's wasteful copying
- If there are mutable objects within an object, you will need to deep copy it by creating new instance and then filling it because just copying it directly will copy the references. So if you update `x.elements`, it's clone `y.elements` will also be updated. Example:
    
  ```java
  class Stack { private Object[] elements; }
  ```

### Better approach
- Use copy constructor or copy factory
- Also known as conversion constructor or conversion factory. They take a particular object and convert it to a new one. Example to clone:

  ```java
  class Stack {
  
      // Copy Constructor
      public Stack(Stack stack) { ... }
      
      // Copy Factory
      public static Stack newInstance(Stack stack) { ... }
  }
  ```
- Avoid `Cloneable` and `clone` when possible
