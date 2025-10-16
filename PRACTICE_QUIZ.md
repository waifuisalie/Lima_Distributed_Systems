# Distributed Systems - Practice Quiz

## Section 1: Socket Fundamentals (Lab 01)

### Question 1
**What are the 4 crucial values needed to establish a socket connection between a client and server?**

<details>
<summary>Click to see answer</summary>

1. Client IP address
2. Client port number
3. Server IP address
4. Server port number

These 4 values are essential for establishing a TCP/UDP connection.
</details>

---

### Question 2
**Why are protocols important in socket communication? What problem do they solve?**

<details>
<summary>Click to see answer</summary>

Protocols are critical because:
- Systems communicate by sending BYTES
- The receiver doesn't know when a message has finished
- Without a protocol, there's no way to know message boundaries
- Protocols define things like endianness (little-endian vs big-endian)
- They standardize how data is formatted and interpreted

Example: TCP is a stream protocol, so you need to define message delimiters or length prefixes.
</details>

---

### Question 3
**In Lab 01, you used NetCat to create servers and clients. Explain how you would create a persistent TCP server that continuously serves requests.**

<details>
<summary>Click to see answer</summary>

Use a while loop in the shell:
```bash
while [ 1 ]; do
    nc -l <port> < response_data
done
```

This makes the server:
- Listen on a port
- Serve one request
- Exit and restart automatically (loop)
- Continue accepting new connections

The while loop ensures the server restarts after each client disconnection.
</details>

---

## Section 2: Distributed Systems Fundamentals

### Question 3.5
**What is Lamport's famous definition of a distributed system? What does it tell us about distributed systems?**

<details>
<summary>Click to see answer</summary>

**Lamport's Definition**:
> "A distributed system is a system in which a failure in a computer that you didn't even know existed can make your own computer unusable."

**What it tells us**:
- Distributed systems have **hidden dependencies**
- Failures in remote components can affect you
- Complexity comes from distributed nature
- You might not even know all the components involved
- Failure handling is CRITICAL in distributed systems

**Key Insight**: This humorous definition highlights the main challenge - dealing with failures and dependencies in components you don't directly control.
</details>

---

### Question 3.6
**What is the fundamental difference between heterogeneity and interoperability in distributed systems?**

<details>
<summary>Click to see answer</summary>

**Heterogeneity** = The problem
**Interoperability** = The solution

**Heterogeneity Issues** (No consensus on):
- Hardware platforms (Intel, ARM, etc.)
- Programming languages (Java, Python, C++)
- Operating systems (Linux, Windows, macOS)
- Network protocols

**Interoperability Requirement**:
- **There MUST be consensus on interfaces and models**
- This is why middleware exists!
- Standard interfaces allow heterogeneous components to work together

**Example**:
- Your Python client (heterogeneous) can talk to a Java server (heterogeneous)
- Because they both use REST/HTTP (interoperable interface)

**Key Point**: We can't eliminate heterogeneity, but we can achieve interoperability through standards.
</details>

---

### Question 3.7
**Why did distributed systems evolve from monolithic applications to component-based architectures? List at least 4 benefits.**

<details>
<summary>Click to see answer</summary>

**Evolution**:
- From: Monolithic applications (everything in one big program)
- To: Component-based architectures (cooperating software components)

**Benefits of Component Architecture** (at least 4):

1. **Distribution**: Components can run on different machines
2. **Update/Upgrading**: Update individual components without replacing entire system
3. **Maintenance**: Easier to maintain smaller, focused components
4. **Reuse**: Components can be reused in different applications
5. **Flexibility**: Easy reconfiguration by swapping components
6. **Availability**: If one component fails, others can continue
7. **Fault Tolerance**: Redundant components provide backup

**Modern Examples**:
- Microservices architecture
- Service-Oriented Architecture (SOA)
- Cloud-native applications

**Connection to course**: This is why we study middleware - to build component-based distributed systems!
</details>

---

### Question 4
**Explain the concept of "location transparency" in distributed systems. Why is it important?**

<details>
<summary>Click to see answer</summary>

**Location Transparency**: Users/programs don't need to know the physical location of resources.

**Importance**:
- Resources can be moved without breaking applications
- Users see a single unified system (not a collection of machines)
- Simplifies system administration
- Enables load balancing and replication
- Core goal: make distributed system look like centralized system

**Example**: You call a method on an object - you shouldn't need to know if it's local or on another machine.
</details>

