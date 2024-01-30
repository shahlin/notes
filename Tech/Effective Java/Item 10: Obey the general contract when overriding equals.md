# Methods Common to All Objects
## Item 10: Obey the general contract when overriding equals

The easiest way to avoid problems is not to override the equals method, in which case each instance of the class is equal only to itself. This is the right thing to do if any of the following conditions apply:
1. Each instance of the class is inherently unique - Example: `Thread` class
2. There is no need for the class to provide a “logical equality” test - Example: `java.util.regex.Pattern` class doesn't really need to support `equals` to check if 2 `Pattern` classes are equal
3. A superclass has already overridden equals, and the superclass behavior is appropriate for this class
4. The class is private or package-private, and you are certain that its equals method will never be invoked
    ```java
    // Disallowing invoking equals
    @Override public boolean equals(Object o) {
        throw new AssertionError(); // Method is never called
    }
    ```

### When to override `equals`?
1. When a class has a notion of *logical equality* that differs from mere object identity
2. A superclass has not already overridden equals
3. This is generally the case for value classes
4. A value class is simply a class that represents a value, such as `Integer` or `String`

Equals implements an "equivalence relation":

- Reflexive: x.equals(x) == true
- Symmetric: x.equals(y) == y.equals(x)
- Transitive: x.equals(y) == y.equals(z) == z.equals(x)
- Consistent: x.equals(y) == x.equals(y) == x.equals(y) == ... (consistently returns true)
- Non-nullity: x.equals(null) returns false (when x is not null)

### Recipe
1. Use the `==` operator to check if the argument is a reference to this object
2. Use the `instanceof` operator to check if the argument has the correct type
3. Cast the argument to the correct type (Because this cast was preceded by an instanceof test, it is guaranteed to succeed)
4. For each "significant" field in the class, check if that field of the argument matches the corresponding field of this object

Typical `equals` implementation should look like:
```java
	@Override
	public boolean equals (Object o){
		if(o == this)
			return true;

		if (!(o instanceof PhoneNumber))
			return false;

		PhoneNumber pn = (PhoneNumber)o;
		return pn.lineNumber == lineNumber
			&& pn.prefix == prefix
			&& pn.areaCode == areaCode;
	}
```
