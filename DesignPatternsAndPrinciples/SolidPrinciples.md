## Single Responsibility Principle 
A class should have only one reason to change, one responsibility. If a class holds multiple responsibilities, a change in one responsibility forces edits to the same class as a change in an unrelated responsibility, increasing the risk of breaking things that had nothing to do with the actual change.
## Open/Closed Principle 
A class should be open for extension but closed for modification, new behaviour is added without changing existing code. If adding a new variant requires editing an existing, already-working class, that class becomes riskier to touch every time the system grows, and existing behaviour can break as a side effect.
## Liskov Substitution Principle 
Objects of a subclass should be replaceable wherever the base type is expected, without breaking correctness. If a subtype doesn't honor the base type's contract, code using the base type has to add special checks for specific subtypes, defeating the point of polymorphism.
## Interface Segregation Principle 
Clients shouldn't be forced to depend on methods they don't use, interfaces should be small and specific to what each client actually needs. A bloated interface forces every implementer to define methods irrelevant to it, leading to empty or dummy implementations and unnecessary coupling to unrelated behaviour.
## Dependency Inversion Principle 
High-level modules shouldn't depend on low-level concrete details, both should depend on abstractions. If a class depends directly on a specific concrete implementation, swapping that implementation or adding a new variant means editing the class itself, coupling it tightly to details it shouldn't need to know about.

[[DesignPatternsAndPrinciples]]