---

### Question 5
**What is the difference between hardware redundancy and software redundancy for achieving reliability?**

<details>
<summary>Click to see answer</summary>

**Hardware Redundancy**:
- Multiple physical components (processors, disks, memory, network links)
- If one fails, another takes over
- Example: RAID for disk redundancy, backup power supplies

**Software Redundancy**:
- Multiple different programs performing the same function
- Protects against software bugs
- If one implementation has a bug, another might not
- More complex to implement than hardware redundancy

**Both** are used together in highly reliable distributed systems.
</details>

---

## Section 3: Concurrency

### Question 6
**Compare Parallel Programming and Distributed Programming. What are their main differences?**

<details>
<summary>Click to see answer</summary>

| Aspect | Parallel Programming | Distributed Programming |
|--------|---------------------|------------------------|
| **Primary Goal** | Performance (beyond single processor capability) | Convenience (availability, reliability) then performance |
| **Interaction** | Frequent, high granularity, low overhead, reliable | Less frequent, large data volumes, potentially unreliable |
| **Communication** | Shared memory or fast interconnect | Network messages |
| **Failure Model** | Generally reliable | Must handle network/node failures |

**Key Insight**: They exist on a spectrum - learning one helps understand the other.
</details>

---

### Question 7
**You need to coordinate multiple distributed processes that depend on each other. What tools would you use and why?**

<details>
<summary>Click to see answer</summary>

**Problem**: Interprocess coordination - processes need to wait for or signal each other.

**Simple Solution**: Message exchange
- **Problem**: Doesn't scale well with many processes

**Better Solutions**:

**etcd**:
- Distributed key-value store
- `watch` mechanism allows processes to monitor changes
- Excellent for configuration management
- Acts like a distributed database with notifications

**Apache Zookeeper**:
- Distributed coordination service
- Provides primitives for synchronization
- Used in many large-scale systems (Hadoop, Kafka)

**When to use**:
- Multiple processes need shared state
- Need distributed locks
- Configuration needs to be synchronized
- Leader election is required

**Connection to TDE**: This is likely what your project uses!
</details>

---

### Question 8
**What is the difference between interprocess and intraprocess concurrency? Which is more important for this test?**

<details>
<summary>Click to see answer</summary>

**Intraprocess Concurrency** (LESS important for test):
- Multiple threads within a single process
- Issues: race conditions, need for semaphores/mutex/monitors
- Example: Server creating thread for each client connection

**Interprocess Coordination** (MORE important for test):
- Coordination between separate processes (possibly on different machines)
- Issues: distributed state, synchronization across network
- Solutions: etcd, Zookeeper
- **This is the focus of the TDE project**

**Professor emphasized** interprocess coordination over intraprocess.
</details>

---

## Section 4: Middleware Concepts

### Question 9
**List at least 3 problems that middleware solves compared to using raw sockets.**

<details>
<summary>Click to see answer</summary>

**Problems with raw sockets that middleware solves**:

1. **Protocol Definition**: No need to manually define message formats (tedious and error-prone)

2. **Data Serialization**: Automatic conversion between language types and byte sequences

3. **Endianness Conversion**: Automatic handling of little-endian vs big-endian

4. **Message Framing**: TCP streams need delimiters - middleware handles this

5. **Type Safety**: Language-specific type conversions (e.g., C char → Java byte)

6. **Location Transparency**: Don't need to manually manage IP addresses and ports

**Result**: You focus on business logic, not communication details.
</details>

---

### Question 10
**What is RPC (Remote Procedure Call)? What role do stubs and proxies play?**

<details>
<summary>Click to see answer</summary>

**RPC (Remote Procedure Call)**:
- Paradigm for building client/server systems
- Makes remote function calls look like local calls
- Procedural programming approach (not object-oriented)

**Stubs (Client-side)**:
- Generated code on client
- Marshals (packs) parameters
- Sends request over network
- Unmarshals (unpacks) results
- Makes remote call look local to client

**Proxies/Skeletons (Server-side)**:
- Generated code on server
- Receives network request
- Unmarshals parameters
- Calls actual implementation
- Marshals results and sends back

**Key Concept**: Stubs and proxies hide the network communication complexity.
</details>

---

## Section 5: CORBA

### Question 11
**What is IDL in CORBA? What does compiling an IDL file produce?**

<details>
<summary>Click to see answer</summary>

