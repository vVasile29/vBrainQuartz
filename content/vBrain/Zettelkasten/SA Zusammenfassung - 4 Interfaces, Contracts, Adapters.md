
## Interface Models

### Requirements

- interoperability checks
- adaptability
- ease of retrieval

### Signature-List-Based Interfaces

==like java interfaces ==

```Java
providesInterface MemoryMgr {
	int init (int, int);
	void release();
	int read(int);
	void write(int);
	void layout1();
	void layout2();
	void free();
}
```

- importance of naming conventions
- no behavioral information given by signature

-> meaning of services? correct ordering (protocol)? Alternative usages?

### Protocol Modeling Interfaces

> [!NOTE]
> A _component protocol_ is a set of call sequences (ordered list or method calls)
> - the provides protocol -> accepted call sequences
> - the requires protocol -> called external methods

-> however, usually infinite sets of call sequences, so notation should have decidable inclusion problem for checking

![[Pasted image 20240530214823.png]]

### Quality-of-Service (QoS)

secondary material...

## Interface Design Guidelines

> [!NOTE]
> Good interfaces _reduce costs_.
> Good interfaces _foster software sustainability_.

- basic prerequisite for creating high-quality designs
	- component types are all described just by their interfaces
- are an abstraction to hide details
	- information hiding
	- consitent level of abstraction
	- adequate abstraction

### Golden Rules of Interface Design

- take both into account:
	1. Component developer
	2. Component user
- adding functionality != extending interface
	- decorator pattern
	- adapter pattern

### Design for Encapsulation/Reusability

- Design should not reflect internal representation

`void addCustomers(List customerList)` -> use of `List` as data type for `Customer` collection... reuse of component should be possible without List-Implementation

### Command / Query-Seperation

A component service can either
- inspect/query the component state (getters)
	- OR - never both!
- transform the state of the component
-> several _calls_ to a query method should return the _same result_! (_like in functional programming_!)

```Java
public class Missile {
	...
	public String getState() {
		launch(); // this changes the state in some way
		return “launched”;
	}
}
```

### Service Naming and CRUD Operations

- Create, Read, Update, Delete

### Interface Design Guidelines

- Define semantics or make interfaces independent
	- Interfaces consist of two parts:
		- programmatic syntax: `methodA()`, `methodB()`
		- semantcis: protocols like first call `mA()`, then `mB()`
	1. semantics should be made explicit and documented
		=> use asserts or exceptions
	2. make interfaces independent form implicit semantics as much as possible
- don't make assumptions about interface's use
- favor read-time convenience over write-time convenience
	- code is ead much more often than written
- Don't put a method into an interface whose abstraction doesn't fit
	- just because a client needs that method...
		- rather write a new interface that fits
	- that is the _interface segregation principle_!
 
## Component Categories

- help structuring large applications

1. inventory - data management and access
	- like repository
	- typical CRUD
	- views on and care for data integrity
3. functionality - support of business services
	- like domain/service
	- access only inventory components and functionality components
3. processes - business processes
	- business logic
4. interaction - with users or other systems (frontend)
	- user access
	- may exist in parallel (mobile, diverse clients...)

#### parallels:

Inventory - Infrastructure (Database) Layer
Functionality - Domain Layer
Application - Processes (Business Logic) Layer
Interaction - UI Layer (frontend)

## Design-by-Contract

### V-Modell

![[Pasted image 20240530222024.png]]

### Contracts

![[Pasted image 20240530222157.png]]

### Contracts for Methods

`CustomerRecord getCustomerRecord (ID customerID)`

_Precondition_
A database is attached and customer c with c.ID == customerID
is unique or does not exist in this attached database

_Postcondition_
If customer with this ID exists in the attached database, then it is
returned, otherwise null is returned

... also:
- "implement in language x"
- assert-keyword for simulating contracts

### Benefits of Contracts

- less error prone software
	- better docs
	- reduced error handling code
	- clear responsibilities
	- results of run-time checking

### Contractual use of Components

- component guarantees provided services
- precondition
	- required services
	- information concerning them
- postcondition
	- provided services
	- information concerning them
- invariant
	- relation between provided and requierd services

=> adaptability of components and their interfaces is essential!

## Adaptation Mechanisms

![[Pasted image 20240530222949.png]]