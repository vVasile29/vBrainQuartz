
## Cloud Computing


> [!info] Definition
> On-demand provisioning of data center resources over the internet
> → SaaS + Utility Computing ("Pay-as-you-go")

- On-demand self-serice
- broad network access
- resource pooling (shared pool)
- rapid elasticity
- measured service

![[Pasted image 20240722134905.png]]

> [!tip] Elasticity
> The degree to which a system is able to adapt to workload changes by provisioning/deprovisioning resoures in an automatic manner such that at each point in time: ==available resources = current demand==

### Cloud-Native Architecture

- modern software applications are often microservices developed based on DevOps Paradigm and deployed in cloud environments
	- DevOps with CI/CD Processes
	- API-driven, Service-based architectures
	- Container-based infrastructure

## Serverless Computing

> [!info] Definition
> - **Develop and run cloud applications without requiring server management**
> - Servers are still included...

> [!success] Advantages
> - No server management required
> - Built-in scalability
> - Increased Dev Speed & Resource Efficiency
> - Saves Costs
> - e.g. AWS Lambda

### Function as a Service (FaaS)

> [!NOTE]
>   - Small → runtime (< 5s) (Good Resource Management)
>   - Stateless → independent
>   - Event-driven → Triggered and executed asyncronously (no blocking)

- Function is a unit of composition
- Possible triggers: API, file upload, DB entry, pub/sub
- Workflow: Composition of functions to achieve complex functionality
	- Wait for Result
	- Retry on Failure
	- Failure Management
    - Parallelization
    - Conditional Logic
	→ Fosters function reuse

#### Cold Start Problem

- Start-up of container takes a lot of time due to installation of packages
- Data shipping Architecture (a lot of data moving involved)
- Functions are not network accessible
- No open source → vendor-driven (AWS)

### Backend as a Service (BaaS)

- Offer backend services (e.g., data storage, authentication, messaging) on a pay-per-use basis
- Provider takes care of deployment, scaling... → billing per usage
- Requirements
	- Predictable Performance (for users)
	- Elastic Scaling
	- High Availability
- Examples
	- AWS Aurora (Relational DB, vertical scaling, no elastic scaling)
	- DynamoDB (NoSQL DB)
	- Firestore (Document DB)

> [!success] Advantages
>   - Reduces Dev. time
>   - Hosting & Scaling managed by BaaS provider (e.g., Firebase)

>[!warning] Disadvantages
>
>- Potential vendor lock-in
>- Limited customization options

| IT         | Car Analogy                               |
| ---------- | ----------------------------------------- |
| Serverful  | Car Rental (pay for rental time, driving) |
| Serverless | Taxi (pay for usage, no driving)          |
| On-premise | Buy a car                                 |

> [!info] Further Info on Serverless Computing
> - Cloud computing paradigm to dev/deploy/run apps.
> - No need to allocate and manage virtualized resources. (NoOps)
> - Provider does Provisioning, Scaling, Infrastructure
> - Utilization-based billing.

### Container as a Service (CaaS)

> [!NOTE]
> - Cloud service model that allows users to deploy & manage containers in the cloud.
> 	- AWS Elastic Kubernetes Service
> 	- Google Kubernetes Engine

> [!question] How is this different from Platform-as-a-Service? Isn't that also Serverless? 
> Depends on level of abstraction/automation.
> → PaaS like early versions of Microsoft Azure had serverless elements but did not completely abstract servers and operational aspects

- Evolution towards Serverless Computing (increasing degree of abstraction)
	- Low-Level VM Interfaces → High-Level Interfaces
	- Explicit allocation of resources → Automatic Allocation
	- Reservation-based billing → Utilization-based pay-per-use
	- Cloud users responsible (for technical stuff) → Provider responsible

### NoOps in Practice

1. Upload code.
2. Setup events to trigger code execution.
3. On-demand execution with continuous scaling.
4. Pay for use time (sub-second metric).

- Resource Sizing: How much CPU, RAM, I/O Bandwidth are allocated to a worker instance.
	- usually implemented as a "memory size" parameter, others scaled accordingly
	- determining optimal size of serverless functions is important but challenging
