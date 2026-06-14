# Design Smells
- "Rigidity: Change cascades to many modules."
- "Fragility: Breaks in unrelated places."
- "Immobility: Can't extract for reuse."
- "Viscosity: Right way harder than hacks."
- "Opacity: Code hard to read/understand."
- "Needless Complexity: Over-engineering for hypothetical futures."
- "Needless Repetition: Copy-paste instead of abstraction."

# Separation of Concerns & Information Hiding
Separation of Concerns: Each part addresses a separate concern
A concern = any piece of **interest** or **focus**
Separate concerns -> **independent change**
MVC, layered体现了

Information Hiding: Each module hides a design decision
Hidden info = **likely change point**
Public interface **reveals WHAT**, hides HOW
Change HOW **without affecting clients**
Info Hiding = PRINCIPLE
Encapsulation = MECHANISM

How They Connect:
SoC decides WHERE to draw boundaries. Information Hiding decides WHAT to expose at each boundary

# Coupling and Cohesion
Coupling: Degree of interdependence between modules. Goal: LOW
Program to an interface, not an implementation

Cohesion: How well parts of a module work together. Goal:HIGH
Each module should have a clear, focused purpose

Low coupling + High cohesion = Well-designed module
# SOLID
## S — Single Responsibility Principle
一个类一个职责
A class should have only one reason to change.
SRP reduces change ripple by isolating responsibilities that evolve independently
## O — Open Closed Principle
画图形：需要画一个新的形状时，只需Implement shape。而不用if shape == XX
Open for extension, closed for modification
![](assets/5%20Architectural%20Design%20Principles/file-20260608220629294.png)
No if/switch to maintain (Rigidity ✓). No scattered type-checks (Fragility ✓). Each shape self-contained (Immobility ✓). Eliminates Opacity & Repetition
## L — Liskov Substitution Principle
子类需要可以被父类完全替换
Subtypes must be substitutable for their base types.

![](assets/5%20Architectural%20Design%20Principles/file-20260608220755462.png)

## I — Interface Segregation Principle
一个接口不能太大，同时包含很多东西
Clients should not depend on methods they do not use.
![](assets/5%20Architectural%20Design%20Principles/file-20260608221408803.png)

## D — Dependency Inversion Principle
高层使用低层  ->  高层继承接口，接口使用低层
High-level modules should not depend on low-level modules. Both depend on abstractions.

![](assets/5%20Architectural%20Design%20Principles/file-20260608221736530.png)
# Law of Demeter
"Each unit should only talk to its immediate friends." (Principle of Least Knowledge)
A Method May Only Call:
1. Its own methods 
2. Its parameters' methods 
3. Objects it creates internally 
4. Its direct component objects

X: customer.getOrder().getItem().getPrice()    -  > (Reaching through 3 objects)
√: customer.getOrderTotal()
