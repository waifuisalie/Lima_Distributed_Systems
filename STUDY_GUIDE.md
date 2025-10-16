# Distributed Systems Test - Study Guide

## 1. Fundamental Concepts

### Socket Communication (Lab 01 - NetCat)
**Key Question from Lab**: "What information is necessary to establish a connection between a client and server using sockets?"

**Answer**: **4 crucial values**:
1. Client IP address
2. Client port number
3. Server IP address
4. Server port number

**Important Concepts**:
- **Pipes**: Foundation for understanding sockets (data flow between processes)
- **Protocols**: Critical because systems send BYTES - the receiver doesn't know when a message is finished
- **Protocol standardization**: Sockets predetermine protocol (e.g., little-endian vs big-endian)
- **Ubiquity**: Sockets are fundamental to everything (CORBA, WebServices all use sockets underneath)

**Lab Activities**:
- Creating TCP servers and clients with NetCat
- Implementing chat systems
- Generic servers (date, file listing, echo)
- Remote control systems
- Persistent servers using loops
- Communication redirection/proxying

---

## 2. What are Distributed Systems? (Intro)

### Definition - Lamport Quote
> "A distributed system is a system in which a failure in a computer that you didn't even know existed can make your own computer unusable."
> — Leslie Lamport

### Formal Definition (Coulouris et al.)
> "A system in which hardware and/or software components located on networked computers communicate and coordinate their actions solely through message passing."

### Key Characteristics
- **Entities (nodes) communicate via message passing**
- Each entity "reacts" when a message arrives
- Each has its own notion of time (no global clock)
- Must integrate into a "group" (open or closed)
- Form overlay networks
- Equipped with list of other processes they can communicate with directly

### Why Study Distributed Systems?

**21st Century Reality**:
- Dependence on networked services
- Web search, video conferencing, stock market
- Internet banking, online shopping, social networks
- Games, cloud services

**Programs exchange data via network infrastructure** = Distributed Systems

### Critical Challenges for Distributed Systems

**Must Handle**:
- **Scalability**: System must grow efficiently
- **Failure handling**: Components will fail
- **Concurrency**: Multiple users/processes simultaneously
- **Transparency**: Hide complexity from users
- **QoS** (Quality of Service): Performance guarantees
- **Mobility**: Devices and services move
- **Adaptability**: Context-sensitive behavior
- **Security**: Protection from attacks

**Key Requirement**: **Interoperability** between heterogeneous components

### Heterogeneity Issues

**No Consensus On**:
- Hardware platforms
- Programming languages
- Operating systems
- Network protocols

**BUT**: There MUST be consensus on **interfaces and models for interoperability**

This is why we have middleware!

### Component-Based Architecture

**Evolution**:
- From monolithic applications
- To component-based architectures

**Components** = Computational "objects"
**Applications** = Collections of cooperating software components

**Benefits of Component Architecture**:
- Distribution
- Update/upgrading capability
- Maintenance
- Reuse
- Flexibility (reconfiguration)
- Availability
- Fault tolerance

---

## 3. Distributed Systems Design Aspects (SDC_02)

### Transparency
**Goal**: Provide users with a single, abstract view of the computational system

**Types**:
- **Location**: Users don't need to know resource locations
- **Migration**: Resources can move without changing names/references
- **Replication**: Users don't know how many copies exist
- **Concurrency**: Multiple users can share resources without conflicts
- **Parallelism**: Activities can occur in parallel without explicit user action

### Flexibility
- Easy insertion of new entities/services into the system

### Reliability
- **Theory**: If one machine fails, another can take over (reliability)
- **Practice**: Some components are more critical - if they fail, whole system may fail

**Aspects**:
- **Availability**: Fraction of time system is available
- **Security**: Protection from unauthorized access
- **Fault tolerance**: Ability to continue operating despite failures

**Techniques**:
- Hardware redundancy (processors, disks, memory, links)
- Software redundancy (multiple programs doing same function)

---

## 3. Concurrency in Distributed Systems (SDC_03)

### Parallel Programming vs Distributed Programming

