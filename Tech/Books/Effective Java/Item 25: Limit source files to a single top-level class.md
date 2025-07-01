# Classes and Interfaces
## Item 25: Limit source files to a single top-level class

- While you can have multiple top-level classes in a single file, there are no benefits and many risks
- If you have two classes repeated in 2 files, there's no guarantee which one the compiler will pick
- Example
  - Main.java
  ```java
  public class Main {
    public static void main(String[] args) {
      System.out.println(Utensil.NAME + Dessert.NAME);
    }
  }
  ```
  - Utensil.java:
  ```java
  class Utensil { static final String NAME = "pan";
  class Dessert { static final String NAME = "cake";
  ```
  - Dessert.java
  ```java
  class Utensil { static final String NAME = "pot";
  class Dessert { static final String NAME = "pie";
  ```
- From the above example, depending on the order in which you compile the classes, your output will differ
- If you run `javac Main.java Dessert.java`, your compilation will fail telling you that multiple classes defined
  - This is because when its compiling `Main.java` and finds reference to the `Utensil` class, it will look in `Utensil.java` and find both classes. Then when it comes across the `Dessert` reference in `Main.java`, it will look in `Dessert.java`, it will find the same defined classes again
- If you run `javac Main.java` or `javac Main.java Utensil.java`, it will print `pancake`
  - Because it comes across `Utensil` reference first and compiles the file (the 2 classes). So then when it comes across the `Dessert` reference, it uses the compiled version of `Dessert`
- If you run `javac Dessert.java Main.java`, it will print `potpie`
-  The solution is to just split the classes into separate files
-  If you really need them together, consider making them a static member class
