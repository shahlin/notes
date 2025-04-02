# Classes and Interfaces
## Item 20: Prefer interfaces to abstract classes

- Java allows two ways to define types that permit multiple implementations: Abstract classes and Interfaces
- To implement the type defined by an abstract class, you need a subclass of the abstract class. A constraint in Java is single inheritance. This would mean that for every type, you'd need a subclass.
- Existing classes can be easily made to implement an interface whereas it's fairly difficult to make a class extend abstract classes

### Benefits of Interfaces
- Interfaces allow defining mixins. This means that a class that implements some particular functionality can also implement some other type to provide related functionalities
- Interfaces allow for construction of nonhierarchical types. Basically it allows us to break hierarchies (inheritance). For example, you might have a class implementing `Singer` interface for a singer. And another class implementing `Songwriter` interface for a songwriter. But maybe a person is both a singer and songwriter, your class can implement both. Or even better, you can create a new interface `SingerSongwriter` which extends from both `Singer` and `Songwriter` interfaces. Imagine doing this as abstract classes, there'd be alot of classes created to achieve the same
- Interfaces enable safe, powerful functionality enhancements. [Not fully clear]

### Notes
- You are not permitted to provide the `equals` or `hashCode` methods in interfaces
- There are ways of combining both abstract classes and interface methods. These are called Skeletal Implementations. The interface defines the type, the abstract class provides basic implementations for the interface methods and then the classes can extend these and modify some parts or use as is if needed. But make sure to document the implementation spec since this is designed for inheritance. Also, if a class cannot extend the abstract class, it can directly implement the interface methods.
