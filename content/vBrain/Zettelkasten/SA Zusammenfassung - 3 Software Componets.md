
> [!tldr]-
> 
> - **Component Definition**:
>     
>     - Independent, deployable unit.
>     - Usable without knowing internal structure.
> - **Key Characteristics**:
>     
>     - Provided and required interfaces.
>     - Follows black-box principle.
>     - Self-contained and adheres to component model.
> - **Benefits**:
>     
>     - Faster development and easier maintenance.
>     - Reduced redundancy through code reuse.
>     - Encapsulation: isolates changes to one module.
> - **Componentization vs. Object-Orientation**:
>     
>     - Prefers composition over inheritance for encapsulation.
> - **Lifecycle**:
>     
>     - Specified, implemented, deployed, and run.
>     - Instantiation via loading or deployment, not like classes.
> - **Modules vs. Components**:
>     
>     - Modules hide design decisions; components are building blocks.
> - **UML 2 Modeling**:
>     
>     - Important diagrams: Structure and Behavior.

> [!info] Definition
> - _unit of composition_
> - can be deployed independently
> - _building block_ for software that can be readily _used by third parties_ without understanding its internal structure

> [!NOTE]
> - has provided (and required) _interfaces_
> - hides its implementation (_back-box_ principle)
> - is _self-contained_
> - adheres to component model

### notation

![[Pasted image 20240516094259.png]]

### Components vs. Layers

- components are _feature-oriented_, they deliver well defined chunk of functionality

## Properties

### Encapsulation

- __information hiding principle__: specifics should not be changed if design decision changes

> [!success] Benefits
> - comprehensibility: focus on connections and recursive decomposition
> - faster development: through code reuse and parallelization
> - less redundancy: common code factored out as a module
> - mainainabliilty: change of design decision ideally affects one one module

### Object-Orientation != Componentisation?

- inheritance is conflicting with the black-box-principle of components

![[Pasted image 20240516094952.png|500]]

### Black-Box principle

> [!NOTE]
> black-box doesn't mean no information on internals at all!

![[Pasted image 20240516095226.png|500]]

### Connectors

- connection should be doable without coding, only configuration at deployment time, provided by component framework
- if mismatch, then adaptation (glue coding) becomes necessary

![[Pasted image 20240516095355.png]]

### 1st class vs. 2nd class entities

![[Pasted image 20240516095510.png|500]]

### Assembly/Composition

components can be bundled into one on the connector

![[Pasted image 20240516095735.png]]

> [!success] Benefits of Components
> - encapsulation
> - black-box reuse
> - better quality, time, costs
> - engineering approach to software development

> [!info] Properties of Components (Szyperski)
> - unit of _independent_ deployment
> - unit of _third party_ composition
> - no persistent state

### Technical Realization of Components

__Java__: Java Beans, POJOs (most approaches are rather object-oriented than component-based)

## Component lifecycle

1. component specified
2. component implemented
3. component installed/deployed
4. component running
	- code loaded in main memory and ready to be executed
	- attention: in some component definitions components can have state
	- question: how to realize stae in stateless component models?

### Component instantiation

- components are not instantiable like classes/objects
	- loading of the component code in memory and its preperation for running can be seen as another form of instantiation
	- no manual instantiation at run time
- implementation or deployment is a form of instantiation
	- abstract specificatioon = specific instance

### Deployment

![[Pasted image 20240516102000.png]]

### Composition over inheritance

- inheritance often requires deep insight into the internals of the superclass
	- white-box reuse -> inheritance breaks encapsulation
- composition only requires that contained classes have well-defined interfaces
	- black-box reuse
- See State and Strategy Design pattern

### Classes vs. Components

![[Pasted image 20240516102424.png|500]]

### Modules

> [!info]
> - encapsulates design decision
> - can only be accessed via interfaces
> - interfaces should provide sufficient information for using the module

### Modules vs. Components

- elements for hierarchical system decompositionn
- used through interface
- often only one instance available

#### Differences

- different goals: 
	- components are building blocks
	- modules hide design decisions

## Modeling components with UML 2

> [!NOTE] Most important diagrams, best learned by excercises 
> - Structure Diagrams
> 	- Repository Diagram
> 	- Component Diagram
> -  Behavior Diagrams
> 	- Activity Diagram
> -  Interaction Diagrams
