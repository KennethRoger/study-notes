# Architectural Pattern vs Design Pattern

## Types of Software Architecture Patterns 

1. Layered Architecture Pattern 

2. Client-Server Architecture Pattern

3. Event-Driven Architecture Pattern 

4. Microkernel Architecture Pattern 

5. Microservices Architecture Pattern

6. Space-Based Architecture Pattern

7. Master-Slave Architecture Pattern

8. Pipe-Filter Architecture Pattern

9. Broker Architecture Pattern

10. Peer-to-Peer Architecture Pattern

# Microservices

* an architectural approach to developing software applications
* collection of small, independent services that communicate with each other over a network

Instead of building a **monolithic application** where all the functionality is tightly integrated into a **single codebase**, microservices break down the application into **smaller**, **loosely coupled** services.

**Microservice** - Small, loosely coupled service that is designed to perform a specific business function. Can be developed deployed and scaled independently

**Monolithic application** - A single, unified software application where all the components (like user interface, business logic, and data access layer) are tightly coupled and run as a single service.

**Single codebase** - All the code for your entire application lives in one place, typically in one version-controlled repository (like a Git repo).

**Loosely Coupled** - Different parts (modules, services, or components) of your system are independent of each other. Changes in one part should not significantly affect others.

## Main Components of Microservices architecture

1. **Microservices:** Small, loosely coupled services that handle specific business functions, each focusing on a distinct capability.

2. **API Gateway:** Acts as a central entry point for external clients also they manage requests, authentication and route the requests to the appropriate microservice.
eg: express gateway, Kong, Traefik, NGINX with custom config

3. **Service Resgistry and Discovery:** Keeps track of the locations and addresses of all microservices, enabling them to locate and communicate with each other dynamically.
eg: Docker compose build-in DNS, Kubernetes DNS

4. **Load Balancer:** Distributes incoming traffic across multiple service instances and prevent any of the microservice from being overwhelmed.
eg: NGINX, Kubernetes Ingress controller

5. **Containerization:** Docker encapsulate microservices and their dependencies and orchestration tools like Kubernetes manage their deployment and scaling.
eg: Docker

6. **Event Bus/Message Broker:** Facilitates communication between microservices, allowing pub/sub asynchronous interaction of events between components/microservices.
eg: Rabbit MQ, Kafka, Redis streams

7. **Database per Microservice:** Each microservice usually has its own database, promoting data autonomy and allowing for independent management and sacling.

8. **Caching:** Cache stores frequently accessed data close to the microservice which improved performance by reducing the repetitive queries.
eg: Redis

9. **Fault Tolerance and Resilience Components:** Components like cricuit breakers and retry mechanisms ensure that the  system can handle failures gracefully, maintaining overall functionality.

10. **CI/CD**: eg: Github actions + docker, Gitlab


## Design Patterns for Microservices Architecture

1. **API Gateway Pattern**

    Acts as a central entry point for external clients also they manage requests, authentication and route the requests to the appropriate microservice.

2. **Service Registry Pattern**

    Keeps track of the locations and addresses of all microservices, enabling them to locate and communicate with each other dynamically.

3. **Circuit Breaker Pattern**

    If a service fails repeatedly, the circuit breaker trips, preventing further requests to that service. After a timeout period, it allows limited requests to test if the service is back online. This reduces the load on failing services and enhances system resilience.

4. **Saga Pattern**

    This pattern is useful for managing complex business processes that span multiple services. Instead of treating the process as a single transaction, the saga breaks it down into smaller steps, each handled by different services. If one step fails, compensating actions are taken to reverse the previous steps. This way, you maintain data consistency across the system, even in the face of failures.

5. **Event Sourcing Pattern**

    Instead of storing just the current state of an application, this pattern records all changes as a sequence of events. Each event describes a change that occurred, allowing services to reconstruct the current state by replaying the event history. This provides a clear audit trail and simplifies data recovery in case of errors.


6. **Strangler Fig Pattern**

    This pattern allows for a gradual transition from a monolithic application to microservices. New features are developed as microservices while the old system remains in use. Over time, as more functionality is moved to microservices, the old system is gradually "strangled" until it can be fully retired. This approach minimizes risk and allows for a smoother migration.

7. **Bullkhead Pattern**

    Similar to compartments in a ship, the bulkhead pattern isolates different services to prevent failures from affecting the entire system. If one service encounters an issue, it won’t compromise others. By creating boundaries, this pattern enhances the resilience of the system, ensuring that a failure in one area doesn’t lead to a total system breakdown.

8. **API Composition Pattern**

    When you need to gather data from multiple microservices, the API composition pattern helps you do so efficiently. A separate service (the composition service) collects responses from various services and combines them into a single response for the client. This reduces the need for clients to make multiple requests and simplifies their interaction with the system.

