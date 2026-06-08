# What Is an Architectural Style?
#exam 填空题
Architecture Style =  **Component**/Connector vocabulary, **Topology**, Semantic **Constraints** 
# Five Classical Style Families
## Data Flow
### Batch Sequential
Read All Records ➡ Process All ➡ Generate Outputs

Characteristics 
- **Components**: independent programs 
- **Connectors**: files / media 
- Entire dataset per step 
- No concurrency 
- High latency, simple logic

### Pipe-and-Filter
Source ➡ Filter A ➡ Filter B ➡ Sink

Filter Operations 
- Enrich — Add information 
- Refine — Remove / condense 
- Transform — Change format 
- Decompose — Split stream 
- Merge — Combine streams 
Filter Types: 
- Active (pull/push data) 
- Passive (data-driven)
Advantages 
- High cohesion, low coupling 
- Filters are reusable across pipelines 
- Supports concurrent execution 
- Easy to extend with new filters
Disadvantages 
- Not suitable for interactive applications 
- Format conversion overhead between filters 
- Not appropriate for large shared data
### Batch Sequential vs Pipe-and-Filter
#exam 

| Criterion     | Batch Sequential        | Pipe-and-Filter            |
| ------------- | ----------------------- | -------------------------- |
| Granularity   | Total (coarse, 粗粒度)     | Incremental (fine, 细粒度)    |
| Latency       | High latency (高延迟)      | Can be real-time (可实时处理)   |
| Concurrency   | No concurrency possible | Concurrency possible (可并发) |
| Data Transfer | Whole dataset (整体传输)    | Stream/tokens (流式传输)       |
## Event System
### Dispatch, Observer & Pros/Cons
Implicit Invocation 
- Publishers don't know subscribers 
- One-to-many communication
- Event-based trigger 
- Asynchronous by nature
Advantages 
- Decoupled functionality — separate concerns 
- Easy replacement and addition of handlers 
- Fault isolation — one handler fails, others continue 
- High reuse potential across systems
Disadvantages 
- Dispatcher: SPOF, bottleneck, simultaneous inputs 
- No guarantee event will be treated 
- Components give up control of computation 
- Correctness harder to guarantee (event storms)
### P2P vs Pub/Sub
#exam 
Point-to-Point (Message Queue)一个消息只发给一个人
- Only **ONE consumer** receives each message 
- Message **deleted after consumption** 
- e.g. Meituan: one rider per order

Publish-Subscribe (**Topic**)一个消息会发给所有的订阅者
- ALL subscribers receive every message 
- Message **deleted on expiry**, not consumption 
- e.g. stock price feeds to all traders

股票交易平台要同时用两种方式
股票价格：用 Pub/Sub，因为价格变化应该让所有交易者都知道。
买卖订单：用 P2P，因为一个买入订单只能由一个 broker 执行
## Call/Return
### Main/Sub & Object-Oriented
主程序调用子程序，Object调用method
Main/Subroutine Style 
Hierarchical decomposition through procedure calls 
- Single thread of control, call-and-return semantics 
- Components: procedures + visible data 
- Connectors: procedure calls + data sharing 
- Parnas: each module should hide secrets

Object-Oriented Style 
- Encapsulation — restrict internal state access 
- Interaction — via method calls (message passing) 
- Polymorphism — runtime method dispatch 
- Inheritance — shared functionality

Four OO Problems at Scale 
- God Object — managing too many objects creates a vast "sea of objects" needing hierarchical structuring 
- Excessive Coupling — single interface becomes limiting; many interactions are hard to manage 
- Distributed Difficulty — OO objects hard to distribute across nodes; behavior is spread across many objects 
- Message Passing Overhead — method invocation overhead at scale; types/classes not enough for design families
### Layered Architecture
#exam 
Application Layer 
Middleware Layer
Operating System
Hardware

Each layer serves the one **above**

