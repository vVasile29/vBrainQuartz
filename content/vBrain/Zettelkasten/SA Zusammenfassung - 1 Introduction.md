
> [!TLDR]+
> **Software Architecture:**
>     
> - Focuses on creating _maintainable_ and _extensible_ software.
> - Involves designing on a larger scale, breaking software into smaller components (**Divide and Conquer**).
> - Ensures _low coupling_ between components and _high cohesion_ within them.
> 
> **Architecture Views:**
>     
> - Different _views_ (e.g., Functional, Physical, Behavioral) are used to address stakeholder concerns.
> - _UML diagrams_ are tools used within these views for detailed representation.
> 
> **Architectural Styles:**
>     
> - Defines a _set of constraints_ and relationships within a system.
> - Examples include _layered_, client-server, service-oriented, and _microservices_ architectures.
> 
> **Component-Based Software Engineering:**
>     
> - Emphasizes the use of _independent_, _self-contained_ components with defined _interfaces_.
> 
> **Distributed Architectures:**
>     
> - Systems can be **monolithic** or **distributed**, with distributed systems offering better scalability and robustness but increased complexity.
>   
> **Architectural Patterns:**
>     
> - _Recurring solutions to common architectural problems_, like the Model-View-Controller (_MVC_) pattern.
>    
> **Architectural Design Decisions:**
>     
> - Decisions impact _performance, security, maintainability_, and other quality attributes.
> - Trade-offs often required to _prioritize_ certain metrics over others.
>
> **Software Metrics:**
>     
> - Used to _evaluate_ design quality, system growth, and maintainability.
> - Include static code analysis, execution time analysis, and dynamic analysis (monitoring).

> [!attention] unmanageable Software → _Big Ball of Mud_

Software Architecture for _maintainable, extensible software_

- design on a ==larger== scale
- partition software into ==smaller== components (**Divide and Conquer**)
- ==low coupling== between components
- ==high cohesion== within components

	![[Pasted image 20240627114056.png]]
 
> [!info] Definition of Software Architecture
> Fundamental _organization_ of a system embodied in its _components_, their _relationships_ to each other and to the _environment_, and the _principles_ guiding its _design_ and evolution

### 1.1 Architecture Views

> [!info] Terminology
> - **UML diagrams** are _specific tools_ used within the context of software design
> - **Views** are _broader_ architectural _representations_ that may _incorporate_ various types of _diagrams_, including UML, to address different stakeholder concerns and provide a comprehensive understanding of the system.

| Architecture View                   | UML Diagram                                  |
| ----------------------------------- | -------------------------------------------- |
| ==Functional / Logical view==       | UML Component Diagrams                       |
| Code / Module view                  | UML Package Diagrams                         |
| Development / Structural view       | UML Package Diagrams                         |
| Concurrency / Process / Thread View | UML Component-, State- and Activity Diagrams |
| ==Physical / Deployment view==      | UML Deployment Diagrams                      |
| User Action / Feedback View         | UML Sequence Diagrams                        |
| Data view                           | UML Class Diagrams, ER Diagrams              |
| ==Behavioral View==                 | UML Sequence Diagrams                        |

#### Logical View - UML Component Diagramm

- large-scale organization of classes into software elements such as _packages_, _components_, _subsystems_, _layers_

![[Pasted image 20240627114906.png]]

#### Behavioral View - UML Sequence Diagramm

- shows how a set of objects _interact in a process over time_

![[Pasted image 20240627114930.png]]

#### Deployment View - UML Deployment Diagramm

- _distribution_ of software elements _on computational resources_ and the _communication_ among them
- UML Deployment Diagrams have two types of _nodes_
	- **device** node (_hardware_ node)
	- **execution environment** node (_software_ node)

![[Pasted image 20240627114945.png]]

### 1.2 Architectural Styles

> [!info] Definition
> An architectural style is a coordinated set of _architectural constraints_ that restricts the roles/features of _architectural elements_ and the _allowed relationships_ among those elements within any architecture that conforms to that stlye.

→ Architectural Design _Desicions_

| Category                    | Styles                                                                                               |
| --------------------------- | ---------------------------------------------------------------------------------------------------- |
| Structure                   | (object-oriented), layered, component-based, microkernel, pipeline                                   |
| Deployment                  | clinet/server, N-tier, three-tier, peer-to-peer                                                      |
| Communictation/Distribution | service-oriented (SOA), message-oriented/publish-subscriber, event-based / event-drived, space-based |

#### Architecture Layers

> [!info] Definition Layer 
> _Coarse-grained_ grouping of classes, packages, components, or subsystems that has _cohesive responsibility_ for a major aspect of the system

