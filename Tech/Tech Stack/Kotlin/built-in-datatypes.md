# Built-in Datatypes
1. Every type is a class
2. Internally the Kotlin types get compiled to primitive types in Java
3. No automatic widening of numbers. Meaning, as an example, an Int cannot be assigned to a Long like Java. To do this, use toLong()
4. Any class (equivalent to Object in Java) is the top most class in Kotlin class hierarchy. It is a superclass to all classes
5. Nothing class (no equivalent in Java) is used when nothing is returned by a function. For example, a function that has an infinite loop for example
6. The Unit class is like void in Java. But in Java, void truly does not return anything whereas in Kotlin, Unit returns a singleton of the Unit class.
