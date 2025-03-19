# Classes and Interfaces
## Item 18: Favor composition over inheritance

- Inheritance is used for code reuse, but it is not always the best tool for the job
- If inheritance is used within a package with no one outside using it, it is still okay to use it
- Or if it's specifically designed and documented for inheritance, it is okay to use it

### Problems with inheritance
- **Violates encapsulation**: A subclass always depends on its superclass. What if the superclass changes any of it's methods or functionality? It will impact the subclass. Take this example:

    ```java
    public class InstrumentedHashSet<E> extends HashSet<E> {

        // The number of attempted element insertions
        private int addCount = 0;
    
        public InstrumentedHashSet() {}
        public InstrumentedHashSet(int initCap, float loadFactor) { super(initCap, loadFactor); }
    
        @Override public boolean add(E e) {
             addCount++;
             return super.add(e);
        }

        @Override public boolean addAll(Collection<? extends E> c) {
             addCount += c.size();
             return super.addAll(c);
        }
    
        public int getAddCount() { return addCount; }
    }
    ```

    If we run the following:
    
    ```java
    InstrumentedHashSet<String> s = new InstrumentedHashSet<>();
    s.addAll(List.of("a", "b", "c");
    ```

    You would expect `getAddCount` to return 3. But it returns 6. Why? Because internally, when `addAll` is called `HashSet` calls the `add` method which double counts in this example. If in a later release, `HashSet` changes its internal implementation, it will break the `InstrumentedHashSet` functionality again.

- **A superclass can acquire new methods** - In future releases, the superclass might add new methods. Using the above example, let's say that the superclass introduces a new method for adding an element. If clients start using that method to add an element, our `getAddCount` will no longer return the correct value because we've not overriden the newly introduced method.

- **It's not safe even if you're not overriding any existing method** - Let's say you create methods in your subclass that doesn't override the superclass. That works fine until the superclass has an update which brings in a new method with the same name but different return type, the code will no longer compile. If it has the same name and same return type, you're prone to the issues mentioned above.


### Use Composition
- In simple words, it's just a private field that contains an instance of an existing class.
- Example:

    ```java
    class Car {
        private final Engine engine;
        
        public Car(Engine engine) {
            this.engine = engine;
        }
    }
    ```

    You have the flexibility to swap engines for different cars.

- Composition uses a 'has-a' relationship. From above example, a `Car` has-a `Engine` whereas inheritance uses 'is-a' relationshiop which in real-world software is almost rare
