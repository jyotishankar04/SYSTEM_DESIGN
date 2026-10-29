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