Advantages 
- **High cohesion** within layers 
- **Implementation hiding** 
- **Coupling constrained** to adjacent layers 
- Layers **replaceable** independently 
**Variants** & Related 
- Strict: **use** only layer directly **below** 
- Relaxed: use any layer below (better perf) 
- **Client/Server: 2-tier → 3-tier → B/S** 
- **Clean Arch / Hexagonal / Ports & Adapters** 
- **Enterprise AI: Infra > Data > Model > App**
### C/S Evolution: 2-Tier → 3-Tier → B/S
#exam 
#### 2-Tier C/S 
Thick Client, Thin Server 
- **Client** handles **UI** + business **logic** 
- **Server** handles **data storage** only 
**Limitations**: 
- **Bloated** client software 
- **Direct DB** access → security **risk** 
- Maintenance nightmare: **update every client** 
- **Different UI styles**, complex usage
#### 3-Tier C/S
Thin Client + App Server 
- Presentation tier (**UI**) 
- Application tier (**logic**) 
- Data tier (**DB**) 
**Advantages**: 
- Better **performance** & scalability 
- Centralized **security** 
- **Parallel development** of tiers
#### B/S (Browser/Server)
Special case of 3-Tier (浏览器 Browser → Web/App Server → 数据库)
- **Client** = HTTP browser 
- Standard HTTP/HTTPS **protocol** 
- **No client installation** needed 
Trade-offs: 
- Can **only "pull,"** not "push" 
- **Security harder** to control (SQL injection) 
- **Server load heavy**, client resources wasted


## MVC: Model-View-Controller
#exam 
Three Components 
- Model (模型): Data + business logic; **notifies View** of state changes 
- View (视图): Presentation/UI; **renders data** from Model 
- Controller (控制器): **Handles user input**; updates Model, selects View 
Core principle: **Separation of Concerns** (关注点分离)

Style **Family Connection** 
- Call/Return: **Controller calls Model** methods 
- **Layered**: View → Controller → Model layers 
- Observer: **Model notifies View** of changes

MVC enables independent UI evolution, parallel development, and testability

## Data-Centered
### Repository & Blackboard
Repository Pattern 
- **Central data structure** + independent **components** 
- Two trigger types: Database (input transactions) vs Blackboard (data state) 
e.g. Windows Registry, IDEs, Redis, Git 

Blackboard Pattern 
- When no algorithm exists — KS（Knowledge Sources） collaborate via shared state 
- Controller **monitors state**, selects knowledge source 
- Condition-action: "if X, I can add Y" 
e.g. HEARSAY-II, LLM agent architectures
## Virtual Machine
### Interpreter & Rule-Based
4 essential parts:
- Execution engine (state machine) 
- Program being interpreted 
- Current state of engine 
- Current state of program

Rule-Based System 
- "Program" is a set of rules 
- Inference engine matches facts vs rules 
- Knowledge Base + Working Memory 
- Cycle: match → select → fire → repeat

Advantages:
- Portability, flexibility, simulate non-native functionality, run code anywhere
Disadvantages:
- Slower than native code, added complexity, rule conflicts at scale
# Classical Styles
| Style              | Key Components       | Key Connectors     | Best For                    |
|-------------------|-------------------|-----------------|-----------------------------|
| Batch Sequential   | Programs           | Files/media      | Overnight batch, ETL        |
| Pipe-and-Filter    | Filters            | Pipes (streams)  | Compilers, data pipelines   |
| Event System       | Sources/handlers   | Event bindings   | GUIs, reactive systems      |
| Main/Sub & OO      | Procedures/objects | Calls/methods    | Business logic, CRUD        |
| Layered            | Layer composites   | Restricted calls | Service hierarchies         |
| Client/Server      | Clients/servers    | Network protocols| Distributed multi-user      |
| Repository         | Central data+comp  | Data access      | IDEs, databases             |
| Blackboard         | BB, KS, controller | Read/write       | AI, speech, diagnosis       |
| Interpreter        | Engine + memories  | Data access      | JVM, Wasm, Docker           |
| Rule-Based         | KB, inference eng  | Rule matching    | Expert systems, OPA         |

