# Methods Common to All Objects
## Item 12: Always override toString

### General rules
- The default implementation of `toString` in `Object` is not the most informative one. It prints the class name followed by an '@' symbol and then the hexadecimal representation of the hashCode. Example: PhoneNumber@8m0239. This is not so useful. The `toString` contract says it is recommended that all subclasses of `Object` overrides this method.
- While it is not as strict as obeying the `equals` and `hashCode` contracts, providing the `toString` implementation just makes the class much more pleasant to use and makes it easier to debug
- The `toString` method is automatically invoked when an object is passed to `println`, `printf`, string concatenation operator assert or is printed by a debugger
- Make sure the `toString` implementation returns all of the interesting/important information
- For value objects, provide the format in the documentation (class documentation). For example, if it's a PhoneNumber class, it will be good to know what format it supports
- Implement `toString` in abstract classes whose subclasses share a common string representation

### When not to use `toString`
- Utility classes
- Enums. Java already provides a good one
