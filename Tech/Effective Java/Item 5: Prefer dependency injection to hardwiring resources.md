# Creating & Destroying Objects
## Item 5: Prefer dependency injection to hardwiring resources
- Do not use a singleton or static utility class to implement a class that depends on one or more underlying resources whose behavior affects that of the class
- Do not create these resources directly in the class
- Pass the resources as dependencies to the class
- This improves flexibility, reusability, and testability of a class

```java
public class SpellChecker {
    private final Lexicon dictionary;

    public SpellChecker(Lexicon dictionary) {
        this.dictionary = Objects.requireNonNull(dictionary);
    }

    public boolean isValid(String word) { ... }

    public List<String> suggestions(String typo) { ... }
}
```
- Any client can pass a dictionary of their choice
- It preserves immutability, so multiple clients can share the dependant objects
