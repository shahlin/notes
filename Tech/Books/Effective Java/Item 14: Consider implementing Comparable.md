# Methods Common to All Objects
## Item 14: Consider implementing Comparable

- The `compareTo` method is not defined by `Object`. It is present in the `Comparable` interface
- By implementing `Comparable`, a class indicates that its instances have a natural ordering
- The `Comparable` interface looks like this:

  ```java
  public interface Comparable<T> {
    int compareTo(T t);
  }
  ```
- The return of `a.compareTo(b)` should be:
  - Negative - If a < b
  - Zero - If a == b
  - Positive - If a > b
- It is strongly recommended that this is true: `(a.compareTo(b) == 0) == (a.equals(b))`. If not, the class must explicitly mention this
  - For example, the `BigDecimal` class's `compareTo` is inconsistent with the `equals` method.
  - If you create a `HashSet` and add `new BigDecimal("1.0")` and `new BigDecimal("1.00")`, the set will contain 2 elements because the 2 instances are unequal when compared using `equals`
  - If you create a `TreeSet` and add the same 2 instances, the set will only contain 1 element because it uses `compareTo`
- Within the `compareTo` method, avoid using '<' or '>'. Instead use the compare methods provided by boxed primitives. For example, `Integer.compare(a, b)`
- Always compare the most significant fields first and then move to least significant fields. Example:

  ```java
  public int compareTo(PhoneNumber pn) {
    int result = Short.compare(areaCode, pn.areaCode);

    if (result == 0) {
      result = Short.compare(prefix, pn.prefix);

      if (result == 0) {
        result = Short.compare(lineNum, pn.lineNum);
      }
    }

    return result;
  }
  ```

### Comparator Interface
- Instead of writing nested if statements as shown above and having error prone code, there's a better fluent styled option. That's using the Comparator
- The above can be rewritten this way:

  ```java
  private static final Comparator<PhoneNumber> COMPARATOR =
    comparingInt((PhoneNumber pn) -> pn.areaCode)
      .thenComparingInt(pn -> pn.prefix)
      .thenComparingInt(pn -> pn.lineNum);

  public int compareTo(PhoneNumber pn) {
    return COMPARATOR.compare(this, pn);
  }
  ```