| Aspect | Parallel Programming | Distributed Programming |
|--------|---------------------|------------------------|
| **Objectives** | Performance (beyond single processor) | Convenience (availability, reliability) + performance (secondary) |
| **Interaction** | Frequent, high granularity, low overhead, reliable | Less frequent, large data volumes, unreliable |

### Types of Concurrency

#### 1. Interprocess Concurrency (Less important for test)
- Multiple threads per connection → potential race conditions
- Solutions: semaphores, mutex, monitors
- Example: Server creating new thread for each client connection

#### 2. Interprocess Coordination (MORE IMPORTANT - TDE focus)
**Problem**: Orchestrating processes that depend on each other

**Simple solution**: Message exchanges
- **Problem**: Doesn't scale well

**Better solutions**:
- **Apache Zookeeper** (zookeeper.apache.org)
- **etcd** (etcd.io)
  - Distributed key-value store
  - Supports `watch` mechanism for orchestration
  - Acts like a distributed database
  - Can coordinate processes effectively

**Connection to your TDE**: This is the main subject of your project!

---

## 4. Middleware Introduction (SDC_04)

### Why Middleware?

**Problems with raw sockets**:
- Need to define protocols (tedious, error-prone)
- Data serialization (byte sequences)
- TCP streams need message delimiters
- Format conversions (little-endian ↔ big-endian)
- Language-specific type conversions (e.g., C char → Java byte)

**Solution**: Use a PLATFORM (Middleware) to simplify distributed application development

### Middleware Benefits
- Transparency of location
- Transparency of data representation
- Remote operations (instead of raw bytes)
- Object migration
- Additional services

### Remote Procedure Call (RPC)
**Paradigm**: Use procedure call paradigm for client/server software
- Isolates implementation from interface
- Uses **stubs** (client-side) and **proxies/skeletons** (server-side)

**RPC Challenges**:
- Service location and invocation
- Failures (timeout, retrial)
- Security (authentication)
- Client/server binding (directories)
- Network data representation (XDR)
- Parameter passing

---

## 5. CORBA - Object-Oriented Middleware (SDC_05)

### OMG (Object Management Group)
- Consortium of developers, government, user groups
- Goals: Promote OO technologies, create specifications, define interoperability standards

### CORBA Overview
**Common Object Request Broker Architecture**

**Purpose**: Provide interoperability between objects running on distributed heterogeneous systems

**Key Features**:
1. Standardize external appearance of objects (interfaces)
2. Propose generic binding mechanism between objects
3. Hide distribution difficulties from programmers (transparency)
4. Offer standardized additional services

### CORBA Components

#### IDL (Interface Descriptive Language)
- Describes methods and variables
- Language-independent interface definition
- Similar concept to header files, but platform-neutral

**IDL Syntax Elements**:
- **Interface name**: Defines the object type
- **Method prototypes**: Return type, method name, parameters
- **Parameters**: Direction (in, out, inout) + type
- **Attributes**: Public variables (can be readonly)
- **Exceptions**: User-defined and standard exceptions
- **oneway**: Best-effort operations (no response expected)

**Example IDL**:
```idl
exception SaldoInsuficiente {};
interface Conta {
    readonly attribute float saldo;
    void deposito(in float valor);
    void saque(in float valor) raises (SaldoInsuficiente);
    oneway void shutdown();
};
```

**IDL Basic Types**:
- short, long, float, char, byte, boolean
- string, sequence
- User-defined types

**IDL Supports**:
- Exception handling (standard + user-defined)
- Inheritance

#### After IDL Compilation
- **STUBS** (client-side): Client code for remote invocation
- **SKELETON** (server-side): Server code to receive invocations

#### IOR (Interoperable Object Reference)
**Like a "ticket" to access the server**

Two types:
1. **TRANSIENT IOR**:
   - Dies when server dies
   - Cannot be reused after server restart

2. **PERSISTENT IOR**:
   - Survives server shutdown
   - Can be reused even after server restart
   - More flexible for real-world applications

### CORBA Implementations
Free options:
- ACE/TAO (C++)
- JacORB (Java)
- omniORB / omniORBpy (Python)

---

## 6. Web Services (SDC_06)

### What are Web Services?
- Applications based on the Internet (typically over HTTP)
- Distributed applications whose components (services) run on different devices
- Program-to-program interaction
- Large amounts of data exchange