- **Separation of concerns** principle 
- Layered Architecture typically features
	- _UI_ / Presentation Layer
	- _Application_ (Business) Locig and Domain (Object) Layer
	- Technical Services / _Data_ Link Layer
- Data needs to flow through all Layers which causes **Sinkhole Problem**

#### Facades

> [!info] Definition
> - _entry point_ to a set of related software classes that are grouped together (__interface__). 
> - Hides internal details (**Black-Box-Principle**).

![[Pasted image 20240627140505.png]]

→ _reduces coupling_

#### Pros and Cons of Layered Architecture

##### Advantages

- simple
- abstracted due to seperation of layers
- isolation between layers protects against modification consequences
- low coupling

##### Disadvantages

- Scalability
- Data flow causes Sinkhole Problem

#### Component-Based Software Engineering

> [!info] Definition Software Component
> A __software component__ is a _unit of composition_ with contractually specified interfaces and explicit context dependencies. It _can be deployed independently_ and is subject to composition by _third parties_ 

- **Black Box Principle**
	- has provided (and requiered) _interfaces_
	- _hides_ its implementation
	- is self-contained
	- adheres to a component model

#### Software Components vs. Layers

- Components are _feature-oriented_
	- deliver some well defined chunk of functionality (similar to **AWS Lambdas**)
	- typically partition the functionality within a layer, may crosscut through multiple layers

#### Monolithic vs. Distributed Architectures

##### Monolithic

- whole systm is bundled into _one_ deployable _unit_
- __Pro__: *simple*
- __Con__: Single point of _failure_, _limited_ foom for _scaling_

##### Distributed

- sytem is deployed into _multiple independent units_ communicating over a _network_
- **Pro**: _scalability_, _elasticity_, _robustness_ and _availability_
- **Con**: _complexity_, more challanging tasks (logging, transactions, versioning, ...), dependant on _network_ and security

#### Client-Server Architecture

==Client <-> Network <-> Service==

- servers profice services
- clients call upon these services
- a network allows clients to access servers

#### Thin- and Fat-Client Architecture

- thin: _client_ is simply responsible for _UI_, server does everything else
- fat: _server_ is only responsible for _data management_, _client_ does _application logic_ **and** _UI_

#### 3-Tier Architecture

==Presentation Tier <-> Application Tier <-> Data Tier==

#### Distributed Systems

- **Client-Servier architectures**
	- _servers_ provide _services_ are treated differentely from _clients_
- **Multi-tier architectures**
	- clear _seperation_ of responsibilities between tiers
	- typically _client-server_ relationships
- **Peer-to-peer architectures** (like in gaming sometimes)
	- system consists of _interconnected_ _peers_ of similar capabilities and responsibilites, where peers can act _as both servers and clients_
	- _self-organizing_ and scale well with the number of participating peers

#### Modern Distributed Architectures

- Service-Oriented Architectures (__SOA__)
- Microservice Architectures
- Cloud-native Architectures

#### Reference Architectures

- _templates_ (instantiation of a style, more concrete)

### 1.3 Architectural Patterns

>[!idea]
>_Recurring solutions_ for _recurring architectural problems_
> - Guideline to solve problem → Adaptation to own purpose needed!

#### Model View Seperation Principle

- **MVC Pattern** means seperation of concerns
	- **Smart-Ui-Anti Pattern** means having application logic in the UI

![[Pasted image 20240627152718.png]]

### 1.4 Architectural Design Decisions

>[!help] Common decisions
>- generic architecture?
>- documentation?
>- which components can/must be bought?
>- ...
>
>Common **reference architectures** can simplify answering these questions
as well as the usage of proven architectural patterns.

> [!CHECK] Advantages of Explicit Architecture
> - Stakeholder communication
> - System analysis
> - Large-scale reuse
> - project planning

> [!caution] Impact on
> - Performance
> - Security
> - Safety
> - Availability
> - Maintainability
> - Scalabilty

#### Tradeoffs

>[! attention] when goals conflict which each other
>- High usability vs. high security
>- High availabilty and faster response time vs. high data intergrity
>- HIgh modifiablity vs. high efficiency
>  
>→ _Prioritize_ some quality metrics over others! 

### 1.5 Software Metrics

> [!hint] Main idea
> -  See how the system is growing
>-  Compare architectural designs
>-  _Evaluate_ design quality
>-  Estimate maintainability

#### Metrics

##### Static code analysis

- cyclomatic complexity
- complexity
- instabillity and abstractness
- Cohesion and Coupling (LCOM4)
	- **afferent** (ingoing coupling)
	- **efferent** (outgoining)
	![[Pasted image 20240421135420.png]]

##### Execution time analysis

- _Big O_ Notation
- Worst-case execution time

##### Dynamic analysis (monitoring)

- code coverage
- _response time_ 
- _CPU usage_
- _RAM usage_
- availabilty
