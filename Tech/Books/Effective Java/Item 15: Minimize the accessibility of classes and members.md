# Classes and Interfaces
## Item 15: Minimize the accessibility of classes and members

A well designed class hides all its implementation details, cleanly separating its API from its implementation. This is called __information hiding__ or __encapsulation__.

**The rule of thumb**: Make each class or member as inaccessible as possible


### Class Visibility
- Top-level classes can either be public or package-private (default if public is not specified)
- If a top-level class is public, it can be accessed publicly
- If a top-level class is package-private, it can only be accessed from within the package
- If you make classes public, you are obligated to support it forever to maintain compatibility
- If a package-private top-level class or interface is used by only one class, consider making the top-level class a private static nested class of the sole class that uses it

### Member Visibility
- You should always prefer making the class members private
- Make it package-private only if another class in the package requires it. If you do this alot, re-examine your design
- Private and package-private members are still part of the class's implementation and do not normally impact its exported API. Although the fields can sometimes leak into the exported API if it implements `Serializable`
- If you make the members protected, there's a huge increase in accessibility. A protected member is part of the class's exported API and you must support it forever. This is because there are _n_ classes that can inherit it.
- Avoid protected members when possible
- A [final] field/member containing a reference to a mutable object has all the disadvantages of a nonfinal field

### Method Visibility
- If a member overrides a superclass method, it cannot be more restrictive in the subclass than in the superclass

> Note: Do not make a class, interface or a member part of the exported API (publicly accessible) just for testing purposes (to access it in unit tests for example)
