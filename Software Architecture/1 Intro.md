# Why Software Architecture? (产生动因)
#exam
记住 SA 的五个动机。架构的存在是因为大型系统需要管理复杂性、共享沟通、合理的早期决策、可重用的知识和受控的风险。
1. Managing Complexity
	- Large systems exceed any individual's comprehension; architecture provides intellectual control through abstraction.
2. Stakeholder Communication
	- Architecture is the shared language between users, developers, managers, and customers.
3. **Early Design Decisions**
	- Architectural choices constrain all downstream implementation — they are the hardest to change later.
4. Reuse of Architectural Knowledge
	- Proven patterns (MVC, microservices, pipe-and-filter) encode decades of collective experience.
5. Risk Reduction
	- Sound architecture identifies and mitigates technical risks before expensive implementation begins
# SA Definitions: From Formulas to Standards
All definitions agree on **components + relationships**. 
but all agree: architecture = the highest level of abstraction, defining components, relationships, and the principles guiding design.
# Components, Connectors, and Constraints
#exam  三者的区别
- Components: The building blocks.
- Connectors: Interaction mechanisms.
- Constraints: Rules governing composition.
- SA Design = Decomposition + Composition.
# Functional vs. Non-Functional Properties
#exam 为什么架构对于质量属性非常重要：好的架构更容易满足质量属性，分层架构提高可修改性等等。坏架构的紧耦合可能损害可修改性。
Non-Functional (Quality Attributes)
How WELL the system does it
- Performance — response time, throughput 
- Security — data protection, auth 
- Reliability — uptime, fault tolerance 
- Modifiability — ease of change

Architecture has MORE impact on non-functional properties than functional ones
# Framework vs. Architecture
#exam 哪个是架构，而不是coding decision
- "Architecture: Abstract design decisions."
- "Architecture: Defines structure and relationships."
- "Framework: Concrete implementation of architecture."
- "Framework: Language/platform specific."
- "Example: 'MVC pattern' / Example: Spring, Django, .NET."
# Influences on the Architecture
#exam 
memorize the chain 'influences → requirements/qualities → architect → architecture → system'.