9. **CQRS Design Pattern**

    CQRS divides the way data is handled into two parts: commands and queries. Commands are used to change data, like creating or updating records, while queries are used just to fetch data. This separation allows you to tailor each part for its specific purpose. For instance, the command side can focus on enforcing business rules, while the query side can be optimized for fast data retrieval. This pattern is especially helpful in applications with a lot of read and write operations, as it enhances performance and scalability by allowing for different optimizations for each side.

**IMP:** Docker, Kubernetes, and container orchestration patterns

## Anti-Patterns for Microservices Architeture

* When microservices share a single centralized database, it can compromise their independence and scalability.
* Microservices that frequently communicate for minor tasks can create excessive network traffic, leading to delays and increased latency.
* Creating too many microservices for small functions can add unnecessary complexity to the system.
* If the boundaries between microservices are not clearly defined, it can cause confusion about their responsibilities.
* Failing to address security issues in microservices can expose the system to vulnerabilities and potential data breaches.

## Microservice communication patterns

Microservices Communication Patterns explore how small, independent services in a software system talk to each other. These patterns are crucial for ensuring that microservices work together smoothly and efficiently. They cover methods like synchronous and asynchronous messaging, using APIs, message brokers, and service registries. Understanding these communication methods helps developers build resilient, scalable, and maintainable applications

The fundamentals of microservices communication in system design involve understanding how these independent services interact to form a cohesive application.Here are the key aspects:

* **Synchronous Communication:** Services communicate in real-time, waiting for a response before proceeding. Common protocols include HTTP/HTTPS using REST or gRPC.
* **Asynchronous Communication:** Services interact without waiting for an immediate response, often through message brokers like RabbitMQ, Kafka, or AWS SQS.
* **Message Brokers:** These facilitate asynchronous communication by allowing services to send and receive messages without direct interaction. Examples include Kafka, RabbitMQ, and AWS SNS/SQS.
* **Service Discovery:** In dynamic environments where services scale up and down, service discovery tools (e.g., Consul, Eureka) help services find and communicate with each other.
* **Load Balancing:** Distributes incoming requests across multiple instances of a service to ensure reliability and efficiency. Tools like NGINX, HAProxy, or cloud-native solutions handle this.
* **Circuit Breakers:** These prevent cascading failures by stopping requests to a failing service, allowing it to recover. Libraries like Hystrix implement this pattern.
* **API Gateway:** Acts as a single entry point for clients, routing requests to the appropriate services, handling tasks like authentication, rate limiting, and logging.

### Importance of Communication Patterns in Microservices

Communication patterns in microservices are crucial for several reasons:

* **Scalability:** Proper communication patterns allow microservices to scale independently. By decoupling services, each can be scaled up or down based on its specific demand without affecting others.
* **Resilience and Fault Tolerance:** Communication patterns like circuit breakers and retry mechanisms help build resilient systems. They prevent failures in one service from cascading to others, ensuring the overall system remains robust.
* **Flexibility and Agility:** By using appropriate communication patterns, teams can develop, deploy, and update services independently. This flexibility speeds up development cycles and allows for quicker adaptations to changes.
* **Improved Performance:** Efficient communication patterns reduce latency and improve the performance of the system. For instance, asynchronous communication can help offload tasks, making services more responsive.
* **Simplified Maintenance:** Clear and well-defined communication patterns make the system easier to understand and maintain. They help in isolating issues, as well-defined interfaces and communication methods make debugging and troubleshooting more straightforward.
* **Data Consistency and Integrity:** Patterns like distributed transactions and eventual consistency ensure that data remains accurate and consistent across different services, even in the presence of network partitions or failures.

### Communication Protocols Used in Microservices

Microservices architecture relies on various communication protocols to enable efficient and effective interaction between services. Here are some commonly used communication protocols in microservices:

* **REST (Representational State Transfer):** A widely used protocol for synchronous communication, leveraging standard HTTP methods (GET, POST, PUT, DELETE) for CRUD operations.
* **GraphQL:** An alternative to REST, allowing clients to request specific data, reducing over-fetching and under-fetching of information.
* **gRPC:** A high-performance, open-source RPC framework developed by Google. It uses HTTP/2 for transport, Protocol Buffers (Protobuf) for interface definition, and supports multiple programming languages. gRPC is efficient for low-latency and high-throughput communication.
* **WebSockets:** Enables full-duplex communication channels over a single, long-lived connection. Useful for real-time applications where services need to push updates to clients or other services.

### Synchronous Communication Patterns

Synchronous communication patterns in microservices involve direct interaction between services where one service sends a request and waits for a response before continuing its process. This type of communication is often used for real-time operations and immediate data consistency. Here are some common synchronous communication patterns in microservices:

