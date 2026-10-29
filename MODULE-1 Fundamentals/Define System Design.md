System design is the process of defining the architecture, modules, interfaces and data for the software system to satisfy the specified requirements 

It acts as the bridge between HLD and LLD

It is the process of deciding how different software components should be organized, communicate, and work together to meet a set of requirements.

It involves:
- What components the system needs.
- What responsibility each component has.
- How components communicate with one another.
- How data is stored, processed and retrieved.
- How the system meets requirements such as performance, reliability, security, and scalability.



## Why we need System design?

A software application is not just about code. A backend application might contain an server, database, cache, and background workers.
Without proper design these components might not work together efficiently.


System design helps us answer questions such as:
- How should requests flow through the application?    
- Which component should be responsible for each operation?    
- Where should data be stored?    
- What happens if a component fails?    
- How can the system handle increasing traffic?    
- What trade-offs should we make between performance, cost, and complexity?


## System Design Fundamentals
---
### Architecture, Components, Interfaces, Implementation

These four concepts describe different parts of how we design and build a software system. Let's understand them one by one using a backend application.

### 1. Architecture
Software architecture is the high level structure of a system, including its major components, their responsibilities, and the relationships between them.

Architecture answers questions such as:
- What are the major parts of the system?    
- How are those parts organized?
- How do they communicate?
- Where are the system's boundaries?

![[SIMPLE-CLIENT-SERVER-ARCH.png|700]]

This diagram represents a simple architectural structure. It does not specify the exact classes, functions, or SQL queries.

### 2. Components
A component is a distinct part of a software system that performs a specific responsibility and interacts with other parts.

Components can exist at different levels of detail.


| COMPONENTS            | RESPOSIBILITIES                           |
| --------------------- | ----------------------------------------- |
| API Server            | Receives and processes HTTP requests      |
| Authentication module | Validates user identity and access        |
| Database              | Stores persistent data                    |
| Cache                 | Stores selected data for faster retrieval |
| Background worker     | Processes asynchronous jobs               |

For example, in a Node.js backend, you might separate the URL-shortening functionality into thesecomponents:
- `UrlController` — handles HTTP requests and responses.
- `UrlService` — contains URL-shortening business logic.
- `UrlRepository` — handles database operations.
- PostgreSQL — persists URL mappings.

Key point: Components divide a system into manageable parts. Good component boundaries improve maintainability and make changes easier to isolate.

## 3. Interfaces
An interface defines how one component can interact with another. It specifies the available operations and the expected inputs and outputs without requiring the caller to know the internal implementation.

Interfaces may take several forms:

- **API interface:** An HTTP endpoint and its request/response contract.
- **Code interface:** A TypeScript interface or function contract.
- **Service interface:** An agreed contract between independently deployed services.
- **Database interface:** The operations a component uses to access stored data.

## 4. Implementation
Implementation is the process of translating a design into working software using code, configurations, database schemas, and other technical artifacts.

Implementation answers questions such as:
- Which functions and classes will we write?
- How will requests be validated?
- Which SQL statements will execute?
- How will errors be handled?
- How will the application be configured and deployed?


## High-Level Design (HLD) vs Low-Level Design (LLD)
---

### 1. High-Level Design (HLD)
High-Level Design describes the overall architecture of a system, its major components, their relationships, and how data flows between them.

HLD answers questions such as:
- What are the main components?
- How do they communicate?
- Which database or storage system is needed?
- How will requests flow through the system?
- What architecture is appropriate for the requirements?

Consider a URL-shortening service:
![[SIMPLE-CLIENT-SERVER-ARCH.png|700]]

This is HLD because it shows the main components and their relationships without specifying their internal code.


### 2. Low-Level Design (LLD)
Low-Level Design describes the internal structure and behavior of individual components, including classes, methods, interfaces, data models, and detailed logic.

LLD answers questions such as:
- What classes and methods are needed?
- What are the responsibilities of each class?
- What parameters does a method accept?
- What does a database model look like?
- How are validation and errors handled?

For the same URL-shortening service, an LLD might define these classes:

```Typescript 
class UrlController {
  // Handles HTTP requests and responses
}

class UrlService {
  // Generates short codes and applies business rules
}

class UrlRepository {
  // Reads and writes URL mappings
}
```

It might also define the database model:
``` Prisma
model Url {
  id          String   @id @default(cuid())
  shortCode   String   @unique
  originalUrl String
  createdAt   DateTime @default(now())
}
```


## 3. Key differences

|Aspect|HLD|LLD|
|---|---|---|
|Focus|Overall system architecture|Internal component design|
|Main elements|Services, databases, caches, queues|Classes, methods, interfaces, schemas|
|Primary question|How is the system organized?|How does each component work internally?|
|Diagrams|Architecture and data-flow diagrams|Class, sequence, and detailed interaction diagrams|
|Typical decisions|Service boundaries, communication patterns|Method signatures, class responsibilities, validation logic|


