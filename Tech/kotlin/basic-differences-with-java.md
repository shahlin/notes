# Basic differences in Kotlin compared to Java
1. No semicolons
2. Kotlin has alot of wrappers (like println, arrayOf, etc)
3. Kotlin has soft and hard keywords which can and cannot be used as identifiers, respectively. The list is [here](https://kotlinlang.org/docs/keyword-reference.html).
4. Being able to use square brackets to access collection keys
5. The String class is Kotlin's own String class, not the Java one
    5.1 Length is a property
6. Kotlin doesn't distinguish between checked and unchecked exceptions.
    6.1. So, no need to have `throws` in function signatures to specify thrown exceptions
7. No ternary operators. Instead `if/else` is an expression
8. Kotlin doesn't have a `static` keyword (the concept exists but no keyword)
9. No `new` keyword for instance creation
10. Bit operators are not `|`, `&` or `^` in Kotlin like Java, instead they're `or`, `and` and `xor` respectively