* **Client-Side Load Balancing:** The client manages a list of available service instances and selects one to send a request to, typically using a load balancing algorithm. Distributing traffic among multiple instances of a service to ensure high availability and reliability.
* **Server-Side Load Balancing:** A load balancer sits between the client and the service instances, directing incoming requests to appropriate service instances based on a load balancing strategy. Centralized load management, easier to manage and scale services.
* **API Gateway:** A single entry point for all client requests, which routes requests to appropriate microservices and often handles cross-cutting concerns like authentication, rate limiting, and logging. Simplifying client interactions, centralizing security and monitoring.
* **Service Registry and Discovery:** Services register themselves with a service registry, which clients query to discover available service instances for direct communication. Dynamic environments where services scale up and down frequently.
* **Service Mesh:** A dedicated infrastructure layer that manages service-to-service communication, including load balancing, service discovery, and security. Enhancing observability, security, and reliability in large microservices deployments.
* **Circuit Breaker:** A pattern that detects failures and prevents requests from being sent to a failing service until it recovers, thereby avoiding cascading failures. Enhancing system resilience and fault tolerance.
* **Bulkhead:** Isolates different parts of the system to prevent failures in one part from affecting others. Each part has its own resources and limits. Increasing resilience by containing faults within isolated components.

### Asynchronous Communication Patterns

Asynchronous communication patterns in microservices allow services to interact without waiting for an immediate response. This approach is beneficial for decoupling services, enhancing scalability, and improving system resilience. Here are some common asynchronous communication patterns in microservices:

* **Publish-Subscribe (Pub-Sub):** Messages are published to a topic or channel and multiple subscribers can receive the messages. Broadcasting events to multiple services, real-time notifications, event-driven architectures.
* **Event Sourcing:** Instead of storing the current state, the system stores a sequence of events that describe state changes. The current state is derived by replaying these events. Building auditable systems, ensuring consistency in distributed systems.
* **Command Query Responsibility Segregation (CQRS):** Separates the read and write operations into different models, often combined with event sourcing. Enhancing performance, scalability, and maintainability by optimizing read and write paths.
* **Saga Pattern:** Manages distributed transactions by breaking them into a series of smaller, independent steps, each with its own compensating action for rollback. Ensuring data consistency across multiple services without using distributed transactions.
* **Dead Letter Queue (DLQ):** A special queue where messages that cannot be processed are sent for later inspection and handling. Handling message processing failures, ensuring no message is lost.
* **Backpressure:** A mechanism to handle situations where a producer of messages outpaces the consumer's ability to process them. Preventing system overload, maintaining stability under varying load conditions.
* **Polling:** Services periodically check a shared resource (e.g., a database or message queue) for new messages or tasks. Simple, low-complexity integration, batch processing.

#### Dimensions of a system that impact the execution flow and the communication style of a system
1. **Consumers**
Consumers of a system can be external programs, web/mobile interfaces, IoT devices etc. Consumer applications often deal with the server synchronously and expect the interface to support that. It is also desirable to mask the complexity of a distributed system with a unified interface for consumers. So it is imperative that our communication style allows us to facilitate it.

2. **Workflow management**
With many participating services, the management of a business-workflow is crucial. It can be implicit and can happen at each service and therefore remain distributed across services. Alternatively, it can be explicit. An orchestrator service can own up the responsibility for orchestrating the business-flows.
The orchestration is a combination of two things. A workflow specification, that lays out the sequence of execution and the actual calls to the services. The latter is tightly bound to the communication paradigm that the participating services follow. Communication style and execution flow drive the implementation of an orchestrator.

A third option is an event-choreography based design. This substitutes an orchestrator via an event bus that each service binds to.
All these are mechanisms to manage a workflow in a system

Let's consider **constraints** associated with them in the current context as we evaluate and select a communication paradigm.

* **Read/Write frequency bias**
Read/Write frequency of the system can be a crucial factor in its architecture. A read-heavy system expects a majority of operations to complete synchronously. A good example would be a public API for a weather forecast service that operates at scale. Alternatively, a write-heavy system benefits from asynchronous execution. An example would be a platform where numerous IoT devices are constantly reporting data. And of course, there are systems in between. Sometimes it is useful to favor a style because of the read-write skew. At other times, it may make sense to split reads and writes into separate components.

#### Synchronous
Synchronous communication is when a caller waits for a response before proceeding. It is widely used due to its conceptual simplicity and straightforward implementation, making it suitable for most situations.

It is commonly associated with HTTP, though alternatives like RPC are equally valid. Each component exposes a synchronous interface that other services call. An interceptor at the entry point pushes the request downstream, where subsequent calls remain synchronous — executed sequentially or in parallel — until processing completes. These calls may be explicitly managed by an orchestrator or flow organically across components.

* **Variations**
1. **De-centralized and synchronous**
In a de-centralized synchronous style, a request enters through an interceptor, which forwards it downstream and waits for responses. Each service may call others sequentially or in parallel until execution completes. This design is simple but distributes the workflow across services, creating tight coupling and limiting flexibility. Since requests can block multiple services, it is not suitable for complex or high-frequency workloads, though it satisfies synchronous consumer expectations.

