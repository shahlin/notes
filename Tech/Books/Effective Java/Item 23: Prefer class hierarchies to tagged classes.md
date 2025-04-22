# Classes and Interfaces
## Item 23: Prefer class hierarchies to tagged classes

- Basically, you might have classes that contains some kind of flag (or tag) property and works differently based on that flag
- Generally, this is a bad practice because you're putting different concerns in a single class. They become verbose, error-prone and inefficient
- For example, if you have a Shape class with type property that can be rectangle or circle. There will be alot of code to make it work for both types. What if we add another shape?
- A simple solution to this can be by creating an abstract class with abstract methods specific to each type.

  ```java
  abstract class Shape {
  	abstract double calculateArea();
  }
  
  class Circle extends Shape {
  	final double radius;
  	
  	Circle(double radius) { this.radius = radius; }
  
  	@Override
  	double calculateArea() {
  		return ...
  	}
  }
  ```

- Every shape can now inherit this abstract class
