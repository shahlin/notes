_____

# Single Responsibility Principle

Every software component should have one and only one reason to change. More reasons to change, more bugs that can be introduced.

## Cohesion
Degree to which the various parts of a software are related. Higher cohesion helps attain better adherence to the Single Responsibility Principle.

### Example
Low cohesion in the following snippet because the `Square` class is calculating values plus performing tasks related to rendering the square.
```php
class Square {
	private $side = 5;
	
    public function calculateArea() {
	    // ...
    }
	
    public function calculatePerimeter() {
	    // ...
    }
	
    public function draw() {
	    // ...
    }
    
    public function rotate() {
	    // ...
    }
}
```
**Solution**: Split the methods into separate classes. One for calculation and other for rendering.
```php
class Square {
	private $side = 5;
	
    public function calculateArea() {
	    // ...
    }
	
    public function calculatePerimeter() {
	    // ...
    }
}

class SquareUI {
	public function draw() {
	    // ...
    }
    
    public function rotate() {
	    // ...
    }
}
```


## Coupling
Level of inter dependency between various software components. Loose coupling helps attain better adherence to the Single Responsibility Principle.

### Example
High coupling in the following example because the query and the database connection is built into the model code. In case of the database vendor change, the model code will also have to be changed.
```php
class Student {

	public function save() {
		$dbUser = "";
		$dbPassword = "";
		$dbHost = "";
		$dbName = "";
		$mysqli = mysqli_connect($dbUser, $dbPassword, $dbHost, $dbName);

		$mysqli->query("INSERT INTO ...");
	}

}
```

**Solution**: Split the database operations into a separate class, in this case, a repository class.
```php
class Student {

	public function save() {
		(new StudentRepository)->save($this);
	}

}

class StudentRepository {

	public function save(Student $student) {
		$dbUser = "";
		$dbPassword = "";
		$dbHost = "";
		$dbName = "";
		$mysqli = mysqli_connect($dbUser, $dbPassword, $dbHost, $dbName);

		$mysqli->query("INSERT INTO ...");
	}

}
```
___
# Open Closed Principle

Software components should be closed for modification but open for extension. Modifying existing code requires further testing, other efforts and potentially introduce new bugs.

### Example
```java
class InsurancePremiumDiscountCalculator {
	public int calculate(HealthInsuranceCustomerProfile customer) {
		if (customer.isLoyal()) {
			return 20;
		}
	}
}

class HealthInsuranceCustomerProfile {
	public boolean isLoyal() {
		return Random.nextBoolean();
	}
}
```
If we bring in a new type of insurance like Vehicle Insurance, it'd be mean that we need to modify existing calculator class like this:
```java
class InsurancePremiumDiscountCalculator {
	public int calculate(HealthInsuranceCustomerProfile customer) {
		if (customer.isLoyal()) {
			return 20;
		}
	}
	
	public int calculate(VehicleInsuranceCustomerProfile customer) {
		if (customer.isLoyal()) {
			return 20;
		}
	}
}

class HealthInsuranceCustomerProfile {
	public boolean isLoyal() {
		return Random.nextBoolean();
	}
}

class VehicleInsuranceCustomerProfile {
	public boolean isLoyal() {
		return Random.nextBoolean();
	}
}
```
**Solution**: We can create an interface and implement that for every new type of insurance while keeping the calculator class unchanged. The calculator class will have to be updated once to use the interface in the function signature rather than the concrete class.
```java
class InsurancePremiumDiscountCalculator {
	public int calculate(InsuranceCustomerProfile customer) {
		if (customer.isLoyal()) {
			return 20;
		}
	}
}

class InsuranceCustomerProfile {
	public boolean isLoyal();
}

class HealthInsuranceCustomerProfile implements InsuranceCompanyProfile {
	public boolean isLoyal() {
		return Random.nextBoolean();
	}
}

class VehicleInsuranceCustomerProfile implements InsuranceCompanyProfile {
	public boolean isLoyal() {
		return Random.nextBoolean();
	}
}

class HomeInsuranceCustomerProfile implements InsuranceCompanyProfile {
	public boolean isLoyal() {
		return Random.nextBoolean();
	}
}
```
___
# Liskov Substitution Principle

> _Subtypes must be substitutable for their base types._

