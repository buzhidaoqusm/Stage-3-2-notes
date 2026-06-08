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
A concern = any piece of interest or focus
Separate concerns -> independent change

Information Hiding: Each module hides a design decision
Public interface reveals WHAT, hides HOW

How They Connect:
SoC decides WHERE to draw boundaries. Information Hiding decides WHAT to expose at each boundary
