# Null References
1. To make a variable nullable, add the question mark next to the data type like: `val str: String?`
2. Example: `val countryCode: String? = userProfile?.country?.countryCode`
3. To have a default value instead of null, use the elvis operator as shown below
    3.1 `val countryCode: String = userProfile?.country?.countryCode ?: "AE"`
4. To safe cast, use `as` with question mark like `val country: String? = userProfile?.country as? String`
5. If you're absolutely sure a variable does not contain null value, use the Non-null assertion operator. If the value is null, it will throw a KotlinNullPointerException.
    5.1. Example:
    ```kotlin
    val str: String? = "This is not null"
    println(str!!.toUpperCase())  // Will not compile if !! or ?. is not present
    ```
6. The double equals operator (`==`) is implicitly safe. Under the hood, it calls the `toEquals()` function safely.
