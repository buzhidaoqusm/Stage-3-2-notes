# Why Describe Architecture?
Communication: Stakeholders understand design decisions 
Forward Engineering: Generate code skeletons from models 
Reverse Engineering: Extract architecture from source code 
Blueprint & Guide: Defines work distribution and quality carrier 
Evaluation: Basis for early analysis and conformance

# Scenario View

![](assets/3%20Architecture%20Description/file-20260608193605143.png)
# Logical View
## Class diagram
#exam 要考relationship（线条的关系）
![](assets/3%20Architecture%20Description/file-20260614181127078.png)
![](assets/3%20Architecture%20Description/file-20260608193911986.png)
![](assets/3%20Architecture%20Description/file-20260608194551403.png)

![](assets/3%20Architecture%20Description/file-20260608194551420.png)
Dependency
- 一个对象将另一个对象作为字段/属性
Aggregation (implies “has-a” or “part-whole”)
- Team ◇── Player（空心菱形连在弱整体-部分，球员不因为球队解散就“消失”）
Composition (implies “ownership”)
- Car ◼── Engine(实心菱形连在强所有权的一方。车没了，发动机也没了)
## Sequence diagram
![](assets/3%20Architecture%20Description/file-20260608194728529.png)
## State Diagram
![](assets/3%20Architecture%20Description/file-20260608194819436.png)

# Process View
## Activity diagram
![](assets/3%20Architecture%20Description/file-20260608194845880.png)
![](assets/3%20Architecture%20Description/file-20260608194922986.png)
# Development View
## Package/Component diagram

# Physical View
## Deployment diagram

# Summary
| View          | Question            | UML Diagrams                | Stakeholder    | Focus              |
| ------------- | ------------------- | --------------------------- | -------------- | ------------------ |
| Scenario (+1) | What does it do?    | Use Case                    | Users / All    | Ties all views     |
| Logical       | What are the parts? | Class, Object, State, Seq., | Developers     | System composition |
| Process       | What’s concurrent?  | Activity                    | Sys. Engineers | Concurrency & flow |
| Development   | How organized?      | Package, Component          | Managers       | Team & reuse       |
| Physical      | Where does it run?  | Deployment                  | Operations     | Hardware topology  |