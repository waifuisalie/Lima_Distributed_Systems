# Distributed Systems Test Preparation - Context & Plan

## Project Context
This is a distributed systems course focusing on multisystems, middlewares, and interprocess coordination. The student has a test tomorrow and needs help preparing using PDF materials from the course.

## Course Topics (from student notes in multisystems_review.md)

### 1. Multisystems

#### Sync or Async Systems
- Focus on how CORBA handles sync/async
- Message control is fundamental
- Key concepts studied:
  - **Pipes**: Foundation for understanding Sockets
  - **Protocols**: Critical because systems send BYTES, receiver doesn't know when message finished
  - Sockets predetermine protocol (e.g., little endian)
  - Sockets are cumbersome but fundamental (used in CORBA, WebServices, everything)
  - Connection requirements: IP + port (client & server = **4 crucial values**)

#### Parallel Activities & Concurrency
- **Interprocess Concurrency** (less focus on test): Multiple threads per connection → race conditions → need semaphores/mutex
- **Interprocess Coordination** (MORE IMPORTANT): Orchestrating processes that depend on each other
  - Solved with message exchanges (but doesn't scale well)
  - Better solutions:
    - **Zookeeper**
    - **etcd**: Distributed key-value store, supports `watch` for orchestration
  - **CHECK TDE** (student project) - this is the main subject focus

### 2. Middlewares

Two main types covered (third for next test):
1. OOP-based (CORBA)
2. Services (Web-based)

#### Introduction to RPC & Middlewares
- **RPC (Remote Procedure Call)**: Isolates implementation from interface using stubs and proxies
- **OOP**: Methods accessible via network from other computers

#### CORBA (OOP-based Middleware)
- **IDL (Interface Descriptive Language)**: Describes methods and variables
- Compilation generates:
  - **STUBS** (client-side)
  - **SKELETON** (server-side)
- **IOR (Interoperable Object Reference)**: Server-generated "ticket" for client access
  - **TRANSIENT**: Dies when server dies
  - **PERSISTENT**: Survives server shutdown

#### WebServices
Uses web infrastructure and protocols for sending messages and requesting services.

**Two approaches:**

1. **SOAP** (XML-based):
   - Uses XML tags (customizable fields)
   - **WSDL** (Web Services Description Language): Interface description (equivalent to IDL)
   - Lab tool: **zeep** library

2. **REST** (Resource-based):
   - Each resource has specific URL
   - HTTP methods - **CRUD** operations:
     - **GET**: Read resource
     - **POST**: Create new resource
     - **PATCH**: Modify resource (partial update)
     - **PUT**: Replace entire resource (full update)
     - **DELETE**: Remove resource
   - Lab tool: **Flask-RESTFUL**
   - **Important**: Each resource needs its own URL

### 3. Test Format (from professor)
- Mixed: Multiple choice + open questions
- No syntax questions
- Questions about project implementations
- Conceptual questions about the discipline
- "Big questions" about overall concepts

## Available Resources

### PDF Files
Located in `/home/waifuisalie/Documents/Lima_Distributed_Systems/Lima_Distributed_Systems/`

- `Lab_01.pdf` (243K)
- `SDC_07.pdf` (1.9M)
- Multiple split PDFs in `split_pdfs/` directory:
  - Intro_0, Intro_1 series
  - SDC_2, SDC_03, SDC_04, SDC_05, SDC_06, SDC_08 series
  - Many files range from 1-5MB

### Available Tools for PDF Extraction

**Text Extraction:**
- `pdftotext`: Best for extracting raw text efficiently
- `qpdf`: PDF inspection, page extraction, JSON output

**Image Extraction (for diagrams, flowcharts, architecture drawings):**
- `pdfimages`: Extracts all images from PDFs
- `pdftoppm`: Converts PDF pages to images
- `ImageMagick (magick)`: Image processing/enhancement
- `Ghostscript (gs)`: Renders PDF pages as images at specific resolutions

## Extraction & Study Plan

### Phase 1: Text Extraction
1. Use `pdftotext` to extract text from key PDFs (Lab_01, relevant SDC chapters)
2. Focus on sections matching review topics:
   - Sockets, protocols, connection fundamentals
   - CORBA: IDL, Stubs, Skeleton, IOR
   - WebServices: SOAP/WSDL, REST/CRUD
   - etcd/Zookeeper for coordination

### Phase 2: Image Extraction
1. Use `pdfimages` or `pdftoppm` to extract diagrams from PDFs
2. Focus on:
   - CORBA architecture diagrams
   - WebService flow diagrams
   - etcd/Zookeeper coordination patterns
   - Client-server communication models

### Phase 3: Analysis & Study Material Creation
1. Feed extracted text to Claude for analysis
2. Share extracted images for diagram understanding
3. Create focused study materials:
   - Concept summaries
   - Comparison charts (CORBA vs WebServices, SOAP vs REST)
   - Practice questions based on course content
   - Review of TDE project implementation

### Phase 4: Test Preparation
1. Quiz on key concepts
2. Review lab implementations
3. Practice answering conceptual questions
4. Prepare for "big picture" questions about distributed systems

## Priority Topics for Test
1. **Interprocess coordination** (etcd, Zookeeper) - linked to TDE project
2. **CORBA fundamentals** (IDL, IOR types, Stubs/Skeleton)
3. **WebServices comparison** (SOAP vs REST, when to use each)
4. **CRUD operations** and REST principles
5. **Socket fundamentals** (4 connection values: client IP/port, server IP/port)
6. **Lab implementations** - be ready to explain project decisions

## Notes
- Student mentions PDFs contain both text and images (diagrams)
- Token limit concerns for reading all PDFs directly
- Strategy: Extract and filter content first, then analyze systematically
- Professor emphasized labs are important for understanding
