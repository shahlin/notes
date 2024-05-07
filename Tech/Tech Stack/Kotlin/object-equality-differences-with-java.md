# Object Equality Differences in Kotlin and Java
1. Java `==` operator does referential equality check (checks if same memory location)
2. Kotlin `==` operator does structural equality check (checks if the object properties are the same)
3. Java `.equals()` does structural equality check
4. Kotlin `.equals()` does structural equality check. The `==` operator just uses `.equals()` under the hood
5. To check for referential equality in Kotlin, use the `===` operator