**IDL (Interface Definition Language)**:
- Language for describing object interfaces in CORBA
- Platform and language independent
- Defines methods, parameters, return types, data structures

**After compiling IDL, you get**:

1. **Stubs** (Client-side):
   - Code for client to invoke remote methods
   - Handles marshalling, network communication

2. **Skeleton** (Server-side):
   - Code for server to receive invocations
   - Handles unmarshalling, dispatching to implementation

**Similar to**: Header files in C, but language-neutral and generates code for both sides.
</details>

---

### Question 12
**What is an IOR in CORBA? Explain the difference between TRANSIENT and PERSISTENT IOR.**

<details>
<summary>Click to see answer</summary>

**IOR (Interoperable Object Reference)**:
- Like a "ticket" or "reference" to access a remote object
- Contains server location, object identity, protocol info
- Client needs this to communicate with server object

**TRANSIENT IOR**:
- Dies when the server process terminates
- Cannot be reused after server restart
- Simpler to implement
- **Use when**: Short-lived services, testing

**PERSISTENT IOR**:
- Survives server shutdown and restart
- Can be saved to file and reused later
- More flexible for production systems
- **Use when**: Long-lived services, need durability

**Analogy**:
- TRANSIENT = phone call (ends when you hang up)
- PERSISTENT = phone number (can call back anytime)
</details>

---

### Question 12.5
**Write a simple IDL interface for a bank account with a read-only balance attribute, deposit and withdrawal methods. Include exception handling.**

<details>
<parameter name="summary">Click to see answer</summary>

**Example IDL**:
```idl
exception SaldoInsuficiente {};  // Insufficient balance exception

interface Conta {
    readonly attribute float saldo;  // Read-only balance attribute

    void deposito(in float valor);   // Deposit method

    void saque(in float valor)       // Withdrawal method
        raises (SaldoInsuficiente);  // Can raise exception

    oneway void shutdown();          // Best-effort shutdown
};
```

**Key Elements**:
- **exception**: User-defined exception (must declare before use)
- **interface**: Defines the object type
- **readonly attribute**: Can only be read, not modified by client
- **in**: Parameter direction (input to server)
- **raises**: Specifies which exceptions the method can throw
- **oneway**: Best-effort operation (no response expected, no guarantees)

**Parameter Directions**:
- **in**: Input parameter (client → server)
- **out**: Output parameter (server → client)
- **inout**: Both directions (bidirectional)

**IDL Basic Types**:
- short, long, float, char, byte, boolean
- string, sequence
</details>

---

### Question 13
**Multiple Choice: Which component is generated on the CLIENT side after compiling a CORBA IDL file?**

A) Skeleton
B) Stub
C) IOR
D) ORB

<details>
<summary>Click to see answer</summary>

**Answer: B) Stub**

**Explanation**:
- **Stub**: Client-side code (correct answer)
- **Skeleton**: Server-side code
- **IOR**: Generated at runtime by server
- **ORB**: Object Request Broker (runtime component, not generated from IDL)

**Remember**:
- STub = ClienT (both have 'T')
- SKELeton = ServEr (both have 'E')
</details>

---

## Section 6: Web Services - SOAP

### Question 14
**What is WSDL? What is it equivalent to in the CORBA world?**

<details>
<summary>Click to see answer</summary>

**WSDL (Web Services Description Language)**:
- XML-based language for describing web service interfaces
- Specifies operations, parameters, data types, endpoints
- Used with SOAP web services

**Equivalent to**: CORBA's **IDL** (Interface Definition Language)

**Both serve the same purpose**:
- Formal interface description
- Language-independent specification
- Used to generate client/server code
- Contract between service provider and consumer

**Key difference**: WSDL is XML-based (web-friendly), IDL has its own syntax.
</details>

---

### Question 15
**In your lab, you used the 'zeep' library. What was it used for?**

<details>
<summary>Click to see answer</summary>

**zeep**: Python library for working with **SOAP** web services

**What it does**:
- Parses WSDL files
- Generates client-side code automatically
- Handles XML serialization/deserialization
- Makes SOAP calls from Python

**Usage pattern**:
```python
from zeep import Client
client = Client('http://service.com/service?wsdl')
result = client.service.some_method(param1, param2)
```

**Connection**: zeep handles the stub/proxy role for SOAP services (like CORBA stubs).
</details>

---

## Section 7: Web Services - REST

