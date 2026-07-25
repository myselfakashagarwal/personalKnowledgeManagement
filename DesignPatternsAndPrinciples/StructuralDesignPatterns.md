
| Pattern   | Corrected Summary (lightweight)                                                                                                  |
| --------- | -------------------------------------------------------------------------------------------------------------------------------- |
| Adapter   | Wraps an incompatible object to expose the interface a client expects, without modifying the original.                           |
| Bridge    | Splits a class into independent parts connected through a common interface, so each part can change without affecting the other. |
| Composite | Treats a single object and a group of objects the same way through one shared interface, allowing them to nest recursively.      |
| Decorator | Adds behaviour to an object at runtime by wrapping it in another object that shares the same supertype.                          |
| Facade    | Exposes a single simplified interface over a complex subsystem of multiple objects.                                              |
| Flyweight | Shares immutable, reusable instances across multiple clients to avoid duplicate object creation.                                 |
| Proxy     | Controls, restricts, or enhances access to an object while exposing the same interface as that object.                           |
## Adapter
A wrapper over pre-existing code without actual modifications, making it fit for needs without changing its original identity or implementation.

Objects that need to be used but cannot be used directly because they are incompatible, and also cannot be modified because doing so might break other components that depend on them, can be integrated by wrapping its functionality with/without extensions to fit for needs.

Consider a scenario where a `RemoveButton` needs to be used as a `CancelButton` in a pop-up window. Both buttons have the same functional behaviour: clicking either button closes the window. However, the `RemoveButton` cannot be used directly because its interface or expected role differs from that of a `CancelButton`. In this case, an Adapter can wrap the `RemoveButton` and expose the `CancelButton` interface. The Adapter translates the `CancelButton` requests into `RemoveButton` operations, allowing the existing `RemoveButton` implementation to be reused without modification.
## Bridge
A decoupler to divide a class into multiple sub units to advance independently though interacting using a bridge without exposing the implementation of the core.

Having a large class with multiple interrelated functionalities has maintenance overhead as changes might be dependable, so division for sub related behaviour is done to advance them independently.

An `Element` can represent multiple types of UI components, such as a `Button` or an `Input`. Since these component families need to evolve independently, `Element` is defined as an interface that establishes the common contract without defining the core behaviour. Each component family, such as `AbstractButton` and `AbstractInput`, implements this contract with behaviour specific to its own type. This separation allows each hierarchy to evolve independently while ensuring that all UI components conform to a common abstraction.
## Composite
An object can be composed of multiple sub-objects that together behave as a single logical entity. By treating both individual objects and groups of objects through the same interface, clients can interact with the entire structure uniformly.

Using inheritance to acquire multiple behaviours introduces unnecessary coupling. A subclass inherits not only behaviour but also the identity and responsibilities of its parent, even when it requires only a subset of that functionality or a different implementation. Composition avoids this problem by allowing an object to retain its own identity while collaborating with other objects to reuse only the behaviour it needs. Instead of inheriting everything, it borrows only what is necessary.

A `Div` can contain multiple `Element` objects, including other `Div` objects. Since a `Div` can contain instances of itself, it forms a recursive tree structure. This recursive composition allows a hierarchy of elements to be represented and treated as a single `Element`.
## Decorator
To add functionality to an object at runtime, a decorator may change the concrete runtime type of the object, but it preserves its supertype (the abstraction) so the client can continue interacting with it transparently.

When an object's functionality needs to be extended while preserving its supertype, a wrapper that implements the same supertype can be used to add the required behaviour. The wrapper delegates operations to the original object while executing additional logic before, after, or around those operations, allowing functionality to be extended without modifying the original implementation.

Consider a `Button` implementing the `Element` interface. If the button needs additional behaviour, such as displaying a loading spinner during rendering, creating a `LoadingDecorator` that also implements `Element` allows the new behaviour to be added without modifying the original `Button`. The `LoadingDecorator` wraps the `Button`, delegates the `render()` operation to it, and adds the loading behaviour. Since both the `Button` and the `LoadingDecorator` implement `Element`, the client continues to use the object through the same `Element` interface.
## Facade
Provide a single, simplified interface to a complex subsystem by encapsulating the interactions with multiple objects behind one entry point.

Imagine a scenario where a complex internal library needs to be utilised, however it's too complex to work with, including inconsistencies in naming convention and no self documenting code. A new wrapper can be used to hide this complexity by providing a less painful, predictable interface to work with.

A simple example of the Facade pattern is a `display()` method. Displaying a page may require multiple operations such as rendering the UI, applying animations, and registering event listeners. Instead of requiring the client to invoke each operation individually, a `display()` method encapsulates these calls and exposes a single operation. The client interacts only with `display()`, while the facade coordinates the underlying subsystem operations internally.
## Flyweight
A Flyweight is a registry of shared, immutable objects that are reused across multiple clients to avoid creating duplicate instances and reduce memory consumption.

When the same object is used repeatedly and its state is immutable, it can be stored and reused instead of creating a new instance each time. Sharing such objects reduces object creation overhead and memory consumption.

In the `Element` hierarchy, `Style` objects are shared subcomponents whose state remains unchanged after creation. Since multiple elements can use the same style, an `ElementFactory` can retrieve shared `Style` instances from a Flyweight registry instead of creating duplicate style objects for every element.
## Proxy
A Proxy is a layer of interaction between a client and an object that controls, enhances, or restricts access to the underlying object while exposing the same interface as that object.

When direct access to an object needs to be controlled, such as delaying creation until needed, checking permissions, or logging calls, a proxy is placed in front of the real object. The client calls the proxy exactly as it would call the real object, unaware that the proxy is intercepting the call.

Consider a `StyleFactory` where loading themed styles from disk is expensive. A `StyleFactoryProxy` implements the same interface as `StyleFactory`, but only creates the real `StyleFactory` instance the first time a style is actually requested, caching it for subsequent calls. The client calls `proxy.getButtonStyle(...)` exactly as it would call the real factory, without knowing the actual loading was deferred and controlled by the proxy.