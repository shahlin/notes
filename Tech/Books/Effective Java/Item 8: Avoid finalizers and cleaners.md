# Creating & Destroying Objects
## Item 8: Avoid finalizers and cleaners
1. In C++, there is destructors which is used to perform actions right before a class is destroyed
2. Java has no destructors
3. Instead, Java has `finalizers` which allow you to run a piece of code whenever the object is being garbage collected
4. When the finalizer will be invoked is unknown to the developer. Finalizers are unpredictable, dangerous and generally unnecessary
5. As of Java 9, finalizers have been deprecated
6. The replacement for finalizers are cleaners. Cleaners are less dangerous but still unpredictable, slow and generally unnecessary
7. One shortcoming of finalizers and cleaners is that there is no guarantee they’ll be executed promptly (or if they'll even run at all)
8. This means that you should never do anything time-critical in a finalizer or cleaner
9. You should never depend on a finalizer or cleaner to update persistent state
10. There is a severe performance penalty for using finalizers and cleaners

### Solution
1. Have your class implement `AutoCloseable`
2. Require its clients to invoke the close method on each instance when it is no longer needed
3. Typically using `try-with-resources` to ensure termination even in the face of exceptions
4. In summary, don’t use cleaners  or finalizers except as a safety net or to terminate noncritical native resources. Even then, beware the indeterminacy and performance consequences.