2. **Orchestrated, synchronous and sequential**
An orchestrated synchronous sequential style improves this by introducing a central orchestrator that defines and manages the workflow. The orchestrator receives the request, coordinates calls to downstream services in sequence, and collects their responses. Workflow changes remain localized to the orchestrator, offering more flexibility, but it must hold all active requests and becomes a potential single point of failure. Despite this, the model works well for read-heavy systems.

3. **Orchestrated, synchronous and parallel**
An orchestrated synchronous parallel style extends the sequential model by allowing independent calls to run concurrently. Since orchestration is already centralized, enabling parallelism only requires workflow-level changes. This reduces response times and increases throughput, though it makes workflow management more complex. The trade-off is often worthwhile, as it improves both performance and efficiency while keeping communication synchronous and suitable for read-heavy architectures.

**Trade-offs**

Synchronous calls are generally simpler to understand, debug, and implement, but they introduce notable trade-offs in a distributed system.

* Balanced capacity
Synchronous systems require careful balancing of service capacity. A temporary spike at one component can flood downstream services with requests. Unlike asynchronous communication, which can buffer bursts with queues, synchronous systems need capacity alignment across all services. Without it, cascading failures may occur. Resilience mechanisms such as circuit breakers can help mitigate sudden traffic surges.

* Risk of cascading failures
Synchronous communication makes upstream services vulnerable to downstream failures. If a service fails, or even just responds too slowly, resources can be depleted quickly, causing a domino effect across the system. Mitigation involves consistent error handling, sensible timeouts, and enforcing SLAs. Bulkhead architectures or circuit breakers can also help prevent widespread failure. In a synchronous setup, deterioration in one service immediately impacts others.

* Increased load balancing and service discovery overhead
Ensuring redundancy and availability requires each service to run behind a load balancer, introducing an additional layer of indirection. Furthermore, every service must integrate into a central service discovery system, where it registers its address and resolves the addresses of downstream services.

* Coupling
Synchronous systems tend to become tightly coupled over time. Services bind directly to each other’s contracts, which forces early adoption of versioning even for minor changes. This increases system complexity or pushes contract changes downstream to all dependent consumers.

* Service mesh as a mitigation
Emerging paradigms such as service mesh (e.g., Istio, Linkerd, Envoy) address some of these issues by introducing decoupling, fault tolerance, and observability. While still maturing, service meshes promise more resilient synchronous systems by abstracting away much of the direct service-to-service dependency.

#### Asynchronous
Asynchronous communication suits distributed architectures because it removes the need to wait for responses, decoupling services. It can be implemented through direct RPC calls such as gRPC or via a central message bus. A message bus provides consistent communication and delivery semantics, making it a common choice. This style handles traffic bursts more effectively since services act as producers, consumers, or both.

* **Variations**
1. **choreographed asynchronous events**
In choreographed asynchronous events, services listen to a message bus and act when an event arrives. Each service may also trigger downstream events, which can introduce coupling if responsibilities like notification content or type are split between services and payloads. Even so, this approach is well suited for implicit tasks such as notifications, error handling, or indexing. It scales well for write-heavy systems but synchronous reads require mediation, and workflow management is decentralized.

2. **orchestrated asynchronous sequential communication**
With orchestrated asynchronous sequential communication, an orchestrator manages the workflow through the message bus. Services consume events, process them, and send responses back to the bus, which the orchestrator uses to trigger the next step. This centralizes workflow management, avoids coupling issues found in choreography, and supports write-heavy traffic. Mediation remains necessary for synchronous consumers.

3. **hybrid of orchestration and choreography**
A hybrid of orchestration and choreography is often used, where orchestration drives explicit flows while choreography handles implicit ones. For instance, the orchestrator can manage main workflow steps while leaf tasks like notifications or indexing are triggered as events. Careful boundary setting is required to avoid overlapping responsibilities.

Overall, asynchronous architectures handle bursts of requests more gracefully than synchronous ones. Queues absorb spikes and allow services to catch up even if traffic is high or a service goes down temporarily. Since all services connect to the queue, only the queue requires service discovery. Multiple instances of services can consume from the queue, removing the need for external load balancers and allowing linear scaling.

**Trade-offs**
* Asynchronous communication comes with trade-offs, mainly around complexity and how reads are handled. Service flows can be harder to trace, and both orchestrators and individual components must adapt to asynchronous execution. These systems are naturally suited for write-heavy workloads, but synchronous consumers often require mediation.

One option is a **synchronous wrapper(sync wrapper)**, which acts as an entry point over an asynchronous flow. It waits for a downstream response or timeout before replying to the client. This approach is easy to implement but introduces statefulness, since the request must return to the same server that received it. While not ideal for large-scale distributed systems, it works for systems with moderate performance needs.

Another approach is **CQRS**, where reads and writes are separated. Writes go to one database, and the data streams into a read-optimized database for queries. This makes the system more scalable and keeps components stateless, but it increases complexity and introduces eventual consistency. CQRS is better suited for systems with heavy read and write demands.

