# Arrays
1. Two ways to create arrays:
    1.1 `arrayOf<Long>(1, 2, 3)`
    1.2 `Array(16) { i -> i * 2}`
2. Mixed arrays in Kotlin are just arrays of type Any (`Array<Any>()`)
3. Primitive arrays can be used to make the arrays interoperable with Java. Example, `intArrayOf()`, `charArrayOf()`, etc
4. Using primitive arrays in kotlin can give a performance boost
5. Primitive array can be instantiated with a length and no values but normal kotlin arrays cant be.
    5.1 This is possible: `val arr = IntArray(5)` (it creates array with 5 zeroes)
    5.2 This is not possible: `val arr = Array<Int>(5)` because it requires 2 arguments in the initialization; length and values
6. Kotlin arrays can be converted to primitive arrays using the type's corresponding converted function. Example: `toIntArray()`
7. Primitive arrays can be converted back to Kotlin typed arrays using the `toTypedArray()` function
