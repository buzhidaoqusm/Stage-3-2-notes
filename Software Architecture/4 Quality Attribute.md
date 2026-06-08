# Quality Attribute Scenarios
- "**Source of Stimulus**: Who/what causes it."
- "**Stimulus**: The specific event."
- "**Artifact**: System part affected."
- "**Environment**: System state."
- "**Response**: What system does."
- "**Response** Measure: Quantifiable metric."
# Availability
- "Availability: **Fault Detection, Fault Recovery, Fault Avoidance**."
- Detection: "Ping/Echo, Heartbeat, Exceptions."
- Recovery: "Voting, Active/Passive Redundancy, Spare, Checkpoint."
- Avoidance: "Service removal, Transactions, Process monitor."
- "Five 9s."
- "SLI, SLO, SLA." service level indicator/objctive/agreement
# Modifiability
Limit Scope of Modification
- **Reduce Coupling**: Use intermediary, Restrict dependencies (Facade) 
- **Encapsulate**: Info hiding, Maintain existing interfaces 
- Increase **Cohesion**: Semantic coherence, Abstract common services 
- **Name Server** (名称服务器): resolve location dependencies
Delay Binding Time
- **Runtime config**: Configuration files (XML, .ini), Feature flags 
- Component **replacement**, Polymorphism, Runtime registration 
- On-**Demand Loading** (按需加载): create instances only when needed 
- **Publish-subscribe**, Interpreter, Name server
# Performance
Response Measures (响应指标) 
- Time to process (search in 0.63s) 
- Events/unit time — TPS (540K txn/sec) 
- Error/loss rate under high load

Response Time vs Throughput trade-off - optimizing one may degrade the other
Processing time = Acquiring resources + Using resources
Resource Demand; Resource Management; Resource Arbitration
# Security
CIA Triad (CIA三元组) — Confidentiality, Integrity, Availability

**Non-repudiation**: No one can deny actions 
**Confidentiality**: Authorized access only ← CIA 
**Integrity**: Data not tampered ← CIA 
**Assurance**: Info reaches destination 
**Availability**: Survives attacks ← CIA 
**Auditing**: Actions logged

Resist Attacks: Authentication, Authorization, Data confidentiality (HTTPS/TLS), Data integrity
Detect Attacks: Intrusion detection (IDS/IPS), SIEM, continuous monitoring
Recover from Attacks: Restore state, Attacker identification, Forensic analysis
# Usability
Onboarding: How quickly new users learn 
Efficiency: Speed for experienced users 
Error Tolerance: Handles user mistakes 
Adaptability: Users can customize 
Confidence: User feels in control

Runtime Tactics 
- Anticipate user tasks (auto-complete, suggestions) 
- Provide feedback (progress bars, status) 
- Maintain consistent experience

Design-Time Tactics 
- Support undo (recycle bin, multi-step undo) 
- MVC: separate Model, View, Controller 
- UI changes don't affect business logic
# Testability
Finding Bugs Before Users Do

Black-Box Testing 
- Record/replay for regression 
- Separate interfaces from implementation 
- Controlled input → observable output 
White-Box Testing 
- Internal monitoring (IDE debugging) 
- Breakpoints, step-through, inspection 
- Specialized tools (WinDbg, Stetho)