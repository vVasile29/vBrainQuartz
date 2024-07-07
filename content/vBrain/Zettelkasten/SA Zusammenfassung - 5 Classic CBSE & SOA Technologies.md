
## Component-based Software Engineering

> [!NOTE]
> how to _design and compose_ software systems from _independently_ __developed__ _building_ _blocks_ (software componets provided by third parties) _without understanding_ their internals

![[Pasted image 20240530223808.png]]

## Service-oriented Architecture (SOA)

> [!NOTE]
> how to _compose software_ systems from _independently_ __operated__ _services_ (deployed componets) _without having knowledge_ and understanding of their implementaion, deployment and operation details

-> AWS lambdas, service/functions you use
-> outsourcing, B2B, loose coupling, flexibility, cost transparency

> [!info] Service-oriented architecture 
> a way of designing, developing, deploying and managing systems, in which
> 
> - Services provide **reusable** business functionality,
> - Service consumers are built using functionality from available services,
> - Service **interface definitions** are first-class artifacts,
> - SOA infrastructure enables **discovery, composition, and invocation** of services,
> - Protocols are predominantly, but not exclusively, **message-based document exchanges**.

> [!info] Services 
> reusable components that represent business tasks
> 
> - Customer lookup
> - Credit card validation
> - Weather
> - Hotel reservation

## Services can be

- _Globally distributed_ across organizations
- _Reconfigured_ into new business processes

> [!tip]
> Service interface definitions are well-defined first-class artifacts (ideally) available in a service repository

## Clients for the functionality provided by the services

- End-user applications
- Internal systems
- External systems
- Composite services

## Consumers programmatically bind to services

![[Pasted image 20240530224321.png]]

![[Pasted image 20240530224330.png]]

![[Pasted image 20240530224342.png]]

![[Pasted image 20240530224353.png]]

### Requestor-Provider-Broker

![[Pasted image 20240530224412.png]]

## Java Enterprise Edition

![[Pasted image 20240530223826.png]]

### Enterprise Java Beans

- very heavy objects that do business logic
	- transaction services
	- naming services
	- security functions

![[Pasted image 20240530223930.png]]

- jigsaw is a module system introduced in Java 9
	- previously JARs as unit of modularity "JAR Hell"
