# Creating & Destroying Objects
## Item 6: Avoid creating unnecessary objects
1. Unnecessary object creations can be avoided using Static Factory Methods instead of constructors
2. Constructors create an instance of the class every time they're called. Static Factory Methods do not have to create an instance every time
3. Prefer primitives to boxed primitives, and watch out for unintentional autoboxing

### Simple Example
```java
String s = new String("bikini"); // DON'T DO THIS!
```
The above creates a new instance of `String` class every time.
```java
String s = "bikini";
```
The above, however, reuses the already created `String` instance.

### `String.matches` for Regular Expressions
1. While String.matches is the easiest way to check if a string matches a regular expression, it’s not suitable for repeated use in performance-critical situations
2. The problem is that it internally creates a Pattern instance for the regular expression and uses it only once
3. Creating a Pattern instance is expensive because it requires compiling the regular expression into a finite state machine
#### Don't
```java
static boolean isRomanNumeral(String s) {
    return s.matches("^(?=.)M*(C[MD]|D?C{0,3})"
    + "(X[CL]|L?X{0,3})(I[XV]|V?I{0,3})$");
}
```

### Do
```java
public class RomanNumerals {
    private static final Pattern ROMAN = Pattern.compile(
        "^(?=.)M*(C[MD]|D?C{0,3})"
        + "(X[CL]|L?X{0,3})(I[XV]|V?I{0,3})$");

    static boolean isRomanNumeral(String s) {
        return ROMAN.matcher(s).matches();
    }
}
```
