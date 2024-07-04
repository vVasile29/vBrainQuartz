> [!tldr]-
> #### Key Properties
> 
> - **Simplicity**: _Easy_ to understand and maintain.
> - **Modularity**: _Independent_, easy to maintain, test, and reuse.
> - **Testability**: _Validates_ system functions correctly.
> - **Deployability**: Continuous _deployment_.
> - **Performance**: _Timely_ user response.
> - **Scalability**: _Future_ performance _scaling_.
> - **Elasticity**: _Dynamic scaling_.
> - **Reliability**: High _functionality_, quick _repairs_.
> - **Fault Tolerance**: Operates despite _failures_.
> 
> #### Styles Overview
> 
> - **Monolithic**: _Simple_, but __single point of failure__, limited __scalability__.
> - **Layered**: _Simple_, _low cost_, but generally __poor__.
> - **Pipeline**: _Simple_, _modular_, _testable_, but __poor__ in other aspects.
> - **Microkernel**: _Simple_, _performant_, _low cost_, but poor __scalability__, __elasticity__, __reliability__, __fault tolerance__.
> - **Distributed**: _Scalable_, _robust_, but __complex__, __network__-dependent.
> - **Service-based**: Generally _good_, but not very __scalable__.
> - **Event-driven**: _Good_, but __complex__, __hard to test__.
> - **Space-based**: _Good_, but __complex__, __hard to test__, __costly__.
> - **Microservice**: Generally _great_, but __complex__, __costly__, potential __network issues__.

>[!note] 
>Software Architecture is always a _trade-off_, no universal best "style"
### 2.1 Architectural Properties

- __Simplicity__: _easy_ to understand and maintain
- __Modularity__: _independence_, easier to maintain, test, analyse, reuse ([[Software Architecture - Software Metrics|test]])
- __Testabillty__: _validate_ that system functions correctly, tied with modularity
- __Deployability__: DevOps, continuously _deployed_
- __Performance__: ability to answer requests from a user in _time_
- __Scalabilty__: _scale_ performance (_in the future_)
- __Elasticity__: _scale_, but _dynamically_ in the moment
- __Reliability__: system _functions properly_ (high MTTF (_failure_) and low MTTR (_repair_))
- __Fault tolerance__: _still runs_, even in the presence of software/hardware failures

### 2.2 Achitectural Style Evaluation

- Anti-Pattern: _Big Ball of Mud_ => Lack of intentional/useful architecture

## Achitectural Styles Evaluation

### Monolithic

>[!info]
> - ==one== deployable unit
>- Pro: simple
>- Con: _single point of failure_, limited scalability

#### Layered Architecture

>[!summary] 
>_simple_ and _low cost_, but ==BAD== at everything else

- single deployment unit with functionality grouped by technical categories
- open/closed __layers__ define whether you must go through one layer to go one step deeper

![[Pasted image 20240506150553.png|600]]

![[Pasted image 20240506150420.png|600]]

#### Pipeline Architecture

>[!summary] 
>_simple_ and _low cost_, a little bit _modular_ and _testable_ (which always correlates!), rest ==BAD==

- System composed of _pipes_ and _filters_, with pipees forming a _one-way_ communication between filters
	- like `.map().filter().collect()` in Java, basically performing operations in a functional programming manner
- A filter typically belogs to one of four types
	1. __producer__ `() -> T`
	2. __transformer__ `.map()`
	3. __tester__ `.reduce()`
	4. __consumer__ `T -> ()`

![[Pasted image 20240506175636.png]]

#### Microkernel Architecture

>[!summary] 
>_simple_, _performant_ and _low cost_, but ==BAD== __scalable__, __elastic__ and also bad __reliability & fault tolerance__ (correlates again!) - rest _meh_

- adds _plugins_ to _add functionality_ (like a __browser__ with __extentions__ or intellij, obsidian, anything with plugins)
- things break when plugins have _dependencies_ with each other and one part gets changed for the worse

![[Pasted image 20240506181526.png | 600]]

### Distributed

> [!info]
> - multiple ==independent units== communicating over a _network_
>- Pro: Scalability, elasticity, robustness, availabilty
>- Con: more _complex_, dependant on reliability, latency, bandwith, cost, security (of _network_)

#### Service-based Architecture

>[!summary] 
> pretty good overall, only NOT really _scalable_

![[Pasted image 20240506184217.png|600]]

- well-defined independent domains deployed as _seperate units of software_
- seperates the application logic into several domain-partitioned services (domain services)
- usually a _seperate user interface and a monolithic database_

![[Pasted image 20240506184316.png]]

#### Event-driven Architecture

>[!summary] 
>overall good, but very _complex_ and _hard to test_!

- _asynchronously_ responds to events that are triggered in a system
- _messaging/streaming_ brokers (accept events and send messages)
	- _broker_ topology: __no central__ event mediator, event goes directly to channel
	- _mediator_ topology: __central point__ where events are handed to event channels

![[Pasted image 20240506184933.png]]

![[Pasted image 20240506184950.png|600]]

#### Space-based Architecture

>[!summary] 
>very _complex_, _hard to test_ and _costly_, otherwise good

![[Pasted image 20240506185339.png|600]]

- all _transactional_ data is _cached in memory_ 
- seperates database from all processing
- data updates are _asynchronously_ synced with the database and shared with all active processing units
- services don't interact transactionally with Database
	- services receives request (addCustomer)
	- adds this to cache
	- data gets replicated to other services
		- then sends it to data _writer_ which then writes data to DB asynchronously, then its back in sync
- _problem_ at archived/cold _start data_, then we have to _query_ the database via the _reader_

![[Pasted image 20240506185547.png]]

#### Microservice Architecture

>[!summary] 
>_complex and costly_, can have __bad performance__ due to _network_ issues, _REST_ (pun intended) is GREAT

![[Pasted image 20240506190019.png|600]]

- __single purpose__ functions deployed as _seperate units_ with each unit owning its own data

![[Pasted image 20240506190001.png | 600]]