***Important:** HLD and LLD are complementary, not competing approaches. HLD establishes the overall structure, while LLD adds detail to individual components. In practice, the process is iterative: detailed design can reveal the need to revise the architecture.*


### Why do we need both HLD and LLD?

- HLD tells us how the overall system is organized.
- LLD tells us how individual components are structured and implemented.

For example, when designing a URL shortener:
- HLD decides that the system has a backend API and PostgreSQL database.
- LLD defines the `UrlService` methods, repository interface, and database model.
![[System Design Fundamentals Cheat Sheet.png]]


## Functional vs Non-Functional Requirements
--- 

### 1. Functional Requirements

A functional requirement describes a specific behavior, feature, or operation that a software system must provide.

For example, suppose you are building a URL-shortening service. Its functional requirements might include:
- Users can submit a long URL.
- The system generates a unique short code.
- Users can access the original URL using the short URL.
- Users can delete their shortened URLs.

Each requirement describes something the system must be able to do.

#### How to identify a functional requirement
Look for an action the system must perform.

Common wording includes:
- The system shall create …
- The system must validate …
- The user can view …
- The API must return …
- The system allows users to update …

A good functional requirement should describe a behavior clearly enough that a developer can implement it and a tester can verify it.


#### Technical example: URL shortener
Consider this requirement:

FR-1: The system shall generate a unique short code when a valid long URL is submitted.
Let's break it down:

| Part            | Meaning                                                                      |
| --------------- | ---------------------------------------------------------------------------- |
| Trigger         | A client submits a valid URL                                                 |
| System behavior | Generate a short code                                                        |
| Expected result | Return a short URL containing that code                                      |
| Verification    | Submit a valid URL and check that a short URL is returned with a unique code |

The requirement describes what the system must do. It does not yet specify how fast the operation must finish or which algorithm must generate the code.

#### Request flow
![[URL-SHORTNER-FR1.png]]

#### Common mistake

Consider these two statements:
- A. The system shall allow users to log in using their email and password.
- B. The system shall support 5,000 login requests per second.

Statement A describes a feature: logging in. It is a functional requirement.

Statement B specifies a performance target. It is a non-functional requirement.

We’ll examine non-functional requirements in the next section, without mixing the two concepts.


## 2. Non-Functional Requirements (NFRs)

A non-functional requirement describes a quality, constraint, or measurable target that specifies how well a system must operate, rather than a feature it must provide.

It answers questions such as:
- How fast must the system respond?
- How much traffic must it handle?
- How reliable must it be?
- How secure must it be?
- How much downtime is acceptable?

For example, in your student learning platform:

- Functional requirement: Students can create and save notes.
- Non-functional requirement: 95% of note-saving requests must complete within 200 milliseconds under a specified workload.

The first defines a capability. The second defines a performance expectation for that capability.


### Common types of functional requirements
- **Performance**:
	- Defines the response time, latency, and throughput expectations.
	- EX: 90% of API requests complete within 200ms.
- **Scalability**:
	- Describes how the system should handle growth in users, traffic, or data.
	- Example: The service should handle increased traffic by adding application instances.
- **Security**: 
	- Defines requirements for protecting data and controlling access.
	- Example: Only the owner of a note can read or modify it.
- **Availability and reliability**
	- Availability concerns whether the system is operational.
	- reliability concerns correct.
	- Example: Target 99.9% service availability.
- **Maintainability**:
	- Describes how easily the software can be understood, modified, tested, and maintained.
	- Example: Modules should have clearly defined responsibilities and automated tests.

These are common categories, not an exhaustive list. A requirement can also involve compatibility, usability, data durability, or regulatory constraints.

### 3. How to write a useful NFR
Avoid vague statements such as “the API should be fast.”

Prefer a requirement that can be measured or verified:

> Under a workload of 1,000 requests per second, 95% of successful API requests should complete within 200 m

This is more useful because it specifies:

- Workload: 1,000 requests per second.
- Metric: Response latency.
- Threshold: 200 ms.
- Percentile: 95th percentile.
![[System Design Requirements Cheat Sheet.png]]
## Constraints
A constraint is a limitation or restriction that the system design must satisfy.

Constraints limit which solutions are acceptable.

For example, when designing a backend service, you might be required to:

- Use PostgreSQL as the database.
- Keep infrastructure costs below ₹10,000 per month.
- Deploy the application on a specified cloud provider.
- Meet a regulatory requirement for storing user data.
- Complete the project within four weeks.

## Types of constraints

