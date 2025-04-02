# Classes and Interfaces
## Item 19: Design and document for inheritance or else prohibit it

- Basically, the class must document the self-use (used by the subclass) of overridable methods
- Java doc contains this specific documentation using the `@implSpec` tag
- In the previous item we saw that when a subclass overrides a method, it might not be completely aware of the internal workings of some overridable methods that it uses. Due to which, there might be unexpected results.
- "The only way to test a class designed for inheritance is to write subclasses"
- Rule: Constructors must not invoke overridable methods directly or indirectly. If it does, program might fail.
- Rule: If the class implements `Cloneable` or `Serializable`, neither `clone` nor `readObject` may invoke an overridable method directly or indirectly
- If you decide to implement `Serializable` in a class designed for inheritance and the class has a `readResolve` or `writeReplace` method, they should be made protected rather than private
