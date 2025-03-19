# Classes and Interfaces
## Item 17: Minimize Mutability

An immutable class is a class whose instances cannot be modified. Example: String, BigDecimal, boxed primitive classes, etc. If at all a class needs to be mutable, limit its mutability. [In Java 16 and above, prefer using Records for immutable objects]

### Rules to make a class immutable
1. Don't provide mutators (methods that modify state)
2. Ensure that the class cannot be extended
3. Make all fields final
4. Make all fields private
5. Ensure exclusive access to any mutable components (never initialize such a mutable field to a clien provided object reference or return the field from an accessor, make defensive copies in constructor or accessors)
    
    ```java
    public final class Number {
        private final int num;
        
        public Number(int num) {
            this.num = num;
        }
        
        public Number plus(Number num1) {
            return new Number(num + num1);
        }
        
        public int number() {
            return num;
        }
    }
    ```
The above code uses a functional approach for the arithmetic operations because the methods return a new instance without modifying the existing one. Functional approach enables immutability.

### Benefits of an immutable object
- Immutable objects are simple. They can only exist in one state forever
- Immutable objects are inherently thread-safe, they require no synchronization. They can be shared freely
- Immutable objects provide failure atomicity for free. Since the state never changes, they will never be temporarily inconsistent
- Allow reusability whenever possible. For example:
    ```java
    public static final Number ZERO = new Number(0);
    public static final Number ONE = new Number(1);
    ```
    You never have to make defensive copies of immutable objects because the copies will always be same as the original. Therefore, you should not provide a `clone` method or a copy constructor on an immutable class

### Disadvantage of an immutable object
- They require separate object for each distinct value. This could be a performance issue incase you perform a multistep operation that generates a new object at every step (like in a loop). There are some ways to fix this
