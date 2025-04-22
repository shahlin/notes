# Classes and Interfaces
## Item 22: Use interfaces only to define types

- When a class implements an interface, the interface serves as a type that can be used to refer to instances of the class
- One exception to the above is _constant interfaces_. This is when you create an interface just for constants.
- The above is considered a poor practice because imagine a class implements this constant interface. As long as no part of the code refers to this class using the interface, it's okay. But if the class is referred to by the interface, it causes issues. Imagine a scenario where your class has implemented this interface and further down the line your class does not use any of the constants defined in the interface. You can't simply remove the `implements` interface because there might be consumers of this class referring to it via the interface
- It also exposes the constants as part of the public API (when a class implementing it is created) which might not be desirable
- You can just use normal classes with static final properties to achieve this
