# Quality Attribute Scenarios
- "**Source of Stimulus**: Who/what causes it."
- "**Stimulus**: The specific event."
- "**Artifact**: System part affected."
- "**Environment**: System state."
- "**Response**: What system does."
- "**Response** **Measure**: Quantifiable metric."
![](assets/4%20Quality%20Attribute/file-20260608211054669.png)
# Availability
- "Availability: **Fault Detection, Fault Recovery, Fault Avoidance**."
- **Detection**（检测问题）: "Ping/Echo, Heartbeat, Exceptions."
- **Recovery**（从问题中恢复）: "Voting, Active/Passive Redundancy, Spare, Checkpoint."
- **Avoidance**（避免问题）: "Service removal, Transactions, Process monitor."
- "Five 9s."
- "SLI, SLO, SLA." service level indicator/objctive/agreement
# Modifiability
**Limit Scope** of Modification（把可能变化的部分隔离开来，让修改只影响很小的一部分系统）
- **Reduce Coupling**: Use intermediary, Restrict dependencies (Facade) 
- **Encapsulate**: Info hiding, Maintain existing interfaces 
- Increase **Cohesion**: Semantic coherence, Abstract common services 
- **Name Server** (名称服务器): resolve location dependencies
Delay Binding Time（系统什么时候确定某些设计/实现的具体细节，配置文件yaml）
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

**Resource Demand**（系统需要处理的事件或操作量）
- Efficient algorithms（On^2 -> On)
**Resource Management**（系统如何分配和使用计算资源，以支持高并发和负载）
- Concurrency (multi-thread/core) 
- Increase resources (vertical/horiz.) 
- Maintain multiple data copies 
- Cloud auto-scaling, CDN
**Resource Arbitration**（当多个任务或请求争用同一资源时，系统如何决定处理顺序）
- FIFO (first in first out) 
- Fixed priority (military lanes) 
- Dynamic priority (no starvation)
- Earliest deadline first (ER triage)
# Security
CIA Triad (CIA三元组) — **Confidentiality, Integrity, Availability**

**Non-repudiation**: No one can deny actions 
**Confidentiality**: Authorized access only ← CIA 
**Integrity**: Data not tampered ← CIA 
**Assurance**: Info reaches destination 
**Availability**: Survives attacks ← CIA 
**Auditing**: Actions logged

**Resist Attacks**（抵御攻击）: **Authentication, Authorization**, Data confidentiality (HTTPS/TLS), Data integrity
**Detect Attacks**（检测攻击）: Intrusion detection (IDS/IPS), SIEM, **continuous monitoring**
**Recover from** Attacks（从攻击中恢复）: Restore state, Attacker identification, Forensic analysis
# Usability
Onboarding: How quickly new users learn 
Efficiency: Speed for experienced users 
Error Tolerance: Handles user mistakes 
Adaptability: Users can customize 
Confidence: User feels in control

**Runtime Tactics** （用户在实际使用系统时的体验和交互）
- Anticipate user tasks (auto-complete, suggestions) 
- Provide feedback (progress bars, status) 
- Maintain consistent experience

**Design-Time Tactics** （系统在设计和开发阶段提供的可用性支持）
- Support undo (recycle bin, multi-step undo) 
- MVC: separate Model, View, Controller 
- UI changes don't affect business logic

> Usability Scenario Example (易用性场景示例) 

Source: End user (新用户) 
Stimulus: New user opens app for first time 
Artifact: User interface 
Env: Normal operation 
Response: User completes core task within 5 min without help 
Measure: Time to task completion, error rate
# Testability
Finding Bugs Before Users Do

**Black-Box** Testing （不看系统内部代码怎么写，只看输入和输出是否正确）
- **Record/replay** for regression 
- Separate interfaces from implementation 
- Controlled input → observable output 
**White-Box** Testing （看系统内部代码、逻辑、分支和执行路径，检查里面有没有问题）
- Internal monitoring (IDE debugging) 
- Breakpoints, step-through, inspection 
- Specialized tools (WinDbg, Stetho)
# Summary
![](assets/4%20Quality%20Attribute/file-20260608211255625.png)