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