### Web Service Characteristics
- Use XML/JSON for data exchange
- Messages sent as HTTP requests/responses
- Standard means for interfacing with software systems
- Can use other transports besides HTTP

### Web Services as "Glue" / "Wrapper"

**Key Concept**: WS act as standard interfaces to heterogeneous backend systems

```
Network Environment
    ↓ (XML/JSON)
WS Interface
    ↓ (converts to binary)
Final System (Database, CORBA, .NET, MOM, etc.)
```

**How it works**:
1. WS interfaces receive standardized XML/JSON messages from network
2. Transform XML/JSON data into formats understood by specific final system
3. Return response message

**Benefits**:
- Any programming language
- Any operating system
- Any middleware
- Can deliver binary messages for efficiency
- Integration within and outside firewall (B2B, EAI)

**Use Cases**:
- Desktop and handheld clients (lightweight)
- Reservation systems, order tracking
- Business-to-Business (B2B) integration
- Enterprise Application Integration (EAI)

### Two Main Approaches

---

## 6.1 SOAP (XML-based)

**Simple Object Access Protocol**

**Characteristics**:
- XML-based messages
- Custom tags/fields
- More formal/rigid structure

**WSDL (Web Services Description Language)**:
- Interface description for SOAP services
- **Equivalent to CORBA's IDL, but for Web Services**
- Describes available operations, parameters, return types

**Lab Tool**: **zeep** library (Python)

### Using Zeep (SOAP Client)