### Question 16
**Explain each CRUD operation and its corresponding HTTP method. What does each one do?**

<details>
<summary>Click to see answer</summary>

| CRUD | HTTP Method | Description | Idempotent? |
|------|-------------|-------------|-------------|
| **Create** | POST | Creates a NEW resource that didn't exist before | No |
| **Read** | GET | Retrieves resource without modification | Yes |
| **Update** | PATCH | Modifies PART of an existing resource | No* |
| **Update** | PUT | REPLACES entire resource with new version | Yes |
| **Delete** | DELETE | Removes the resource | Yes |

**Key Differences**:
- **POST vs PUT**: POST creates new, PUT replaces existing
- **PATCH vs PUT**: PATCH modifies partially, PUT replaces entirely
- **GET**: Should never modify data (safe operation)

**Idempotent**: Multiple identical requests have same effect as single request
</details>

---

### Question 17
**Why must each resource in REST have its own URL? What does this enable?**

<details>
<summary>Click to see answer</summary>

**Why each resource needs its own URL**:

1. **Resource Identification**: URL uniquely identifies the resource
2. **RESTful Principle**: "Resource-oriented" architecture
3. **Standard Operations**: Same HTTP methods work on different URLs
4. **Caching**: Browsers/proxies can cache based on URL
5. **Bookmarking**: Users can bookmark specific resources
6. **Stateless**: Each request is self-contained

**Example**:
```
GET  /api/users/123        # Get user 123
PUT  /api/users/123        # Update user 123
DELETE /api/users/123      # Delete user 123
GET  /api/users/123/posts  # Get posts by user 123
```

**Bad Design** (not RESTful):
```
POST /api/action with body: {action: "get_user", id: 123}
```

**Rule**: URL identifies WHAT (resource), HTTP method identifies HOW (operation).
</details>

---

### Question 18
**In your lab, you used Flask-RESTFUL. How did you define REST APIs with it?**

<details>
<summary>Click to see answer</summary>

**Flask-RESTFUL workflow**:

1. **Create resource classes**:
```python
class UserResource(Resource):
    def get(self, user_id):
        # Handle GET request
        return {'user': user_id}

    def put(self, user_id):
        # Handle PUT request
        return {'updated': user_id}

    def delete(self, user_id):
        # Handle DELETE request
        return {'deleted': user_id}
```

2. **Map to URLs**:
```python
api.add_resource(UserResource, '/users/<int:user_id>')
```

3. **Each HTTP method = one function** in the resource class

4. **Each resource gets its own URL pattern**

**Key concept**: You implement methods (get, post, put, patch, delete), Flask-RESTFUL routes HTTP requests to them.
</details>

---

### Question 18.5
**Use the analogy: "SOAP is like a letter, REST is like a postcard." Explain what this means and why REST uses less bandwidth.**

<details>
<summary>Click to see answer</summary>

**Analogy Explanation**:

**SOAP = Letter**:
- Has envelope (SOAP envelope)
- Formal structure (SOAP headers, body)
- Can have attachments
- More "paper" (XML verbosity)
- Formal addressing

**REST = Postcard**:
- Simple, direct message
- Less "paper" (just URL + minimal data)
- Lightweight format (JSON)
- Direct addressing (URL)

**Bandwidth Comparison**:

**SOAP Example** (get balance for account 1234-5):
```xml
<?xml version="1.0"?>
<soap:Envelope xmlns:soap="http://www.w3.org/2001/12/soap-envelope"
               soap:encodingStyle="...">
  <soap:Body pb="http://www.banco.com/contas">
    <pb:ObtemSaldo>
      <pb:ID>1234-5</pb:ID>
    </pb:ObtemSaldo>
  </soap:Body>
</soap:Envelope>
```
(~200+ bytes of XML)

**REST Equivalent**:
```
GET http://www.banco.com/contas/1234-5
```
(~40 bytes)

**Why REST is lighter**:
- No XML envelope overhead
- No namespace declarations
- URL encodes the resource and operation
- Typically uses JSON (more compact than XML)
- HTTP headers are smaller than SOAP headers

**But both**:
- Have equivalent functionality
- Provide same security level
- Can do the same things

**Trade-off**: SOAP provides formal contracts (WSDL), REST provides simplicity.
</details>

---

### Question 19
**Multiple Choice: Which HTTP method is used to PARTIALLY update a resource?**

A) POST
B) PUT
C) PATCH
D) UPDATE

