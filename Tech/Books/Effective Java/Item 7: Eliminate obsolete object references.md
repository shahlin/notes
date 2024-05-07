# Creating & Destroying Objects
## Item 7: Eliminate obsolete object references
1. Dereferencing means the action of accessing an object's features through a reference
2. An obsolete reference is simply a reference that will never be dereferenced again
3. Memory leaks affect the application performance
4. Obsolete object references can take up additional memory as they are not garbage collected, in turn affecting performance

```java
public class Stack {
    private Object[] elements;
    private int size = 0;
    private static final int DEFAULT_INITIAL_CAPACITY = 16;

    public Stack() {
        elements = new Object[DEFAULT_INITIAL_CAPACITY];
    }

    public void push(Object e) {
        ensureCapacity();
        elements[size++] = e;
    }

    public Object pop() {
        if (size == 0)
            throw new EmptyStackException();
        return elements[--size];
    }

    private void ensureCapacity() {
        if (elements.length == size)
            elements = Arrays.copyOf(elements, 2 * size + 1);
    }
}
```

1. The above code has a memory leak in the `pop` method
2. It just returns nth element without deleting its reference
3. Next time `pop` is called, it returns n-1th element and the nth element becomes obsolete

Fix:
```java
public Object pop() {
    if (size == 0)
        throw new EmptyStackException();

    Object result = elements[--size];
    elements[size] = null; // Eliminate obsolete reference
    return result;
}
```
1. Nulling out object references should be the exception rather than the norm
2. The `Stack` class above manages its own memory
3. Whenever a class manages its own memory, the programmer should be alert for memory leaks
4. Whenever an element is freed, any object references contained in the element should be nulled out

### Other Memory Leak Causes
1. Caches - When a value is put in cache and is not used for long (unwanted)
2. Listeners and Other Callbacks - When you register callbacks but don't deregister them
