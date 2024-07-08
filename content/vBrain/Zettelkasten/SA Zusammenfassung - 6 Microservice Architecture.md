
> [!tldr]-
> - No assumption on the use of middleware (AppServer)
> 	- Typically employ client-side load balancing and circuit breakers
> 	- Containers enable flexible microservice deployments
> 	- Swarm/Kubernetes as container orchestration frameworks
> - Microservices are a modern form of dynamic components
> - The benefits of a microservice architecture come at the costs of higher complexity and performance overhead
> - Should only be used if the benefits outweigh the costs

## Motivation / DEVOPS Recap

### Classical Component Technologies

> [!caution] Huge gap between developer view and operation view!

- Deployment done on application server (middleware)
- Component deployment is complex and heavyweight process
- limited support for redeployment
- infrastructure is managed seperately

### Traditional Release Cycle

![[Pasted image 20240708091801.png|300]]

→ only shifts the blame

### DevOps

> [!info]
> DevOps is the philosophy of unifying development and operations at the culture, practice, and tool levels, to achieve accelerated and more frequent deployment of changes to production.

![[Pasted image 20240708091901.png|300]]

- development, operations and QA no longer strictly seperated teams
- automate as much as possible
- developers deploy and provision infrastructure
- rapid release cycle witha quick feedback loop

### DevOps Enablers

- Continuous Integration
- Microservices
- Continuous Delivery
- Infrastructure-as-Code

### Microservices

> [!success]
> aims to combine the benefits of both CBSE and SOA, while enabling DEVOPS
> - CBSE: build software from independently developed components without understanding their details
> - SOA: Deploy and operate components independently as services that can be used by others without having knowledge of their implementation, deployment, and operation details

→ weak coupling for maximum flexibility and interoperability

- devs responsible for deployment + operations as well
- minimum assumptions about component model & execution environment
- interfaces based on lightweight protocols (REST) → __indepentent of technology__
- fine-grained, clearly scoped services → __single responsibility principle__
- no middleware (App Server) → __flexibilty__

## Microservice Architecture

> [!summary]
> - Microservice applications are composed of a set of independent, distributed, and context-bounded services
> - Each service only has a fine-grained __single purpose__, leading to small and manageable units (hence the name __micro__service)
> - The services can be deployed and scaled __independently__, and they __communicate__ with each other over __network__ (e.g., using REST)
> - Usually, each service is __responsible__ of managing its __own data

> [!question] Are Microservices "Components"?

→ No, since no contractually specified interfaces

## Microservice Design Principles (IDEALS)

- __Interface Segregation__
	- each client sees the interface it needs
	- BFF (Backend for Frontends) pattern
![[Pasted image 20240708093426.png]]

- __Deployability__
	- high number of deployment units → automated deployment & operations needed (Container Management Software - Docker)
- __Event-Driven__
	- "Activated" by request from user/other service → improves scalability & throughput
- __Availabilty over Consistency__
	- CAP Theorem: In distributed data storage, you can't provide all three (Pick 2): 
		- Consistency
		- Availabilty
		- Partition Tolerance 
- __Loose Coupling__
	- publish/subscribe messaging
	- BFF Pattern
	- DB for each microservice
- __Single Responsiblity__
	- only one reason to change

## Microservices Challenges in Practice

- __Microservice Discovery__
	- loose coupling + no middlewhere → where to send requests?
	- solution: Registry Service (itself a microservice)
		- provides keep-alive functionality
		- no typing system → name-based or resource-based
- __Microservice Load Balancing__
	- clients get addresses of all services from registry. Which service to call?
	- solution: Clinet-side Load Balancer
		- autonomous (unaware of target state or other caller states)
		- no middleware
- __Microservice Circuit Breaker__
	- what happens if service fails?
	- detect failure and reroute to another service instance

> [!caution] Dependency (Registry, Load Balancer, ...) maangement systems are important for things to not break!

## Microservice Deployment and Lifecycle

### Virtualization in Distributed Architectures

- enables pre-configurd templates of VMs, easy to start/stop
- automated deployment
- Type-1-Hypervisor runs directly on hardware as a minimal OS

> [!question] Deciding When and How to Scale?
> - Management software (OpenStack, CloudStack, EC2)
> - must be configured with rules
> - Proactive Scaling (start new instance if an increase is predicted) is better than Rule-based Scaling (start new instance on CPU > 80%)

> [!question] Use a separate VM template for each microservice?
> 
> > [!success] Pros
> > -  Fine granular control for each service instance
> > -  Instances can be easily added / removed (migration is not necessary: stateless)
> > - Failure resilience: Failing services can simply be shut down and replaced
>
> > [!fail] Cons
> > - Overhead of many VMs; performance overhead of virtualization

### Containers

- Old Solution: VM
	- easy to migrate
	- overhead
- New Solution: Containers (e.g. Docker)
	- application-level virtualization
	- share the OS kernel
	- lower overhead
	- harder to  migrate running stateful services
	- fewer performance isolation options


> [!tip] Containers in Continuous Integration
> - Goal: Automatically create Container/VM template as part of build process
> - On new version: kill old containers, start containers with new version
> - Requires “container creation language” with container build tools (e.g., Docker)
> - Docker also provides container management CLI and tools

## Docker Swarm & Kubernetes

> [!fail] Drawbacks of Microservices
> - send requests to which instance?
> - where/how to deploy?
> - when to register a service instance?
> - when to scale?

### Docker Swarm

- Docker administation and abstraction layer
	- Container updates
	- Frontend Load balancing
	- Service definition/replication
	- placement prefences

### Kubernetes

- Standard and CLI for managing container-based services
	- even higher abstraction level than Docker Swarm, more features
		- Pod / Service registry
		- Front-end load balancing
		- Load balancers for inter-service communication
		- Keep-alive and health checks
		- ==Microservice does not need to contain client-side load blancer, registry client, ... → Kubernetes handles that administration==
- Groups containers into __Pods__
	- pods are the management entities of Kubernetes
	- configures dusing a declarative description file
	- Prods are closed ot the outside world and can only be accessed using the service abstraction
- Prods of a common type are grouped into a __Deployment__
	- Services can be
		- managed top-down
		- (auto-)scaled
		- load-balanced

> [!Question] Is Kubernetes the new Application Server
> Features are similar: Pod, Frontend load balancing, keep alive, ...
> Kubernetes replaces Componetns with Containers (more powerfull, more diversity in implementation)

### Microservice Trade-Offs

![[Pasted image 20240708120731.png|400]]

> [!success] Benefits
> 1. Strong Module Boundaries
> 2. Independent Deployment
> 3. Technology Diversity

> [!fail] Costs
> 1. Distribution → Complexity + Network overhead
> 2. Eventual Consistency
> 3. Operational Complexity