Objects should be replaceable with their subtypes without affecting the correctness of the program **OR** objects of a superclass should be replaceable with objects of its subclasses without breaking the application.

- An overridden method of a subclass needs to accept the same input parameter values as the method of the superclass. That means you can implement less restrictive validation rules, but you are not allowed to enforce stricter ones in your subclass.
- Similar rules apply to the return value of the method. The return value of a method of the subclass needs to comply with the same rules as the return value of the method of the superclass. [Source](https://stackify.com/solid-design-liskov-substitution-principle/)

## Example

**Breaking example:**
```php
public class Bird {
    public void fly() {}
}

public class Duck extends Bird {}
```
The above seems right, but what about this?
```php
public class Ostrich extends Bird {}
```
Ostrich is a bird but it cannot fly. In this case, the `fly()` function is unimplemented for `Ostrich` class or it might throw an exception. This breaks LSP.

**Solution, break the hierarchy:**
```php
public class Bird {}

public class FlyingBirds extends Bird {
    public void fly() {}
}

public class Duck extends FlyingBirds {}

public class Ostrich extends Bird {} 
```
^Could probably also be done using interfaces like `Flyable`.

---
# Interface Segregation Principle

No client should depend on methods that it does not use. Basically, do not create interfaces that classes partially implement (leaving some methods empty). This would mean that the class is not really following the complete contract.

## Example

Incorrect scenario:
```php
interface MultiFunctionPrinter {
	public function print();
	public function getPrintSpoolDetails();
	public function scan();
	public function scanPhoto();
	public function fax();
}

class XeroxWorkCenter implements MultiFunctionPrinter {
	public function print() { // implementation }
	public function getPrintSpoolDetails() { // implementation }
	public function scan() { // implementation }
	public function scanPhoto() { // implementation }
	public function fax() { // implementation }
}

class CanonPrinter  implements MultiFunctionPrinter {
	public function print() { // implementation }
	public function getPrintSpoolDetails() { // implementation }
	public function scan() { }
	public function scanPhoto() { }
	public function fax() { }
}
```

**Solution**, split into smaller interfaces:
```php
interface Printable {
	public function print();
	public function getPrintSpoolDetails();
}

interface Scanable {
	public function scan();
	public function scanPhoto();
}

interface Faxable {
	public function fax();
}

class XeroxWorkCenter implements Printable, Scanable, Faxable {
	public function print() { // implementation }
	public function getPrintSpoolDetails() { // implementation }
	public function scan() { // implementation }
	public function scanPhoto() { // implementation }
	public function fax() { // implementation }
}

class CanonPrinter  implements Printable {
	public function print() { // implementation }
	public function getPrintSpoolDetails() { // implementation }
}
```

## Possible indicators of breaking ISP

1. Unimplemented methods
2. Interfaces with low cohesion
3. Fat interfaces

---
# Dependency Inversion Principle

High level modules should not depend on low-level modules. Both should depend on abstractions.

Abstractions should not depend on details. Details should depend on abstractions.

**High level module**: PaymentProcessor
**Low level modules**: GooglePayProcessor, ApplePayProcessor, WireTransfer

## Example

In the following example, high level modules are dependent on low level modules which breaks DIP.
```php
class SQLProductRepository {

	public function getAllProductNames() {
		return [...];
	}

}

class ProductCatalog {

	public function listAllProducts() {
		$sqlProductRepository = new SQLProductRepository();
		
		$sqlProductRepository->getAllProductNames();
	}

}

```
The application is tightly coupled to SQL Product Repository. If tomorrow we change the database to NoSQL, all the concrete classes need to be updated to change the dependency (SQLProductRepository).

**Solution** to this would be to depend on an abstraction instead.
```php
interface ProductRepository {
	public function getAllProductNames();
}

class SQLProductRepository implements ProductRepository {

	public function getAllProductNames() {
		return [...];
	}

}

class NoSQLProductRepository implements ProductRepository {

	public function getAllProductNames() {
		return [...];
	}

}

class ProductFactory {

	public static function create(): ProductRepository {
		return new SQLProductRepository();
	}

}

class ProductCatalog {

	public function listAllProducts() {
		$productRepository = ProductFactory::create();
		
		$productRepository->getAllProductNames();
	}

}
```