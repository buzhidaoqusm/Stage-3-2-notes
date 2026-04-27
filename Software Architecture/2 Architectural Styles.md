# What Is an Architectural Style?
Architecture Style =  Component/Connector vocabulary, Topology, Semantic Constraints 
# Five Classical Style Families
## Data Flow
### Batch Sequential
Read All Records ➡ Process All ➡ Generate Outputs

Characteristics 
- Components: independent programs 
- Connectors: files / media 
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
### Batch Sequential vs Pipe-and-Filter
#exam 

| Criterion | Batch Sequential | Pipe-and-Filter |
|---|---|---|
| Granularity | Total (coarse, 粗粒度) | Incremental (fine, 细粒度) |
| Latency | High latency (高延迟) | Can be real-time (可实时处理) |
| Concurrency | No concurrency possible | Concurrency possible (可并发) |
| Data Transfer | Whole dataset (整体传输) | Stream/tokens (流式传输) |
## Event System
### Dispatch, Observer & Pros/Cons
Implicit Invocation 
- Publishers don't know subscribers 
- One-to-many communication
- Event-based trigger 
- Asynchronous by nature

| 类型              | 有没有中间调度器(Dispatcher) | 典型形式             |
| --------------- | -------------------- | ---------------- |
| No Dispatcher   | 没有                   | Observer Pattern |
| With Dispatcher | 有                    | P2P / Pub/Sub    |
### P2P vs Pub/Sub
#exam 
Point-to-Point (Message Queue)
- Only **ONE consumer** receives each message 
- **Message deleted** after consumption 
- e.g. Meituan: one rider per order

Publish-Subscribe (Topic)
- ALL subscribers receive every message 
- Message **deleted on expiry**, not consumption 
- e.g. stock price feeds to all traders
## Call/Return
### Layered Architecture
#exam 
Application Layer 
Middleware Layer
Operating System
Hardware

Advantages 
- **High cohesion** within layers 
- **Implementation hiding** 
- **Coupling constrained** to adjacent layers 
- Layers **replaceable** independently 
**Variants** & Related 
- Strict: use only layer directly below 
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

Style Family Connection 
- Call/Return: **Controller calls Model** methods 
- **Layered**: View → Controller → Model layers 
- Observer: **Model notifies View** of changes

## Data-Centered
### Repository & Blackboard
Repository Pattern 
- Central data structure + independent components 
- Two trigger types: Database (input transactions) vs Blackboard (data state) 
e.g. Windows Registry, IDEs, Redis, Git 
Blackboard Pattern 
- When no algorithm exists — KS collaborate via shared state 
- Controller monitors state, selects knowledge source 
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