<details>
<summary>Click to see answer</summary>

**Answer: C) PATCH**

**Explanation**:
- **POST**: Creates new resource
- **PUT**: Replaces ENTIRE resource (full update)
- **PATCH**: Modifies PART of resource (partial update) ✓
- **UPDATE**: Not a real HTTP method

**Example scenario**:
```
User resource: {name: "Alice", email: "alice@example.com", age: 30}

PATCH /users/1 with {age: 31}
→ Only age changes, name and email stay the same

PUT /users/1 with {age: 31}
→ Entire resource replaced - name and email might be lost!
```
</details>

---

### Question 19.5
**Explain how Web Services act as "glue" or "wrapper" for heterogeneous backend systems. Why is this important?**

<details>
<summary>Click to see answer</summary>

**Web Services as Glue/Wrapper**:

**Architecture**:
```
Network Environment (clients, browsers, mobile apps)
    ↓ (XML/JSON messages)
WS Interface Layer (standardized)
    ↓ (converts to binary/native formats)
Backend Systems (Database, CORBA, .NET, MOM, Legacy systems)
```

**How it works**:
1. **Receive**: WS interface receives standardized XML/JSON from network
2. **Transform**: Converts data into format understood by specific backend system
3. **Process**: Backend system processes request in its native format
4. **Return**: Response converted back to XML/JSON
5. **Send**: Standardized response sent to client

**Why this is important**:

**Integration Benefits**:
- **Any programming language** on client side
- **Any operating system** on either side
- **Any middleware** in backend (CORBA, .NET, MOM)
- **Legacy system integration** without rewriting them
- **B2B (Business-to-Business)** integration across companies
- **EAI (Enterprise Application Integration)** within firewall

**Real-World Example**:
```
Mobile App (iOS/Swift)
    ↓ (JSON via REST)
Web Service Wrapper
    ↓ (converts to proprietary format)
Legacy COBOL Mainframe System
```

**Key Advantage**: Backend system doesn't need to change - WS provides modern interface to old systems.

**Efficiency Note**: For performance, WS can also deliver binary messages when needed (not just XML/JSON).
</details>

---

## Section 8: Comparisons

### Question 20
**Compare SOAP and REST. When would you choose one over the other?**

<details>
<summary>Click to see answer</summary>

| Aspect | SOAP | REST |
|--------|------|------|
| **Data Format** | XML (verbose) | JSON (lightweight) |
| **Interface Spec** | WSDL (formal contract) | No formal spec (OpenAPI optional) |
| **Complexity** | High (many standards) | Low (just HTTP) |
| **Type Safety** | Strong (WSDL defines types) | Weak (JSON is flexible) |
| **Overhead** | Higher (XML parsing) | Lower (JSON parsing) |
| **Tooling** | Enterprise tools (zeep) | Simple HTTP libraries |

**Choose SOAP when**:
- Need formal contracts (WSDL)
- Enterprise environment
- Strong type safety required
- Complex transactions
- Legacy system integration

**Choose REST when**:
- Building web APIs
- Need simplicity
- Mobile/web clients
- Public APIs
- Performance matters
- Modern microservices

**Trend**: REST dominates modern web development, SOAP still used in enterprise.
</details>

---

### Question 21
**Compare CORBA and Web Services (both SOAP and REST). What are the main differences?**

<details>
<summary>Click to see answer</summary>

| Aspect | CORBA | SOAP | REST |
|--------|-------|------|------|
| **Era** | 1990s | 2000s | 2000s+ |
| **Paradigm** | Distributed objects | Service-oriented | Resource-oriented |
| **Interface** | IDL | WSDL | No formal spec |
| **Protocol** | IIOP (binary) | HTTP + XML | HTTP + JSON |
| **Language Independence** | Yes | Yes | Yes |
| **Complexity** | High | Medium-High | Low |
| **Current Use** | Legacy enterprise | Enterprise web services | Modern web APIs |

**CORBA advantages**:
- Binary protocol (faster)
- True object distribution
- Language independence

**CORBA disadvantages**:
- Complex
- Firewall unfriendly
- Declining adoption

**Web Services advantages**:
- HTTP-based (firewall friendly)
- Internet-native
- Simpler (especially REST)

**Evolution**: Sockets → RPC → CORBA → SOAP → REST
</details>

---

## Section 9: Implementation Questions

### Question 22
**You're building a distributed system where multiple services need to coordinate and share configuration. Which technology would you use and why?**