**Zeep** = Python SOAP client library (https://docs.python-zeep.org)

**Basic Usage**:
```python
from zeep import Client

# Connect to WSDL
url = 'https://www.dataaccess.com/webservicesserver/NumberConversion.wso'
client = Client(url + '?WSDL')

# Call service method
result = client.service.NumberToWords(123)
print(result)  # "one hundred and twenty three"
```

**How it Works**:
1. Client fetches WSDL from endpoint URL + `?wsdl`
2. Zeep parses WSDL to understand available methods
3. Generates client-side code automatically
4. Handles XML serialization/deserialization
5. You call methods as if they were local functions

**Real Example - CEP Service**:
```
URL: http://cep.republicavirtual.com.br/web_cep.php?cep=80215901&formato=xml
Returns address for Brazilian ZIP code
```

**Use cases**:
- Enterprise applications
- When you need formal contracts between services
- Complex transactions
- Strong typing requirements

---

## 6.2 REST (Resource-based)

**REpresentational State Transfer**

**Origin**: Roy Fielding's PhD thesis - describes architectural style for networked systems

### Core Principles
- **Resource-based**: Everything is a resource (any item of interest)
- **Each resource has its own URL** (VERY IMPORTANT!)
- Uses standard HTTP methods
- Stateless communication
- **Resources have representations** (capture the state of the resource)
- **Uses nouns (resources) instead of verbs (operations)**

### Representations
**MIME Types** (data formats):
- JSON (most common, lightweight)
- XML
- CSV (Comma-Separated Values)
- HTML
- Plain text

**Key Concept**: Resources are useless without representations to capture their state

### CRUD Operations (HTTP Methods)

| HTTP Method | CRUD Operation | Description |
|-------------|----------------|-------------|
| **GET** | Read | Retrieves the resource (no modification) |
| **POST** | Create | Creates a NEW resource that didn't exist |
| **PATCH** | Update | Modifies part of an existing resource |
| **PUT** | Update/Replace | Completely replaces resource with new version |
| **DELETE** | Delete | Removes the resource |

### REST Key Rules
1. Each resource MUST have its own unique URL
2. Use appropriate HTTP methods for operations
3. Stateless - each request contains all necessary information
4. Typically uses JSON (lighter than XML)

**Lab Tool**: **Flask-RESTFUL** (Python)
- Define API endpoints
- Implement handlers for GET, POST, PATCH, PUT, DELETE
- Each resource class maps to URL routes

**Use cases**:
- Web APIs
- Mobile backends
- Microservices
- When simplicity and performance matter
- Public APIs

### REST vs SOAP Comparison

**SOAP Example**:
```xml
<?xml version="1.0"?>
<soap:Envelope xmlns:soap="...">
  <soap:Body pb="http://www.banco.com/contas">
    <pb:ObtemSaldo>
      <pb:ID>1234-5</pb:ID>
    </pb:ObtemSaldo>
  </soap:Body>
</soap:Envelope>
```

**REST Equivalent**:
```
http://www.banco.com/contas/1234-5
```

**Analogy**:
- SOAP = "Letter" (with envelope, attachments, formal structure)
- REST = "Postcard" (uses less "paper"/bandwidth, simpler)

**Key Points**:
- REST is simpler than SOAP (no WSDL or XML schemas required)
- "Lightweight Web Services"
- Everything that can be done with SOAP can be done with REST
- Same security level as SOAP
- REST saves bandwidth (less verbose)

---

## 7. Comparison Tables for Quick Review

### CORBA vs Web Services

| Aspect | CORBA | Web Services (SOAP) | Web Services (REST) |
|--------|-------|-------------------|-------------------|
| **Interface Definition** | IDL | WSDL | No formal spec (OpenAPI optional) |
| **Client Code** | Stubs | Stubs/Proxies | HTTP client library |
| **Server Code** | Skeleton | Service implementation | Resource handlers |
| **Data Format** | Binary (IIOP) | XML | JSON (typically) |
| **Transport** | IIOP/TCP | HTTP/HTTPS | HTTP/HTTPS |
| **"Ticket"** | IOR (Transient/Persistent) | Endpoint URL | Resource URL |
| **Complexity** | High | Medium-High | Low |
| **Best for** | Enterprise distributed objects | Formal web services | Web APIs, microservices |

### Interprocess Coordination Solutions

| Tool | Type | Key Feature | Use Case |
|------|------|-------------|----------|
| **Messages** | Simple | Direct communication | Small scale, simple coordination |
| **Zookeeper** | Distributed coordination | Configuration management, synchronization | Large distributed systems |
| **etcd** | Distributed key-value store | `watch` mechanism, distributed config | Modern cloud-native apps |

---

## 8. Important Conceptual Understanding

### The Full Stack (Bottom to Top)

```
Application Layer (Your Code)
    ↓
Middleware Layer (CORBA/WebServices)
    ↓
Transport Layer (Sockets/TCP/HTTP)
    ↓
Network Layer (IP)
    ↓
Physical Layer (Hardware)
```

**Key Insight**: Everything builds on sockets!
- CORBA uses sockets (via IIOP)
- Web Services use sockets (via HTTP/TCP)
- Understanding sockets is fundamental

### Why Use Middleware?

**Without Middleware (Raw Sockets)**:
```python
# You handle everything manually:
- Protocol definition
- Byte serialization
- Endianness conversion
- Message framing
- Error handling
- Connection management
```

**With Middleware (CORBA/WebServices)**:
```python
# Middleware handles:
- Data marshalling/unmarshalling
- Remote invocation
- Location transparency
- Type safety
- Service discovery

# You focus on:
- Business logic
- Application functionality
```

---

## 9. Test Preparation Checklist

### Conceptual Questions to Prepare For

1. **Sockets**:
   - [ ] Can you explain the 4 values needed for socket connection?
   - [ ] Why are protocols important in socket communication?
   - [ ] How do sockets relate to higher-level middleware?

2. **Distributed System Design**:
   - [ ] What types of transparency exist and why are they important?
   - [ ] How is reliability achieved in distributed systems?
   - [ ] Difference between availability and fault tolerance?

3. **Concurrency**:
   - [ ] Difference between parallel and distributed programming?
   - [ ] Why is interprocess coordination important?
   - [ ] How do etcd and Zookeeper solve coordination problems?
   - [ ] How does this relate to your TDE project?

4. **Middleware**:
   - [ ] Why was middleware created?
   - [ ] What problems does it solve?
   - [ ] What is RPC and how does it work?

5. **CORBA**:
   - [ ] What is IDL and what does it generate?
   - [ ] Difference between stubs and skeletons?
   - [ ] What is an IOR?
   - [ ] Difference between TRANSIENT and PERSISTENT IOR?
   - [ ] When would you use each type?

6. **Web Services**:
   - [ ] What is the difference between SOAP and REST?
   - [ ] What is WSDL and what is it equivalent to?
   - [ ] Can you explain all CRUD operations?
   - [ ] Why must each REST resource have its own URL?
   - [ ] When would you choose SOAP vs REST?

7. **Comparisons**:
   - [ ] CORBA vs Web Services - when to use each?
   - [ ] SOAP vs REST - advantages/disadvantages?
   - [ ] etcd vs Zookeeper - what do they solve?
   - [ ] Parallel vs Distributed programming - key differences?

### Implementation Questions (Based on Labs)

8. **Your Lab Implementations**:
   - [ ] How did you implement socket communication in Lab 01?
   - [ ] How did you use CORBA in your labs? (IDL → compilation → IOR)
   - [ ] How did you implement REST API with Flask-RESTFUL?
   - [ ] How did you use SOAP with zeep?
   - [ ] How did you use etcd or Zookeeper in your TDE?

---

## 10. Quick Review - Key Terms

### Must-Know Definitions

- **Socket**: Communication endpoint (IP + port)
- **Protocol**: Rules for how data is formatted and transmitted
- **Middleware**: Software layer between application and transport
- **RPC**: Remote Procedure Call - calling functions on remote machines
- **Stub**: Client-side code for remote invocation
- **Skeleton**: Server-side code to receive remote calls
- **IDL**: Interface Definition Language (CORBA)
- **WSDL**: Web Services Description Language (SOAP's IDL)
- **IOR**: Interoperable Object Reference (CORBA's "endpoint")
- **TRANSIENT**: Dies with the server
- **PERSISTENT**: Survives server restarts
- **CRUD**: Create, Read, Update, Delete
- **REST**: REpresentational State Transfer (resource-based)
- **SOAP**: Simple Object Access Protocol (XML-based)
- **etcd**: Distributed key-value store with watch capability
- **Zookeeper**: Distributed coordination service

---

## 11. "Big Picture" Questions

These are conceptual questions about the overall discipline:

1. **Evolution of Distributed Systems**:
   - Sockets (low-level) → RPC (procedural) → CORBA (OO) → Web Services (Internet-scale)

2. **Why does each generation exist?**:
   - Sockets: Maximum control, but very complex
   - RPC: Procedural abstraction, easier than sockets
   - CORBA: Object-oriented, language-independent, enterprise-grade
   - SOAP: Internet-friendly, XML-based, cross-platform
   - REST: Simple, lightweight, web-native

3. **Trade-offs**:
   - **Control vs Convenience**: Sockets give control, middleware gives convenience
   - **Performance vs Simplicity**: Binary protocols (CORBA) vs text (REST)
   - **Formal vs Flexible**: SOAP/WSDL (formal) vs REST (flexible)

4. **Modern Trends**:
   - REST dominates for web APIs (simplicity wins)
   - SOAP still used in enterprise (formal contracts needed)
   - CORBA legacy systems (but declining)
   - Coordination tools (etcd/Zookeeper) critical for microservices

---

## 12. Common Mistakes to Avoid

1. **Confusing stubs and skeletons**: Stubs are CLIENT-side, skeletons are SERVER-side
2. **Mixing up IDL and WSDL**: IDL is for CORBA, WSDL is for SOAP
3. **Forgetting the 4 socket values**: Client IP/port + Server IP/port = 4 values
4. **Confusing PUT and PATCH**: PUT replaces entire resource, PATCH modifies partially
5. **Thinking etcd is just a database**: It's a coordination tool with watch capability
6. **Missing the importance of URLs in REST**: Each resource NEEDS its own URL!

---

## Study Tips

1. **Review your lab implementations** - professor said this is important!
2. **Practice explaining concepts** - not just memorizing definitions
3. **Draw diagrams** - especially for CORBA architecture and client-server flows
4. **Compare and contrast** - CORBA vs WebServices, SOAP vs REST
5. **Connect to TDE** - your project likely uses etcd or Zookeeper for coordination
6. **Think about trade-offs** - why would you choose one technology over another?

---

Good luck on your test! Focus on understanding the concepts and being able to explain your implementation decisions.
