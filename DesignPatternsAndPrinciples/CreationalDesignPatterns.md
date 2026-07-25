| Pattern          | Corrected Summary (lightweight)                                                                                      |
| ---------------- | -------------------------------------------------------------------------------------------------------------------- |
| Factory          | Separates the object creation responsibility for objects of the same type.                                           |
| Abstract Factory | Separates the object creation responsibility for objects from different families that share the same trait.          |
| Prototype        | Creates new objects by cloning a preconfigured prototypical instance, instead of constructing each one from scratch. |
| Builder          | Separates the object configuration logic to create objects with different configurations.                            |
| Singleton        | Shares a single universal, central, or shared instance of an object.                                                 |
## Factory 
A simple factory separates object creation from object usage by implementing a class/object solely responsible for object creation.

Consider two services, `S1` and `S2`, that both instantiate the same object `O1` directly. If the implementation or constructor of `O1` changes, both services must be modified. As the number of services and shared objects increases, maintaining object creation logic across the codebase becomes difficult and error prone.

A factory centralizes object creation. Both `S1` and `S2` request `O1` from the factory instead of creating it directly. If the implementation of `O1` changes, only the factory needs to be updated, while the service classes remain unchanged. This reduces code duplication, lowers coupling, and improves maintainability.

When a new service is introduced, it simply requests the required objects from the factory without implementing object creation logic itself. The service remains focused solely on its business functionality.

In this codebase, `ButtonFactory` is responsible for creating different button objects. It accepts parameters such as the button type and theme, and returns the appropriate concrete button instance. Client code requests a button from the factory without knowing or depending on its concrete implementation.

Variants : Objects for stateful, Class for stateless
## Abstract Factory 
A centralised  creation of objects of different domains but with same trait. 

When a system contains multiple families of objects, a single factory becomes difficult to maintain because it must handle object creation for every domain. Any change in one domain requires modifications to the same factory, violating the Open Closed Principle. Creating separate factories for each domain improves modularity but removes the ability to create a consistent family of related objects through a single entry point.

An Abstract Factory addresses this by introducing a factory of factories. Each concrete factory is responsible for creating a complete family of related objects. The client selects the required factory, which then produces all objects belonging to that family while hiding the concrete implementations.

`ButtonFactory`, `InputFactory`, `TextFactory`, and `ContainerFactory` are specialized factories responsible for creating objects within their respective domains. `ElementFactory` acts as an abstraction over these factories by providing a single entry point for UI element creation. Instead of interacting with individual factories, the client requests an element from `ElementFactory`, which delegates the request to the appropriate specialized factory. This centralizes access while keeping the creation logic for each domain modular and independent.

Variants : Objects for stateful, Class for stateless 
## Prototype 
A mechanism to create new objects by cloning a preconfigured prototypical instance, instead of constructing each one from scratch.

The Prototype Pattern is useful when object creation is repetitive and most of the object's configuration remains the same. Instead of constructing each object from scratch, a preconfigured prototype is cloned and only the required differences are modified. Each clone is a separate object with its own state, ensuring that neither the object nor its mutable state is shared with the prototype or other clones.

A base `AddButton` is maintained as a prototype. Whenever a new themed button is required, the prototype is cloned and its `Style` object is replaced with the desired implementation, such as `RetroStyle` or `MinimalistStyle`. The cloned buttons inherit the common configuration of the prototype while remaining independent objects with different visual styles.

Aspects: New separate different copy, no shared state.
## Builder
Builder pattern separates the construction process from the object's representation, allowing the same construction process to create different configurations of an object.

When an object contains many optional or configurable attributes. Instead of using multiple constructors or long parameter lists, a builder incrementally configures the object before producing the final instance.

Consider creating an `AddButton` with configurable properties such as style, width, height, text, icon, tooltip, and interaction. Rather than exposing multiple constructors, an `AddButtonBuilder` configures each property individually before building the final object.

```
AddButton button = new AddButtonBuilder()
    .setStyle(new AddButtonRetroStyle())
    .setText("Create")
    // more and more 
    .build();
```

The builder constructs the `AddButton` step by step and returns a fully configured object, while hiding the complexity of the construction process.
## Singleton 
Singleton aims to be a single global instance which can be used as an entry point or a centrepiece to work with. 

Objects that must have a single shared instance, such as configuration managers, loggers, caches, or application settings. It prevents multiple instances from being created and ensures consistent access across the application.

Consider a `StyleFactory` used throughout the application to provide UI styles. Since every UI element requests styles from the same source, only one instance of `StyleFactory` is required.

```
StyleFactory factory = StyleFactory.getInstance();

Style retro = factory.getButtonStyle("add", "retro");
Style minimalist = factory.getInputStyle("text", "minimalist");
```

All `Button`, `Input`, `Container`, and `Text` elements obtain their styles from this single `StyleFactory` instance, ensuring centralized style management while avoiding multiple factory instances.