# Modern Style
| Modern Style        | Key Components          | Key Connectors              | Best For                     |
|--------------------|-----------------------|----------------------------|-------------------------------|
| Microservices       | Independent services  | HTTP/gRPC, messaging       | Large-scale web apps          |
| Serverless/FaaS     | Functions             | Event triggers             | Event-driven, variable load   |
| Event Sourcing      | Event store, projections | Event streams           | Audit trails, finance         |
| Agent-Oriented      | Agents, tools, memory | Perception/action          | AI reasoning tasks            |
| Modular Monolith    | Bounded modules       | In-process calls           | New projects, small teams     |
| Cell-Based          | Self-contained cells  | Cell gateway routing       | Blast radius, multi-region    |
| Edge Computing      | Cloud/edge/device tiers | Sync, edge functions     | IoT, AR/VR, low latency      |
# Decision Framework for Choosing Styles
1. Dominant Force? Data flow → Pipe/Batch | Events → Pub/Sub | Request-response → Call/Return | Shared state → Repository | Portability → VM | AI reasoning → Agent 
2. Scale & Team Size? <10 devs → Modular Monolith | 10-50 → Microservices (selective) | 50+ → Cell-Based + Microservices 
3. Latency & Consistency? Sub-ms → In-process/edge | Strong consistency → ACID+Call/Return | Eventual OK → CQRS/Event-driven | Offline → Edge 
4. Operational Capability? Low DevOps → Monolith/Serverless | Medium → Microservices+Mesh | High → Cell-Based multi-region 
5. How Will It Evolve? Scale → Modular boundaries | Integrations → Event-driven | Regulation → Event Sourcing | AI → Agent patterns
# Summary
| Style                                   | Components                                                                                     | Connectors                                          | Strengths (Advantages)                                                                                                                                                                            | Weaknesses (Disadvantages)                                                                                                                                                                               |
| --------------------------------------- | ---------------------------------------------------------------------------------------------- | --------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Batch Sequential                        | Independent programs                                                                           | Files / Media                                       | Simple logic                                                                                                                                                                                      | **High latency**; **No concurrency**; Whole dataset                                                                                                                                                      |
| Pipe-and-Filter                         | Filters                                                                                        | Pipes (streams)                                     | **High cohesion**, **low coupling**; Filters are **reusable** across pipelines; Supports **concurrent** execution; **Easy to extend** with new filters; Can be **real-time**; **Stream**/tokens   | Not suitable for **interactive** applications; **Format conversion overhead** between filters; Not appropriate for **large shared data**                                                                 |
| Event System (Observer / Pub-Sub / P2P) | Sources / Handlers; Observable / Observer                                                      | Event bindings                                      | **Decoupled functionality** – separate concerns; **Easy replacement** and addition of handlers; **Fault isolation** – one handler fails, others continue; **High reuse** potential across systems | Dispatcher: **SPOF**, bottleneck, simultaneous inputs; **No guarantee** **event** will be **treated**; Components **give up control** of computation; Correctness harder to guarantee (**event storms**) |
| Main/Subroutine                         | Procedures + visible data                                                                      | Procedure calls + data sharing                      | Single thread of control; Call-and-return semantics; Modules hide secrets                                                                                                                         | Not explicitly stated; large systems require additional structuring                                                                                                                                      |
| Object-Oriented                         | Objects                                                                                        | Method calls (message passing)                      | Encapsulation; Interaction; Polymorphism; Inheritance                                                                                                                                             | God Object; Excessive Coupling; Distributed Difficulty; Message Passing Overhead                                                                                                                         |
| Layered                                 | Layer composites                                                                               | Restricted calls                                    | **High cohesion** within layers; **Implementation hiding**; **Coupling constrained** to adjacent layers; **Layers replaceable** independently                                                     | Strict layering may **reduce performance**                                                                                                                                                               |
| MVC                                     | Model, View, Controller                                                                        | Controller calls Model methods; Model notifies View | **Separation of Concerns**; Independent **UI evolution**; **Parallel development**; **Testability**                                                                                               | Not explicitly stated                                                                                                                                                                                    |
| Client/Server                           | Clients / Servers                                                                              | Network protocols                                   | Better performance & scalability; Centralized security; Parallel development of tiers                                                                                                             | Bloated client software (2-tier); Direct DB access → security risk; Maintenance nightmare; Server load heavy; Security harder to control                                                                 |
| Repository                              | Central data structure + independent components                                                | Data access                                         | Central data structure; Independent components                                                                                                                                                    | Data format change affects all modules; Hard to enhance or reuse                                                                                                                                         |
| Blackboard                              | Blackboard, Controller, Knowledge Sources                                                      | Read / Write shared state                           | Useful when no algorithm exists; Knowledge sources collaborate through shared state                                                                                                               | Not explicitly stated                                                                                                                                                                                    |
| Interpreter                             | Execution engine; Program being interpreted; Current state of engine; Current state of program | Data access                                         | Portability; Flexibility; Simulate non-native functionality; Run code anywhere                                                                                                                    | Slower than native code; Added complexity                                                                                                                                                                |
| Rule-Based System                       | Knowledge Base; Working Memory; Inference Engine                                               | Rule matching                                       | Flexible reasoning; Knowledge-driven behavior                                                                                                                                                     | Rule conflicts at scale; Slower execution                                                                                                                                                                |
