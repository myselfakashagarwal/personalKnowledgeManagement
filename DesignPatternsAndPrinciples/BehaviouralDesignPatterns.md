|Pattern|Corrected Summary (lightweight)|
|---|---|
|Chain of Responsibility|Passes a request along a line of handler objects until one of them handles it, instead of one object processing everything.|
|Command|Wraps a request as an object carrying its own data and logic, so it can be executed through a common interface without the invoker knowing the details.|
|Interpreter|Represents a recursive expression as a tree of nodes, each holding its own evaluation logic, so shared sub-conditions are reused instead of duplicated.|
|Iterator|Visits elements of a collection based on availability, not on a fixed index or ordering mechanism.|
|Mediator|Centralizes interaction logic between objects in one mediator, so objects communicate through it instead of referencing each other directly.|
|Memento|Saves and restores an object's state through a snapshot, without exposing that state's internal details to whoever stores it.|
|Observer|Notifies a list of dependent objects automatically when the subject's state changes, without the subject knowing what they do with it.|
|State|Delegates state specific behaviour to a separate object, swapped when the state changes, instead of branching on status inside one class.|
|Strategy|Encapsulates interchangeable algorithms into objects sharing a common interface, usable without their implementation baked into the calling class.|
|Template|Fixes the overall sequence of steps in a base class, leaving only the varying steps for subclasses to override.|
|Visitor|Moves an operation on a set of classes into a separate visitor object, so new operations can be added without changing the classes themselves.|
## Chain of Responsibility
Instead of having a single object perform all the processing sequentially, the responsibilities are divided among multiple handler objects. A request is passed from one handler to the next, where each handler decides whether to process the request, terminate the chain, or delegate it to the next handler. (The chain is linear)

When the request data determines how it should be processed at runtime, each handler evaluates the request and decides whether it is responsible for handling it. If not, the request is forwarded to the next handler in the chain until it is handled or the end of the chain is reached.

During form validation, the request passes through a chain of validation handlers. Each handler validates one aspect of the form. If a validation fails, the chain terminates immediately and returns the error. If the validation succeeds, the request is forwarded to the next handler until all validations pass.
## Command
The request is converted into an object which carries both data and logic, this object is passed to a common interface which performs the carried logical operation on the carried data.

When a user requests an operation, the request and the data required to perform it are encapsulated into a command object. The command is then passed to an invoker through a common interface, which executes it without knowing the underlying receiver or implementation details.

Imagine a form builder that supports operations such as adding and removing elements. For each request, a command object is created that encapsulates the element data and the logic required to perform the operation. The command is then passed to a common interface, allowing the invoker to execute the request without being coupled to the form or the specific operation.
## Interpreter
Instead of baking the logic into hardcoded branching control flow, a tree of nodes, each carrying its own evaluation logic, is used to check multi-conditional expressions recursively, where nodes can be reused across multiple parents instead of duplicated.

When a condition is recursive and shares the same sub-conditions, baking it into if-else or a decision tree causes hardcoded logic and duplication. Instead, each sub-condition is evaluated by its own node, and the same node can be reused across multiple parents with no duplication.

Two decisions share a condition: A = `c1 AND c2`, B = `c2 AND c3`, final rule = `A OR B`. `c2` is shared by both. Decision tree: `c2` gets checked twice, once in A's branch, once in B's branch, since a node can only have one parent. Change `c2`'s logic, update it in two places. Interpreter: `c2` is built once as a node object, passed as a child into both `A` and `B`. One object, reused by two parents. Change it once, both A and B pick up the change.
## Iterator
Visits elements of a collection based not on an ordering mechanism, but on availability.

When the number of objects in a collection isn't fixed, a stable index can't be defined, so a fixed, final index doesn't apply. In that case only availability matters, along with making sure the same element isn't visited twice.

A tree of elements needs a leaf removed. Instead of indexing into the structure, the iterator traverses each element in turn, if it has no children, it's removed, otherwise traversal moves on to the next.
## Mediator
Instead of hardcoding communication paths between every pair of objects, a single mediator object is used to hold the interaction logic, where objects call the mediator instead of calling each other directly.

When objects talk directly, each one holds references to the others and the interaction logic is scattered across all of them. Changing how one object reacts to another means updating both sides. With a mediator, objects only call `mediator.notify(self, event)`, and the mediator alone decides who reacts to what. Objects hold no references to each other.