<details>
<summary>Click to see answer</summary>

**Answer: etcd or Zookeeper**

**Why**:
- Need distributed coordination (not just communication)
- Need shared configuration across services
- Need to detect changes (watch mechanism)
- Message passing alone doesn't scale

**etcd approach**:
```
1. Store configuration as key-value pairs
2. Services watch specific keys
3. When config changes, all watchers notified
4. Services update behavior automatically
```

**Example use case**:
- Microservices architecture
- Distributed lock management
- Service discovery
- Configuration management
- Leader election

**Connection to TDE**: Your project probably uses this for coordination!

**Wrong answers**:
- ~~CORBA~~: Too complex, wrong problem
- ~~REST API~~: Doesn't solve coordination
- ~~Raw sockets~~: Too low-level, no coordination primitives
</details>

---

### Question 23
**Explain how you would implement a simple RESTful API for managing a list of books. Include the URLs and HTTP methods.**

<details>
<summary>Click to see answer</summary>

**Resource**: Books

**API Design**:

```
# List all books
GET /api/books
→ Returns: [{id: 1, title: "Book1"}, {id: 2, title: "Book2"}]

# Get specific book
GET /api/books/1
→ Returns: {id: 1, title: "Book1", author: "Author1", year: 2020}

# Create new book
POST /api/books
Body: {title: "New Book", author: "New Author", year: 2024}
→ Returns: {id: 3, title: "New Book", ...}

# Update book (full replacement)
PUT /api/books/1
Body: {title: "Updated", author: "Updated Author", year: 2024}
→ Returns: {id: 1, title: "Updated", ...}

# Partial update
PATCH /api/books/1
Body: {year: 2025}
→ Returns: {id: 1, title: "Book1", author: "Author1", year: 2025}

# Delete book
DELETE /api/books/1
→ Returns: {deleted: true, id: 1}
```

**Key principles**:
- Each book has unique URL: `/api/books/{id}`
- Collection URL: `/api/books`
- HTTP method indicates operation
- JSON for data exchange
</details>

---

### Question 24
**In CORBA, you have a Calculator interface with methods add and subtract. Describe the steps from writing IDL to making a remote call.**

<details>
<summary>Click to see answer</summary>

**Step-by-step process**:

**1. Write IDL file** (`Calculator.idl`):
```idl
interface Calculator {
    long add(in long a, in long b);
    long subtract(in long a, in long b);
};
```

**2. Compile IDL**:
```bash
idl Calculator.idl
```
Generates:
- **Stub** (client code)
- **Skeleton** (server code)

**3. Server implementation**:
- Implement Calculator interface
- Start ORB (Object Request Broker)
- Register object with ORB
- Generate IOR (Interoperable Object Reference)
- Make IOR available to clients (file, naming service, etc.)

**4. Client implementation**:
- Initialize ORB
- Obtain IOR (from file, naming service, etc.)
- Convert IOR to object reference
- Call methods like local calls

**5. Runtime**:
```
Client calls calc.add(5, 3)
    ↓
Stub marshals parameters
    ↓
Network transmission (IIOP)
    ↓
Skeleton unmarshals parameters
    ↓
Implementation executes add(5, 3)
    ↓
Result returned (reverse path)
```

**Key**: Everything in between is handled by CORBA - you just call methods!
</details>

---

## Section 10: Conceptual "Big Picture" Questions

### Question 25
**Explain the evolution of distributed systems technologies from sockets to modern web services. Why did each generation emerge?**

<details>
<summary>Click to see answer</summary>

**Evolution timeline**:

**1. Raw Sockets (1980s)**:
- **What**: Direct TCP/UDP communication
- **Pros**: Maximum control, performance
- **Cons**: Very complex, error-prone, lots of boilerplate
- **Why it emerged**: Basic network communication primitive

**2. RPC - Remote Procedure Call (1980s-90s)**:
- **What**: Procedural abstraction over network
- **Pros**: Easier than sockets, function-call syntax
- **Cons**: Still procedural (not OO), language-specific
- **Why it emerged**: Hide network complexity

**3. CORBA - Object-Oriented Middleware (1990s)**:
- **What**: Distributed objects, language-independent
- **Pros**: True OO, language independence, comprehensive
- **Cons**: Complex, heavy, firewall issues
- **Why it emerged**: Support enterprise OO applications

