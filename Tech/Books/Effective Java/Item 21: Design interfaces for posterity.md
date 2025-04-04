# Classes and Interfaces
## Item 21: Design interfaces for posterity

- Interfaces pre Java 8 didn't have default methods. So if you add a method to an interface which has existing implementations, it'd break
- But that's not the case after default methods were introduced, you can add them to interfaces with existing implementations without breaking them
- Even though you can add default methods, you need to be careful when using them
- Default methods might not work for all implementations
- Imagine a scenario where an interface is already in use by few implementations and you add a default method to the interface. For the users of the interface, the users of the interface might try to use the new default method. But it might not produce results they expect, because this particular implementation probably does it differently
