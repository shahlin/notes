# Service Mesh

## Basics
1. A service mesh is an infrastructure layer that can be added to your applications
2. Typically used in a microservices based application. It handles all communications between services
3. This visible infrastructure layer can document how well (or not) different parts of an app interact, so it becomes easier to optimize communication and avoid downtime as an app grows
3. Emerged around 2017
4. It gives you the ability to add features like observability, security, traffic management, etc without adding any of it to the source code
5. Usually in a kubernetes based system, when there is a growth in size and complexity, it becomes harder to understand and manage the system (doing discovery, load balancing, failure recovery, metrics, monitoring, etc)
6. A service mesh also often addresses more complex operational requirements, like A/B testing, canary deployments, rate limiting, access control, encryption, and end-to-end authentication

## Why do you need a service mesh?
1. Different teams may build individual microservices and choose their languages and tools. However, the microservices must communicate for the application code to work correctly
2. Application performance depends on the speed and resiliency of communication between services
3. Developers must monitor and optimize the application across services, but it’s hard to gain visibility due to the system's distributed nature. As applications scale, it becomes even more complex to manage communications
4. Service mesh helps in solving these issues

## Vendors
1. Istio
2. Linkerd
3. Kong
4. AWS App Mesh