**4. SOAP - XML Web Services (2000s)**:
- **What**: Web-based services using XML
- **Pros**: HTTP-based (firewall-friendly), formal contracts (WSDL)
- **Cons**: XML overhead, still complex
- **Why it emerged**: Internet-scale interoperability

**5. REST - Lightweight Web APIs (2000s-present)**:
- **What**: Resource-oriented, uses HTTP naturally
- **Pros**: Simple, lightweight, web-native, JSON
- **Cons**: Less formal, no built-in contract
- **Why it emerged**: Simplicity and web/mobile revolution

**Pattern**: Each generation trades control for convenience, except REST which also brings back simplicity.

**Modern reality**:
- REST dominates (web/mobile APIs)
- SOAP persists (enterprise)
- CORBA declining (legacy)
- Sockets always underneath
</details>

---

### Question 26
**Your professor said "labs are important." Based on what you learned, explain how Lab 01 (NetCat/Sockets) connects to CORBA and Web Services.**

<details>
<summary>Click to see answer</summary>

**Connection - Everything builds on sockets!**

**Lab 01 Foundation**:
- IP addresses and ports (4 values for connection)
- Protocols for message framing
- TCP/UDP communication
- Challenges: byte streams, endianness, message boundaries

**How CORBA uses these concepts**:
- IIOP (Internet Inter-ORB Protocol) runs over TCP
- Still needs IP + port (but hidden in IOR)
- Still sends bytes (but handled by ORB)
- CORBA provides protocol (don't need to define yourself)
- Stubs/skeletons hide socket complexity

**How Web Services use these concepts**:
- HTTP runs over TCP (which uses sockets)
- Web server listens on port 80/443
- HTTP defines protocol (headers, methods, etc.)
- REST/SOAP hide socket complexity
- You work with URLs, not IP addresses

**Key insight from professor**:
> "Sockets are always present, whether it's CORBA or Web Services, they all use sockets."

**Lab progression**:
1. Lab 01: Learn raw sockets (hard way)
2. Later labs: Use middleware (easier way)
3. Appreciation: Understand what middleware does for you

**Why labs matter for test**:
- Understanding implementation decisions
- Knowing trade-offs
- Conceptual questions about "how did you implement X?"

**Big picture**:
```
Your application
    ↓
Middleware (CORBA/WebServices) ← Higher-level abstraction
    ↓
Sockets (Lab 01) ← Foundation you learned
    ↓
TCP/IP
    ↓
Network hardware
```
</details>

---

## Bonus: Quick Fire Review

### Question 27
**Rapid-fire definitions - Define in one sentence:**

a) Stub
b) Skeleton
c) IDL
d) WSDL
e) IOR
f) CRUD
g) REST
h) SOAP
i) etcd
j) Middleware

<details>
<summary>Click to see answers</summary>

a) **Stub**: Client-side generated code that marshals parameters and makes remote calls
b) **Skeleton**: Server-side generated code that unmarshals parameters and dispatches to implementation
c) **IDL**: Interface Definition Language - describes CORBA object interfaces
d) **WSDL**: Web Services Description Language - describes SOAP service interfaces (SOAP's IDL)
e) **IOR**: Interoperable Object Reference - CORBA's "address" or "ticket" to access remote objects
f) **CRUD**: Create, Read, Update, Delete - basic database/API operations
g) **REST**: REpresentational State Transfer - resource-oriented architecture using HTTP
h) **SOAP**: Simple Object Access Protocol - XML-based web services protocol
i) **etcd**: Distributed key-value store with watch capability for coordination
j) **Middleware**: Software layer between application and network that simplifies distributed programming
</details>

---

## Answer Key Summary

### Critical Concepts to Memorize:

1. **4 socket values**: Client IP/port + Server IP/port
2. **Stub = Client**, Skeleton = Server
3. **WSDL = SOAP's IDL**
4. **TRANSIENT vs PERSISTENT IOR**
5. **GET (read), POST (create), PATCH (partial update), PUT (replace), DELETE (delete)**
6. **Each REST resource needs its own URL**
7. **etcd/Zookeeper for distributed coordination** (TDE focus!)
8. **Middleware solves**: protocols, serialization, endianness, message framing
9. **Parallel vs Distributed**: performance vs convenience/reliability
10. **Everything runs on sockets underneath**

---

Good luck on your test tomorrow! Review these questions, understand the concepts, and be ready to explain your lab implementations!
