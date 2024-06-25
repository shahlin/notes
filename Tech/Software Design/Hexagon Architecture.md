# Hexagonal Architecture
> [!IMPORTANT]  
> Core Principle: Isolating the application's business logic

Hexagonal Architecture is also called the Ports and Adapters architecture. The fundamental idea is that applications should be driven by users and programs, batch scripts, or automated tests. It tries to push for development and testing in isolation (free from the external dependencies like database, etc)

Hexagonal architecture resembles Domain-Driven Design (DDD), which emphasizes the importance of the business domain over technology. But they're 2 different concepts, although they can complement each other.

## Traditional layered architecture
1. MVC or 3 tiered architecture is one that's commonly known. MVC splits the application into 3 layers: Presentation layer, logic layer and data layer
2. In Eric Evans' DDD Book, he proposed a 4-tier architecture. A domain layer with the business logic and the other 3 layers: User Interface, Application and Infrastructure
3. While layered architecture provided a good way to separate concerns, there was no natural mechanism to detect when logic leaks between layers. There would be scenarios where the business logic would leak into the presentation layer, etc

## Origin Story
1. In 2005, Alistair Cockburn realized that there wasn’t much difference between how the user interface and the database interact with an application
2. Since they are both external actors which are interchangeable with similar components that would interact with an application
3. This way, one could focus on keeping the application agnostic of these "external" actors, allowing them to interact via Ports and Adapters
4. This would avoid entanglement and logic leakage between business logic and external components

## The Hexagonal Architecture
1. Allows input by users or external systems to arrive to the Application at a Port via an Adapter
2. Allows output to be sent out from the Application through a Port to an Adapter
3. This creates an abstraction layer that protects the core of an application and isolates it from external tools and technologies.
4. Everything on the outside can depend on the inside but the inside cannot depend outwards (See: [Onion Architecture](https://medium.com/expedia-group-tech/onion-architecture-deed8a554423))

![hexagon](https://github.com/shahlin/notes/assets/32275018/aa81e86c-61dc-4d48-a104-366644fa22d7)


### Ports
1. Ports are technology-agnostic entry points. It determines the interface which will allow external actors to communicate with the Application, regardless of who or what will implement said interface
2. Example: A USB port allows multiple types of devices to communicate with a computer as long as they have a USB adapter
3. Ports also allow the Application to communicate with external systems or services, such as databases, message brokers, other applications, etc.
4. Technically, ports are interfaces that the adapters will implement

### Adapters
1. The Adapter will initiate the communication with the Application through a Port using a specific technology like REST controllers
2. There can be multiple adapters to a port

### Application
1. Application is the core of the system
2. It contains the services which orchestrate the functionality or the 'use cases'
3. It also contains the Domain Model which is the Business Logic in Aggregates, Entities, Value Objects
4. It is represented by a hexagon which receives commands or queries from the Ports, and sends requests out to other external actors, like databases, via Ports as well.

### Driving Side vs Driven Side

![hexagon1](https://github.com/shahlin/notes/assets/32275018/fe856a11-320f-40ed-92e9-2d28468d4c35)

#### Driving Side
1. Driving (or primary) side is the one that initiates the interactions (left side)
2. For example, a driving adapter could be a rest controller that takes user inputs and passes it on to the Application via a Port

#### Driven Side
1. Driven side is the one that is 'kicked into behaviour' by the Application
2. For example, a database adapter is called by the Application (via a Port) to save an entity to the database

### Technical Implementation
1. Ports will be represented as **interfaces** in code
2. Driving Adapters will use a Port and an Application Service will implement the Interface defined by the Port, in this case both the Port’s interface and implementation are inside the Hexagon
3. Driven adapters will implement the Port and an Application Service will use it, in this case the Port is inside the Hexagon, but the implementation is in the Adapter (outside of the Hexagon)
4. The entry point (Java's _main_ method for example) to the application will be in the adapter as that's where the user interacts (in the _driving_ adapters). For the driven adapters, the implementations are not in the hexagon (domain logic), so for those implementations to be available in the application context (for the domain logic to call it), it makes sense for the entry point to be in adapters

#### Dependency Inversion from SOLID Principles
1. High-level modules should not depend on low-level modules. Both should depend on abstractions
2. Abstractions should not depend on details. Details should depend on abstractions

## Benefits of Hexagon Architecture
1. Separation of domain logic from the application logic
2. Avoids vendor lock in as the domain logic is not dependant on any technology
3. Goes well with Domain Driven Design

## References
1. https://medium.com/ssense-tech/hexagonal-architecture-there-are-always-two-sides-to-every-story-bc0780ed7d9c