**Dual support** is a middle ground between a sync wrapper and CQRS where each service supports synchronous queries and asynchronous writes. Read queries can hop between components and be served synchronously for immediate responses, while writes flow into asynchronous channels to be processed later. This works well at medium scale because it keeps reads fast without the full complexity of CQRS, but the trade-off is you cannot independently optimize read and write paths as you can with CQRS, so it is less suitable for very high-traffic systems that need separate, read- and write-optimized stacks.

* In an asynchronous system, the message bus acts as the backbone since every service produces and consumes through it. This makes it a critical dependency and a potential single point of failure. If the bus cannot scale horizontally, it undermines the very goal of distributed architecture.

* Another trade-off is eventual consistency. Because writes flow asynchronously, queries may not always return the most up-to-date results. Over time the data converges to a consistent state, but this delay must be accounted for in both system design and user experience.

#### Hybrid
A hybrid approach combines synchronous and asynchronous communication, but this often amplifies the drawbacks of both. The system must manage two communication styles at once, with synchronous calls risking cascading failures and asynchronous calls adding design complexity. In practice, choosing one approach in isolation is usually more effective.


* As Martin Fowler notes, once microservices are chosen, the execution flow style must be deliberate. Asynchronous communication with a sync-over-async wrapper works best for write-heavy systems, while synchronous communication is a better fit for read-heavy systems. For systems that are both read and write heavy but operate at moderate scale, synchronous design keeps things simpler. At very high scale and performance needs, asynchronous design with a CQRS pattern is the stronger option.
# DOCKER

Docker is an open platform for developing, shipping, and running applications. Docker enables you to separate your applications from your infrastructure so you can deliver software quickly. With Docker, you can manage your infrastructure in the same ways you manage your applications. By taking advantage of Docker's methodologies for shipping, testing, and deploying code, you can significantly reduce the delay between writing code and running it in production.

## Container

Containers are isolated processes for each of your app's components.

A docker run command takes the following form:

```
$ docker run [OPTIONS] IMAGE[:TAG|@DIGEST] [COMMAND] [ARG...]
```

The docker run command must specify an image reference to create the container from.
The image reference is the name and version of the image. You can use the image reference to create or run a container based on an image.

`docker run IMAGE[:TAG][@DIGEST]` - create container and run it
`docker create IMAGE[:TAG][@DIGEST]` - only create container

### File System Mounts

By default, the data in a container is stored in an ephemeral, writable container layer. Removing the container also removes its data. If you want to use persistent data with containers, you can use filesystem mounts to store the data persistently on the host system. Filesystem mounts can also let you share data between containers and the host.

* Volume mounts
* Bind mounts