A form has a Button and an Input. Button doesn't know Input exists. When clicked, Button tells the mediator "I was clicked." Mediator decides that means "disable the input" and does it. Input doesn't know Button exists either, it just tells the mediator "I changed." Mediator decides that means "update the label."
## Memento
An object (Originator) needs to save a snapshot of itself so it can be restored later, but its internal fields shouldn't be exposed to whoever is doing the saving (Caretaker). So the Originator creates a Memento, a snapshot object, and hands it to the Caretaker. Caretaker holds onto it but can't read or modify it, only passes it back to the Originator when a restore is needed.

Undo/redo, checkpoints, rollback. Keeps the save/restore logic inside the object itself, so the state's internal structure stays private, nothing outside needs to know or depend on how the object represents its own data.

A text editor has content. Before each edit, it saves a memento of the current content onto an undo stack (Caretaker holds the stack). Hit undo, Caretaker hands the last memento back to the Originator, Originator restores its content from it. Caretaker never reads or touches the text inside the memento, it just stores snapshots and returns them in order.
## Observer
The subject holds the list of observers in memory. Observers can be added or removed from that list at runtime. On state change, the subject loops through the list and calls each one via the common interface, notifying all of them.

Baking the list of observers into the code breaks extensibility, but adding or removing them at runtime, and avoiding having observers poll the state constantly, keeps things efficient.

A weather station tracks temperature. A phone display and a website widget both want to know when it changes, so they register into the station's observer list. When temperature changes, the station loops through that list and calls update() on each one, phone and website react on their own, station never calls them by name. Add a car dashboard later, it registers itself, station's code doesn't change.
## State
Behaviour is state specific, so instead of branching on status inside the main object, the behaviour lives in a separate state object. When state changes, that sub-object reference is swapped to the one holding the new behaviour. The main object always calls the same method, whatever state object is currently plugged in decides what actually happens.

Coding all state-specific behaviour inside one class as if-else branches means adding a new state becomes a problem, every existing method has to be reopened and edited to add another branch. Instead, each state's behaviour lives in its own object, the main object just swaps the reference to whichever state object is current, adding a new state means adding a new object, not touching existing ones.

A Button's appearance depends on its current state, normal, hover, disabled, pressed. Instead of render() branching with if-else on the state to decide which style to apply, each state is its own object holding that logic. Button just delegates to its current state object, which applies the right style and sets the next state. Add a new state like loading, add a new state object, Button's render() code never changes.
## Strategy
Algorithms are encapsulated into objects sharing a common interface, these can be used without needing their implementation baked into the class using them.

When a class needs to do the same kind of task in different ways, baking every variant into if-else means the class grows every time a new variant is needed. Instead, each variant is its own strategy object implementing the common interface, the class holds a reference to one and calls it, swapping the reference swaps the behaviour.

An Input needs different validation depending on type, number, date, email. Instead of `if type == "number": ... elif type == "date": ...` inside AbstractInput, each type has its own validation strategy object. AbstractInput just calls `strategy.validate(value)`. Add EmailInput with its own validation strategy, AbstractInput's code never changes.
## Template
The overall sequence of steps is fixed in a base class, only the specific steps that vary are left for subclasses to override, the rest stays written once and shared.

When multiple variants share the same overall steps but differ in a few, letting each subclass rewrite the whole sequence duplicates the shared steps everywhere. Instead, the base class defines the steps in order once, calling out to a few methods left open, subclasses only fill in those specific pieces.

Heading and Paragraph both extend AbstractText, both go through render, apply style, output content. AbstractText defines that sequence once. Heading overrides only the content formatting step, Paragraph overrides it differently, render and apply style stay written once in the base class.
## Visitor
An operation on a set of classes is pulled out into a separate visitor object, each class just accepts the visitor and lets it perform the operation, instead of each class implementing the operation itself.

When new operations need to be added often but the set of classes is stable, adding a method to every class per new operation means touching every class every time. Instead, each class exposes one `accept(visitor)` method, the operation logic lives inside visitor objects, one new visitor per new operation, existing classes untouched.

ElementFactory produces Button, Input, Text elements. Need to export the form to HTML, then later to JSON. Instead of adding `toHtml()` and `toJson()` to every element class, each Element exposes `accept(visitor)`, calling `visitor.visit(self)`. A new HtmlExportVisitor or JsonExportVisitor is added instead, AbstractButton, AbstractInput, AbstractText themselves never change.