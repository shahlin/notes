# Creating & Destroying Objects
## Item 9: Prefer try-with-resources to try-finally
Example of `try-finally`:
```java
static void copy(String src, String dst) throws IOException {
    InputStream in = new FileInputStream(src);
    try {
        OutputStream out = new FileOutputStream(dst);
        try {
            byte[] buf = new byte[BUFFER_SIZE];
            int n;

            while ((n = in.read(buf)) >= 0)
                out.write(buf, 0, n);
        } finally {
            out.close();
        }
    } finally {
        in.close();
    }
}
```
1. Firstly, there's too much code to just close the resources
2. The code in both the try block and the finally block is capable of throwing exceptions
3. For example, the call to `readLine` could throw an exception due to a failure in the underlying physical device, and the call to `close` could then fail for the same reason
4. Due to point (3), the exception for the `close` call completely takes over the `readLine` exception. This makes it harder to debug

### `try-with-resources`
1. In Java 7, try-with-resources statement was introduced
2. To make a resource usable by try-with-resources construct, the resource needs to implement the `AutoCloseable` interface which consists of `close` method
3. The same above code written in try-with-resources:
```java
static void copy(String src, String dst) throws IOException {
    try (
        InputStream in = new FileInputStream(src);
        OutputStream out = new FileOutputStream(dst)
    ) {
        byte[] buf = new byte[BUFFER_SIZE];
        int n;
    
        while ((n = in.read(buf)) >= 0)
            out.write(buf, 0, n);
    }
}
```
1. The code is much cleaner
2. If exceptions are thrown by both the `readLine` call and the (invisible) `close`, the latter exception is suppressed in favor of the former
3. You can put catch clauses on try-with-resources statements
4. In summary, always use try-with-resources in preference to try-finally when working with resources that must be closed
5. The resulting code is shorter and clearer, and the exceptions that it generates are more useful
