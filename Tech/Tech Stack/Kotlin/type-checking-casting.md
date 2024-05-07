# Type checking and casting
1. Use the `is` keyword to check if an object is an instance of a given class
2. The converse is `!is`
3. To cast a variable to a particular class, use the `as` keyword.
    3.1 Example: `val employee = anotherEmployeeObject as Employee`
4. There's a concept of smart casting. If you check first that an object `is` of a given type, then you don't need to cast it to that type within the block. Kotlin casts it automatically for you
