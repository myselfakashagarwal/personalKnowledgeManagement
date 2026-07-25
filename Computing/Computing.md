**Computing** is the formal process of representing information as symbols, transforming those symbols through well defined rules, and executing those transformations using physical or abstract machines to produce deterministic or probabilistic outcomes. It consists of four irreducible components: **information encoding**, an **algorithm or rule system** that defines state transitions, a **computational model** that governs execution semantics, and a **physical or virtual substrate** that realizes the computation.

The **goal of computation** is to **systematically transform information into new information according to formally defined rules in order to obtain meaningful results**.

A paradigm is the foundational worldview that precedes every technical decision. It does not specify structure, technology, or behaviour. It determines what questions are worth asking, what problems are considered real, and what counts as a valid solution. It is the assumption layer beneath all else, invisible until it is wrong, at which point it is abandoned entirely and replaced by a new worldview that better explains reality.

An architecture is the structural decomposition of a system into its components, their responsibilities, and their relationships. It defines what each component owns, what crosses each boundary, and what guarantees must be preserved across the whole. It does not describe how components are built. It describes what they must be and what they must never violate. It is the constraint layer, the set of rules that define what the system is allowed to become and what it is forbidden from becoming.

A model is the formal specification of how computation must proceed. It is the abstract machine that defines the admissible transitions between states given any input. It is precise, technology agnostic, and exists independently of any implementation. It does not run. It defines what running must mean, the logical rules that govern execution regardless of what physical or virtual machinery realises them.

Semantics is the meaning layer, the contracts and guarantees that give every primitive its significance. It is not the model itself but the precise definition of what the model's operations promise and what violation of those promises means. Semantics is what separates a system that computes correctly from one that merely runs. When a semantic guarantee breaks, the system is not slow or inefficient. It is wrong.

A system is the living instance of all of the above enacted through time. It has actual state, actual failures, actual latencies, and actual costs. It is where every abstraction above it collides with reality and the gap between what was specified and what actually happens becomes visible. It can degrade. It can die. It evolves. No two instances of the same architecture running the same implementation are ever identical systems because reality is never identical twice.

A substrate is the actual physical or virtual foundation on which every level above it is realised. It is the hardware, the network, the energy, the silicon. It defines the true failure domains, the true capacity limits, and the true cost of all computation. Everything above it is abstraction. The substrate is where abstraction ends and physics begins. It is what actually consumes energy, what actually fails, and what actually persists information across time.

These six levels are not a stack of independent choices. They are mutually constraining and mutually defining. The paradigm shapes the architecture. The architecture selects from the model. The model gives meaning to the semantics. The semantics define correctness for the system. The system is bounded by the substrate. And the substrate's true limits reveal whether every level above it was coherently designed or merely assumed.

A system computes not because it runs a program but because it holds itself together across all six levels simultaneously, maintaining coherence between its philosophy, its structure, its formal rules, its meaning contracts, its living behaviour, and its physical foundation. Its computational limits are exactly the limits of how long and how well it can sustain that coherence. When any level loses alignment with the others, organisation begins to collapse. When collapse becomes irreversible, computation ends. That is the complete definition of what it means for a system to compute and what it means for it to die.

[[Computing]]
[[Paradigms]]
[[Systems]]



