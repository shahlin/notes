Domain Driven Design or DDD to me, is a mindset shift in how we write code and go about the process of implementing something. It's more about thinking of the business domain/problem first and modeling the software as close as possible to the business domain.
I don't know how to properly define it yet. I am going to talk about all the concepts I've come across while trying to understand DDD and hopefully it makes sense as a whole.
# Concepts
1. Domain Model
2. Domain Expert
3. Ubiquitous Language
4. Bounded Context
5. Value Objects

## Domain Model
From my understanding, the Domain Model is the representation of your domain and it's translation within the code. It also represents all the relationships between each of the domain objects. It can be used to model the business logic and the behaviors. It's basically understanding the concepts involved in the domain and how they are interlinked.
I believe the first step to modeling the domain is to come together as a team and brainstorm the involved pieces and then see how they connect with each other.

[More info on Domain Models here](https://ddd-practitioners.com/home/glossary/domain-model/)
## Domain Expert
Domain Expert is someone who has deep knowledge about the problem in hand, or the domain. They are aware of the business rules, processes and concepts that are essential for the software being built. They serve as the main source of information for any questions related to the domain.
## Ubiquitous Language
Ubiquitous means 'common' or 'found everywhere'. Ubiquitous Language in DDD is a common language shared by all the members working on a system. This includes the domain experts, developers, leads, etc. The idea is to make sure everyone speaks the same language and so does the software.

Speaking in terms of code, this would mean your code structure and naming conventions closely represent the domain. The words that are used by the domain experts are the same names used within the code. For example, if we've had a discussion with the team on the 'Booking hotel room' feature, the code must also follow the same naming `bookRoom()` instead of naming something like `reserveRoom()`.

The advantages of using the same language spoken with the domain experts within the code are many. For one, it's easier to read and understand the code. The code becomes the documentation. Second, it's easy to communicate with non-technical members of the team because we're using a _ubiquitous language_ rather than a technical one.
## Bounded Context
> _A bounded context is simply the boundary within a domain where a particular domain model applies_

Bounded context is where a set of rules, terms and definitions apply or make sense. For example, "Property Listings", "User Profiles", and "Booking Management" are different bounded contexts within the same system.
Note that a large model can be broken down into multiple bounded contexts.


> :warning: Need more clarity and understanding

### Duplication over Coupling
While DRY (Don't Repeat Yourself) warns us against duplication, it is sometimes okay to have duplicates if it means lesser coupling. DRY is important within a bounded context and reducing coupling across bounded contexts is more important.
Example: In a consultancy software solution, it is okay to have 'Employee' class in both Finance and HR bounded contexts and under Sales we can have the same class but named as 'Consultant' to match the language of that context.

This:
```
java/
├─ finance/
	├─ Employee
	├─ EmployeeRepository
	├─ SalaryService
├─ sales/
	├─ Consultant
	├─ ConsultantRepository
	├─ RateCalculator
├─ hr/
	├─ Employee
	├─ EmployeeRepository
	├─ Course
	├─ CourseEnrolment
```

Is better than:

```
java/
├─ shared/
	├─ Employee
	├─ EmployeeRepository
├─ finance/
	├─ SalaryService
├─ sales/
	├─ RateCalculator
├─ hr/
	├─ Course

```
Reason:
- It lets the context evolve alone by itself without depending on others.
- It decreases cognitive load since you only have to think about your own bounded context
## Value Objects
DDD discourages the use of primitive types for domain objects. For example, you might write some code like this:
```java
public String getCustomerId() {
	return // ...
}
```

While this works, it's not recommended because of various reasons.
- They are not self-validating. Let's say our customer IDs are always 5 characters long. If we use a string, we allow the users to create customer IDs with any number of characters. We have to add a validation separately to avoid this.
- In case the naming is changed along the way or the language is not followed, it makes it unclear. One developer might name it `getUserId()` and the other might call it `getId()`. When they return primitive types, it's hard to understand what it is we're really returning. (Ideally they should be named correctly as per the ubiquitous language but that's a different point)
- Makes it flexible, we can change internal implementation or rules within the value object without affecting the usages.
Read more [here](https://medium.com/swlh/value-objects-to-the-rescue-28c563ad97c6). 

A better approach would be to do this instead:
```java
class CustomerId {
	...
}

...

public CustomerId getCustomerId() {
	return // ...
}
```
Another example is if you have an `Email` value object, you can have your email pattern matching validation within the object creation so whenever the object is created, you're sure to have a valid email.