**Volume mounts**: `docker run --mount source=<VOLUME_NAME>,target=[PATH] [IMAGE] [COMMAND...]`
The data is persisted in a volume. Using a volume allows us to take down the services without losing the data. When we start the services again, the data will still be there (assuming we didn't delete the volume, of course!).

**Bind mounts**: `docker run -it --mount type=bind,source=[PATH],target=[PATH] busybox`

## Images

A container image is a standardized package that includes all of the files, binaries, libraries, and configurations to run a container.

**i.** Search for images using

```
docker search IMAGE
```

**ii.** Pull the image using

```
docker pull IMAGE
```

**iii.** List your downloaded images using

```
docker image ls
```

**iv.** List the image's layers using

```
docker image history
```

## Registry

An image registry is a centralized location for storing and sharing your container images. It can be either public or private.
While Docker Hub is a popular option, there are many other available container registries available today, including Amazon Elastic Container Registry (ECR), Azure Container Registry (ACR), and Google Container Registry (GCR). You can even run your private registry on your local system or inside your organization. For example, Harbor, JFrog Artifactory, GitLab Container registry etc.

### Registry vs Repository

A registry is a centralized location that stores and manages container images, whereas a repository is a collection of related container images within a registry.

## Docker Compose

With Docker Compose, you can define all of your containers and their configurations in a **single YAML file** (compose.yaml). If you include this file in your code repository, anyone that clones your repository can get up and running with a single command.

It's important to understand that Compose is a declarative tool - you simply define it and go. You don't always need to recreate everything from scratch. If you make a change, run docker compose up again and Compose will reconcile the changes in your file and apply them intelligently.

Use the `docker compose up` command to start the application

Use the `docker compose down` command to remove everything

## CI/CD

CI/CD falls under DevOps (the joining of development and operations teams) and combines the practices of continuous integration and continuous delivery. CI/CD automates much or all of the manual human intervention traditionally needed to get new code from a commit into production, encompassing the build, test (including integration tests, unit tests, and regression tests), and deploy phases, as well as infrastructure provisioning. With a CI/CD pipeline, development teams can make changes to code that are then automatically tested and pushed out for delivery and deployment. Get CI/CD right and downtime is minimized and code releases happen faster.

### What is CI?

Continuous integration is the practice of integrating all your code changes into the main branch of a shared source code repository early and often, automatically testing each change when you commit or merge them, and automatically kicking off a build. With continuous integration, errors and security issues can be identified and fixed more easily, and much earlier in the development process. By merging changes frequently and triggering automatic testing and validation processes, you minimize the possibility of code conflict, even with multiple developers working on the same application.

### What is Continuous Delivery?

Continuous delivery is a software development practice that works in conjunction with CI to automate the infrastructure provisioning and application release process.

Once code has been tested and built as part of the CI process, CD takes over during the final stages to ensure it's packaged with everything it needs to deploy to any environment at any time. CD can cover everything from provisioning the infrastructure to deploying the application to the testing or production environment.

With CD, the software is built so that it can be deployed to production at any time. Then you can trigger the deployments manually or move to continuous deployment, where deployments are automated as well.

### What is continuous deployment?

Continuous deployment enables organizations to deploy their applications automatically, eliminating the need for human intervention. With continuous deployment, DevOps teams set the criteria for code releases ahead of time and when those criteria are met and validated, the code is deployed into the production environment. This allows organizations to be more nimble and get new features into the hands of users faster.

While you can do continuous integration without continuous delivery or deployment, you can't really do CD without already having CI in place. That's because it would be extremely difficult to be able to deploy to production at any time if you aren't practicing CI fundamentals like integrating code to a shared repo, automating testing and builds, and doing it all in small batches on a daily basis.

### What is meant by continuous testing? 

Continuous testing is a software testing practice where tests are continuously run in order to identify bugs as soon as they are introduced into the codebase. In a CI/CD pipeline, continuous testing is typically performed automatically, with each code change triggering a series of tests to ensure that the application is still working as expected.

In continuous testing, various types of tests are performed within the CI/CD pipeline like:

- Unit testing, which checks that individual units of code work as expected
- Integration testing, which verifies how different modules or services within an application work together
- Regression testing, which is performed after a bug is fixed to ensure that specific bug won't occur again

### CI/CD fundamentals

- A single source repository -> Source code management (SCM) that houses all necessary files and scripts to create builds is critical. The repository should contain everything needed for the build. This includes source code, database structure, libraries, properties files, and version control. It should also contain test scripts and scripts to build applications.
- Frequent check-ins to main branch -> Integrate code in your trunk, mainline or master branch — i.e., trunk-based development — early and often. Avoid sub-branches and work with the main branch only.
- Automated builds -> Scripts should include everything you need to build from a single command. This includes web server files, database scripts, and application software. 
- Self-testing builds -> Testing scripts should ensure that the failure of a test results in a failed build. Use static pre-build testing scripts to check code for integrity, quality, and security compliance. Only allow code that passes static tests into the build.
- Frequent iterations -> Multiple commits to the repository results in fewer places for conflicts to hide. Make small, frequent iterations rather than major changes.
- Stable testing environments -> Code should be tested in a cloned version of the production environment. Create a cloned environment that's as close as possible to the real environment. Use rigorous testing scripts to detect and identify bugs that slipped through the initial pre-build testing process.
- Maximum visibility -> Use version control to manage handoffs so developers know which is the latest version. Maximum visibility means everyone can monitor progress and identify potential concerns.
- Predictable deployments anytime -> CI/CD testing and verification processes should be rigorous and reliable, giving the team confidence to deploy updates at any time.

### Common CI/CD tools

Tekton Pipelines is a CI/CD framework for Kubernetes platforms that provides a standard cloud-native CI/CD experience with containers.

Beyond Tekton Pipelines, other open source CI/CD tools you may wish to investigate include:

- Jenkins, designed to handle anything from a simple CI server to a complete CD hub
- Spinnaker, a CD platform built for multicloud environments.
- GoCD, a CI/CD server with an emphasis on modeling and visualization.
- Concourse, "an open-source continuous thing-doer."
- Screwdriver, a build platform designed for CD.

Teams may also want to consider managed CI/CD tools, which are available from a variety of vendors. The major public cloud providers all offer CI/CD solutions, along with GitLab, CircleCI, Travis CI, Atlassian Bamboo, and many others.

Additionally, any tool that’s foundational to DevOps is likely to be part of a CI/CD process. Tools for configuration automation (such as Ansible, Chef, and Puppet), container runtimes (such as Docker, rkt, and cri-o), and container orchestration (Kubernetes) aren’t strictly CI/CD tools, but they’ll show up in many CI/CD workflows.

#### Some Docker Terms

Content addressible layer
Union file system
view layer
virtual view
virtual union
Build Cache
Cache invalidation
Multi-stage builds
bridge network(virtual network)
port forwarding
container volumes

# KUBERNETES

Kubernetes is a portable, extensible, open source platform for managing containerized workloads and services, that facilitates both declarative configuration and automation. It has a large, rapidly growing ecosystem. Kubernetes services, support, and tools are widely available.

## Why K8s

Containers are a good way to bundle and run your applications. In a production environment, you need to manage the containers that run the applications and ensure that there is no downtime. For example, if a container goes down, another container needs to start. Wouldn't it be easier if this behavior was handled by a system?

That's how Kubernetes comes to the rescue! Kubernetes provides you with a framework to run distributed systems resiliently. It takes care of scaling and failover for your application, provides deployment patterns, and more. For example: Kubernetes can easily manage a canary deployment for your system.

Kubernetes provides you with:

* **Service discovery and load balancing** Kubernetes can expose a container using the DNS name or using their own IP address. If traffic to a container is high, Kubernetes is able to load balance and distribute the network traffic so that the deployment is stable.
* **Storage orchestration** Kubernetes allows you to automatically mount a storage system of your choice, such as local storages, public cloud providers, and more.
* **Automated rollouts and rollbacks** You can describe the desired state for your deployed containers using Kubernetes, and it can change the actual state to the desired state at a controlled rate. For example, you can automate Kubernetes to create new containers for your deployment, remove existing containers and adopt all their resources to the new container.
* **Automatic bin packing** You provide Kubernetes with a cluster of nodes that it can use to run containerized tasks. You tell Kubernetes how much CPU and memory (RAM) each container needs. Kubernetes can fit containers onto your nodes to make the best use of your resources.
* **Self-healing** Kubernetes restarts containers that fail, replaces containers, kills containers that don't respond to your user-defined health check, and doesn't advertise them to clients until they are ready to serve.
* **Secret and configuration management** Kubernetes lets you store and manage sensitive information, such as passwords, OAuth tokens, and SSH keys. You can deploy and update secrets and application configuration without rebuilding your container images, and without exposing secrets in your stack configuration.
* **Batch execution** In addition to services, Kubernetes can manage your batch and CI workloads, replacing containers that fail, if desired.
* **Horizontal scaling** Scale your application up and down with a simple command, with a UI, or automatically based on CPU usage.
* **IPv4/IPv6 dual-stack** Allocation of IPv4 and IPv6 addresses to Pods and Services
* **Designed for extensibility** Add features to your Kubernetes cluster without changing upstream source code

## Kubernetes Components

A Kubernetes cluster consists of a control plane and one or more worker nodes.

### Control plane components

Manage the overall state of the cluster:

**kube-apiserver**: The core component server that exposes the Kubernetes HTTP API.
**etcd**: Consistent and highly-available key value store for all API server data.
**kube-scheduler**: Looks for Pods not yet bound to a node, and assigns each Pod to a suitable node.
**kube-controller-manager**: Runs controllers to implement Kubernetes API behavior.
**cloud-controller-manager (optional)**: Integrates with underlying cloud provider(s).

### Node Components 

Run on every node, maintaining running pods and providing the Kubernetes runtime environment:

**kubelet**: Ensures that Pods are running, including their containers.
**kube-proxy (optional)**: Maintains network rules on nodes to implement Services.
**Container runtime**: Software responsible for running containers. Read Container Runtimes to learn more.

### Addons

Addons extend the functionality of Kubernetes. A few important examples include:

**DNS**: For cluster-wide DNS resolution.
**Web UI (Dashboard)**: For cluster management via a web interface.
**Container Resource Monitoring**: For collecting and storing container metrics.
**Cluster-level Logging**: For saving container logs to a central log store.

## Objects In Kubernetes

Kubernetes objects are persistent entities in the Kubernetes system. Kubernetes uses these entities to represent the state of your cluster.

Specifically they can describe:

* What containerized applications are running (and on which nodes)
* The resources available to those applications
* The policies around how those applications behave, such as restart policies, upgrades, and fault-tolerance

To work with Kubernetes objects—whether to create, modify, or delete them—you'll need to use the Kubernetes API. When you use the kubectl command-line interface, for example, the CLI makes the necessary Kubernetes API calls for you.

Almost every Kubernetes object includes two nested object fields that govern the object's configuration: the object `spec` and the object `status`. For objects that have a `spec`, you have to set this when you create the object, providing a description of the characteristics you want the resource to have: its desired state.

The `status` describes the current state of the object, supplied and updated by the Kubernetes system and its components. The Kubernetes control plane continually and actively manages every object's actual state to match the desired state you supplied.

For example: in Kubernetes, a Deployment is an object that can represent an application running on your cluster. When you create the Deployment, you might set the Deployment spec to specify that you want three replicas of the application to be running. The Kubernetes system reads the Deployment spec and starts three instances of your desired application--updating the status to match your spec. If any of those instances should fail (a status change), the Kubernetes system responds to the difference between spec and status by making a correction--in this case, starting a replacement instance.

### Describing a Kubernetes object

When you create an object in Kubernetes, you must provide the object spec that describes its desired state, as well as some basic information about the object (such as a name). When you use the Kubernetes API to create the object (either directly or via kubectl), that API request must include that information as JSON in the request body. Most often, you provide the information to kubectl in a file known as a manifest. By convention, manifests are YAML (you could also use JSON format). Tools such as kubectl convert the information from a manifest into JSON or another supported serialization format when making the API request over HTTP.

Here's an example manifest that shows the required fields and object spec for a Kubernetes Deployment:

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
spec:
  selector:
    matchLabels:
      app: nginx
  replicas: 2 # tells deployment to run 2 pods matching the template
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx:1.14.2
        ports:
        - containerPort: 80

```

One way to create a Deployment using a manifest file like the one above is to use the kubectl apply command in the kubectl command-line interface, passing the .yaml file as an argument. Here's an example:

```
kubectl apply -f https://k8s.io/examples/application/deployment.yaml
```

### Required fields

In the manifest (YAML or JSON file) for the Kubernetes object you want to create, you'll need to set values for the following fields:

* `apiVersion` - Which version of the Kubernetes API you're using to create this object
* `kind` - What kind of object you want to create
* `metadata` - Data that helps uniquely identify the object, including a name string, UID, and optional namespace
* `spec` - What state you desire for the object

#### Some K8s Terms

Declarative configuration
Pod
Node
Cluster
Deployment
Service
Volume
ConfigMap & Secret
namespace

## NGINX

nginx ("engine x") is an HTTP web server, reverse proxy, content cache, load balancer, TCP/UDP proxy server, and mail proxy server.

nginx has one master process and several worker processes. The main purpose of the master process is to read and evaluate configuration, and maintain worker processes. Worker processes do actual processing of requests. nginx employs event-based model and OS-dependent mechanisms to efficiently distribute requests among worker processes. The number of worker processes is defined in the configuration file and may be fixed for a given configuration or automatically adjusted to the number of available CPU cores

### Configuration File’s Structure

nginx consists of modules which are controlled by directives specified in the configuration file. Directives are divided into simple directives and block directives. A simple directive consists of the name and parameters separated by spaces and ends with a semicolon (;). A block directive has the same structure as a simple directive, but instead of the semicolon it ends with a set of additional instructions surrounded by braces ({ and }). If a block directive can have other directives inside braces, it is called a context (examples: events, http, server, and location).

Directives placed in the configuration file outside of any contexts are considered to be in the main context. The events and http directives reside in the main context, server in http, and location in server.

The rest of a line after the # sign is considered a comment. 

## CLEAN ARCHITECTURE

Clean Architecture is a software design philosophy introduced by Robert C. Martin, also known as Uncle Bob. The primary goal of Clean Architecture is to create systems that are:

- Maintainable: Easy to understand and modify, allowing developers to make changes with minimal risk of introducing errors.
- Testable: Designed in such a way that testing is straightforward, enabling the creation of automated tests for various parts of the system.
- Independent of Frameworks: Frameworks can be replaced with minimal impact on the system.
- Independent of UI: The user interface can change without affecting the underlying logic or business rules.
- Independent of Database: Data storage and retrieval mechanisms can change without affecting the system.
- Independent of Any External Agency: Business rules are not tied to any specific implementation detail, making the system adaptable to change.

### Layers of Clean Architecture

Clean Architecture organizes the system into several distinct layers, each with a specific responsibility. This structure promotes separation of concerns, maintainability, and scalability. Here’s a breakdown of the layers in Clean Architecture:

* Frameworks and Drivers (Outer Layer):
    The application layer contains all the external enablers and gadgets that it avails itself to. This is because of such things as web frameworks, database drivers, user interfaces, and other third-party libraries.
    It is the most dynamic and can most certainly experience constant changes and updates. It communicates with the system via the user interface adapters.
* Interface Adapters:
    This layer translates the data from their most commonly used format by use cases and entities into a format generated from external entities such as databases and website interfaces. It possesses controllers, presenters, gateways, and * APIs that are essential components of software.
    This eliminates the possibility of the inner layers changing due to new additions in the outer layers, thus maintaining the dependency rule.
* Domain Layer (Entities):
    This is the heart of the system and here lays the basic business rules in the enterprise. Activities are the least abstract and represent the most general and the highest level of knowledge in the given application. They can be something with methods or a container that holds a function.
* Application Layer (Use Cases):
    This layer describes data in the context of a particular application; it contains the business rules. It is responsible for managing the data flow between the entities as well as within the application to enable the right use cases to be implemented. This is because it aligns the user’s actions and communicates with the outer layers to execute instructions on a macro level.
    It is independent of application frameworks, databases, and user interfaces so that modifications to these components of the implementation do not impact the business rules.

### Design Principles in Clean Architecture

* SRP
* OCP
* LSP
* ISP
* DIP

# Necessary Tool and Concepts to Learn

Log Aggregation Tool (Humio)







   
