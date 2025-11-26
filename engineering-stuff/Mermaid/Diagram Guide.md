 A practical reference for creating all major architecture diagrams in Mermaid, including conventions, syntax patterns, common mistakes, and advanced techniques.

---

# 1. General Mermaid Conventions

## Naming conventions
- PascalCase for components  
- UPPER_SNAKE_CASE for tables  
- Short labels for sequence diagram participants  
- Meaningful IDs only (avoid spaces)

## Layout conventions
- graph LR (left-to-right) for system, container, integration  
- flowchart TD (top-down) for workflows  
- Use subgraph for grouping  
- Keep labels short but clear  

## Style conventions
- Boxes = components  
- Cylinders = databases  
- Rounded shapes = boundaries  
- Solid arrows = sync  
- Dashed arrows = async  

---

# 2. System Context Diagrams  
**Type:** C4 Level 1

## Minimal pattern
```
graph LR
    User[(User)]
    System[Insurance Platform]
    CRM((CRM System))
    Payments((Payment Gateway))

    User --> System
    System --> CRM
    System --> Payments
```

## Advanced: Grouping and boundaries
```
graph LR
    subgraph External
        CRM((CRM))
        Email((EmailService))
    end

    subgraph Platform
        API[Core API]
    end

    User --> API
    API --> CRM
    API --> Email
```

---

# 3. Container Diagrams  
**Type:** C4 Level 2

## Minimal pattern
```
graph LR
    Browser[Web Browser]
    Frontend[SPA]
    API[(Backend API)]
    Worker[Worker]
    DB[(Postgres)]
    Queue((Queue))

    Browser --> Frontend
    Frontend --> API
    API --> DB
    API --> Queue
    Queue --> Worker
    Worker --> DB
```

## Advanced: Deployment grouping
```
graph LR
    subgraph Cloud["AWS"]
        API[(API)]
        Queue((SQS))
    end

    Browser[Browser] --> API
    API --> Queue
```

---

# 4. Sequence Diagrams  
**Type:** Behaviour workflow  
**Syntax:** sequenceDiagram

## Minimal pattern
```
sequenceDiagram
    participant U as User
    participant FE as Frontend
    participant API as Backend
    participant DB as Database

    U ->> FE: Click Login
    FE ->> API: POST /login
    API ->> DB: Validate
    DB -->> API: User record
    API -->> FE: Token
    FE -->> U: Render dashboard
```

## Advanced: Activation, opt blocks, loops
```
sequenceDiagram
    API->>DB: Query
    activate DB
    DB-->>API: Result
    deactivate DB

    opt Invalid token
        API -->> FE: 401
    end

    loop Retry 3 times
        FE ->> API: Request
    end
```

---

# 5. Activity / Flow Diagrams  
**Type:** Workflow  
**Syntax:** flowchart TD

## Minimal pattern
```
flowchart TD
    A[Start] --> B[Collect Data]
    B --> C{Valid?}
    C -->|No| D[Reject]
    C -->|Yes| E[Calculate]
    E --> F[Finish]
```

## Advanced: Parallel branches
```
flowchart TD
    A --> B1 & B2
    B1 --> C
    B2 --> C
```

---

# 6. State Machine Diagrams  
**Type:** Entity lifecycle  
**Syntax:** stateDiagram-v2

## Minimal pattern
```
stateDiagram-v2
    [*] --> Draft
    Draft --> Submitted
    Submitted --> Approved
    Submitted --> Declined
    Approved --> Active
```

## Advanced: Composite states
```
stateDiagram-v2
    state Review {
        [*] --> Manual
        Manual --> Auto
    }
```

---

# 7. ER Diagrams  
**Type:** Data model  
**Syntax:** erDiagram

## Minimal pattern
```
erDiagram
    USER {
        UUID id
        string email
        date created
    }

    ORDER {
        UUID id
        UUID user_id
        decimal total
    }

    USER ||--o{ ORDER : places
```

## Advanced: constraints
```
erDiagram
    USER {
        UUID id PK
        string email UNIQUE
    }
```

---

# 8. Class / Domain Model Diagrams  
**Type:** OOP or DDD  
**Syntax:** classDiagram

## Minimal pattern
```
classDiagram
    class Customer {
        +UUID id
        +string name
        +createOrder()
    }

    class Order {
        +UUID id
        +decimal amount
        +submit()
    }

    Customer "1" --> "0..*" Order
```

## Advanced: abstract + interfaces
```
classDiagram
    class Payment {
        <<Abstract>>
        +process()
    }

    class INotifier {
        <<interface>>
        +notify()
    }
```

---

# 9. Deployment / Infrastructure Diagrams  
**Type:** Architecture & ops  
**Syntax:** graph LR

## Minimal pattern
```
graph LR
    User --> LB[Load Balancer]
    LB --> App1[App Server]
    App1 --> DB[(Postgres)]
```

## Advanced: Network zones
```
graph LR
    subgraph VPC
        LB
        App1
        App2
    end

    App1 --> DB[(RDS)]
```

---

# 10. Integration Diagrams  
**Syntax:** graph TD

## Minimal pattern
```
graph TD
    Client --> API[(Core API)]
    API --> CRM((CRM))
    API --> Billing((Billing))
```

## Advanced: Webhooks
```
graph TD
    Billing -->> API: webhook event
```

---

# 11. Mermaid Debugging Tips

- Avoid spaces in IDs  
- Avoid trailing commas  
- Make sure each arrow points to a valid ID  
- ER diagrams break if you forget a closing relationship  
- Obsidian requires blank lines around code blocks  
- Class diagrams must use spaces, not tabs  

---

# 12. Quick Templates (Copy/Paste)

### Sequence template
```
sequenceDiagram
    participant A
    participant B
    A ->> B: Request
    B -->> A: Response
```

### Flow template
```
flowchart TD
    A --> B
    B --> C{Decision}
    C -->|Yes| D
    C -->|No| E
```

### State template
```
stateDiagram-v2
    [*] --> A
    A --> B: event
```

### ER template
```
erDiagram
    T1 ||--o{ T2 : relation
```

### Class template
```
classDiagram
    class A {
        +method()
    }
```

---
