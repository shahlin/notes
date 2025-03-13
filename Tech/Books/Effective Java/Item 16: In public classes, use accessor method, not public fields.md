# Classes and Interfaces
## Item 16: In public classes, use accessor method, not public fields

- If data fields of public classes are exposed, they do not offer the benefits or encapsulation/information hiding
- You can't change the representation of the field without changing the API nor can you perform some action when the field is accessed
- Instead, you can make the fields private and create getters and if required, setters.
- If a class is a package-private or private nested class, there's nothing wrong with exposing the data fields and avoid getters/setters.
- Another option, which is less harmful than public fields is when the fields are immutable. Example:
    ```java
    public class Time {
        public final int hour;
        public final int minute;

        public Time(int hour, int minute) {
            this.hour = hour;
            this.minute = minute;
        }
    }
    ```
- But we now have records so you can just do `public record Time(int hour, int minute) {}`
- When you do need a class, you can go with the class with final fields approach