|Type|Meaning| Example                                                                     |
|---|---|---|
|Technical|Restricts technologies or technical approaches| The backend must use Node.js and PostgreSQL.                                |
|Budget|Limits spending| Monthly infrastructure cost must not exceed ₹10,000.                        |
|Time|Limits development or delivery time| The MVP must be delivered in four weeks.                                    |
|Regulatory|Requires compliance with applicable rules| Personal data must be handled according to applicable privacy requirements. |
|Infrastructure|Restricts the deployment environment| The system must run on the company's existing servers.                      

## Technical example: URL-shortening service

Suppose you're designing a URL shortener with these requirements:

- It must use PostgreSQL.
- It must run on one application server.
- The monthly infrastructure budget is limited to ₹5,000.

These are constraints because they restrict your available choices.

Constraint 1
Database: PostgreSQL is mandatory.

Constraint 2
Deployment: Only one application server is permitted.

Constraint 3
Budget: Infrastructure spending must stay within ₹5,000 per month.

Design implications
- You cannot freely choose another database.
- You cannot rely on multiple application servers for horizontal scaling.
- You must consider resource usage and infrastructure costs.

Notice that a constraint does not necessarily tell you the best design. It defines the boundaries within which you must find a suitable design.

For example, one application server may be sufficient for a small workload, but it can become a reliability limitation if that server fails.

## Important distinction: Constraint vs requirement

A requirement describes something the system must do or achieve. A constraint limits how the system can be designed or operated.

|Statement|Classification|
|---|---|
|Users must be able to create short URLs.|Functional requirement|
|95% of requests must complete within 200 ms.|Non-functional requirement|
|PostgreSQL must be used.|Technical constraint|
|The system must be delivered within four weeks.|Time constraint


## Assumptions
An assumption is something we treat as true for the purpose of designing a system, even though it has not been fully confirmed.

Assumptions are useful because system design often begins with incomplete information.

For example, suppose you're designing a URL-shortening service, but the interviewer hasn't specified the expected traffic.

You might state:

> “For this initial design, I'll assume the system receives 10,000 URL-creation requests per day.”

This gives you a starting point for estimating workload and choosing an architecture.

However, the number is an assumption—not a confirmed fact. If the actual traffic is much higher, some design decisions may need to change.


## Why do we need assumptions?

Assumptions help us:

1. Proceed when information is incomplete.
2. Make our reasoning explicit.
3. Estimate system requirements and resource needs.
4. Identify which decisions depend on uncertain information.
5. Recognize when a design needs revision.

## Technical example

Imagine you're designing a note-taking backend, but you don't know how many users will use it.

You state these assumptions:

|Assumption|Why it matters|
|---|---|
|Initially, there will be 5,000 registered users.|Helps estimate storage needs.|
|At most 500 users will be active concurrently.|Helps estimate application capacity.|
|Most notes will be text-based.|Helps estimate average storage per note.|

## Constraint vs assumption

This distinction is important.

|Constraint|Assumption|
|---|---|
|A restriction the design must obey.|An unconfirmed condition accepted for planning.|
|“We must use PostgreSQL.”|“We expect 500 concurrent users.”|
|Usually defines a boundary.|Helps fill gaps in available information.|

An assumption can later be confirmed or disproved. A constraint generally remains binding unless the relevant stakeholder changes it.


## Trade-offs
A trade-off is a design decision in which improving one aspect of a system involves sacrificing or complicating another aspect.

In system design, we rarely maximize every quality at once. We often balance:

- Performance vs cost
- Availability vs consistency
- Simplicity vs flexibility
- Security vs convenience
- Storage space vs query speed

A trade-off doesn't necessarily mean one choice is wrong. It means we choose the option that best fits the system's requirements and constraints.


## Technical example: Database indexing

Suppose your note-taking application frequently retrieves notes by `userId`.

You can create a database index on that column.

```SQL
CREATE INDEX idx_notes_user_id
ON notes(user_id);
```

An index can make queries that filter by `user_id` faster, but it also consumes additional storage and adds work when rows are inserted, updated, or deleted.


## Another backend example: Caching

Suppose your API repeatedly reads the same note metadata from PostgreSQL.

Adding Redis caching may reduce database load and improve read latency. However, it introduces extra infrastructure and a cache-invalidation problem: how do we keep cached data correct when the underlying note changes?

|Choice|Benefit|Cost or limitation|
|---|---|---|
|Read directly from PostgreSQL|Simpler architecture and fewer components|More database reads|
|Add Redis caching|Potentially lower read latency and database load|More complexity and possible stale data|

Neither option is universally better. The correct choice depends on the requirements and workload.

## How constraints, assumptions, and trade-offs work together

Consider our URL-shortening service:

- Constraint: The system must use PostgreSQL.
- Assumption: We expect 10,000 URL-creation requests per day.
- Trade-off: We may choose to keep the initial architecture simple rather than introduce a distributed caching layer before there is evidence that it is needed.
![[System Design Study Sheet_ Constraints, Assumptions & Trade-offs.png]]

![[System Design Trade-Offs Cheat Sheet.png]]