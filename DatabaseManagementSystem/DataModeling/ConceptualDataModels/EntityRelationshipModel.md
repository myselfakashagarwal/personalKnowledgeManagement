Entity Relationship model is used to represent real world objects, attributes and their relationships in graphical way.
## Entity 
An entity can be defined as the real world object or concept that is capable of independent existence and is uniquely identifiable. 
### Types of entities 
#### Strong Entity
A strong entity is a standalone database table that can exist on its own with a unique primary key. It doesn't need any other table to identify its records. 
#### Weak Entity
A weak entity cannot stand alone and needs to "borrow" identity from another table. Without the parent entity, the weak entity cannot be uniquely identified.
#### Regular Entity
This is simply another name for a strong entity. It's a standard, independent table that has its own primary key and can exist without depending on other tables. The terms "regular entity" and "strong entity" are used interchangeably in database design.
#### Associative Entity
When two tables have a many-to-many relationship, an associative entity acts as a bridge between them. It's created specifically to handle this relationship and often stores additional information about the connection.
#### Recursive Entity
A recursive entity has a relationship with itself, where records in the same table are related to each other. It's like an employee table where some employees supervise other employees - the same entity plays different roles in the relationship.
#### Super Entity (Supertype)
A super entity is like a parent category that contains common characteristics shared by multiple related sub-categories. It represents the general attributes that all its subtypes inherit.
#### Sub Entity (Subtype)
A sub entity is a specialized version of a super entity that inherits all parent attributes and adds its own specific characteristics.
## Attributes 
An attribute can be defined as behaviour & characteristics that make up an entity. 
### Types of attribute include 
#### Simple Attribute
A simple attribute cannot be broken down into smaller meaningful parts. It represents a single, indivisible piece of data that has no sub-components. 
#### Composite Attribute
A composite attribute can be divided into smaller sub-attributes, each with its own meaning. It's like a container that holds multiple related pieces of information. 
#### Single-Valued Attribute
A single-valued attribute can hold only one value at a time for each entity instance. 
#### Multi-Valued Attribute
A multi-valued attribute can store multiple values for a single entity instance. It's like having a list of values rather than just one.
#### Derived Attribute
A derived attribute's value is calculated or derived from other attributes rather than being stored directly. It's computed automatically based on existing data. 
#### Stored Attribute
A stored attribute's value is directly entered and physically saved in the database. Unlike derived attributes, these values are not calculated but are explicitly provided and maintained in the database tables.
#### Key Attribute 
A key attribute (or combination of attributes) uniquely identifies each entity instance. It serves as the primary identifier and cannot have duplicate values. 
#### Non-Key Attribute 
A non-key attribute provides descriptive information about an entity but doesn't uniquely identify it. These attributes can have duplicate values across different entity instances.

## Relationship
A relationship is how one entity is related to other entity.
### Types of relationship
#### `CARDINALITY-BASED RELATIONSHIPS`
_Classifications based on how many instances can participate in the relationship_
#### **One-to-One (1:1) Relationship**
A one-to-one relationship creates a perfect pairing where each entity instance corresponds to exactly one instance of another entity. Represented by a diamond with "1" on both connecting lines, this is exemplified by the person-passport relationship where each person has exactly one passport.
#### One-to-Many (1:M) Relationship
The most common relationship type where one entity instance associates with multiple instances of another entity, but each second entity relates to only one first entity. This hierarchical structure uses a diamond with "1" on one side and "M" on the other, like the teacher-students relationship.
#### Many-to-One (M:1) Relationship
Multiple first entity instances relate to a single second entity instance, emphasizing aggregation patterns. Using a diamond with "M" and "1" on opposite sides.
#### Many-to-Many (M:M) Relationship
Multiple instances of both entities can associate simultaneously, creating bidirectional relationships. Depicted by a diamond with "M" on both sides.
### `PARTICIPATION-BASED RELATIONSHIPS`
_Classifications based on the number of different entity types involved_
#### Unary/Recursive Relationship
A self-referencing relationship where an entity relates to itself in different roles, enabling hierarchical structures within a single entity type. Represented by a diamond with a curved line looping back, like employee supervision systems.
#### Binary Relationship
The foundation of relational design connecting exactly two distinct entity types. Symbolised by a diamond connected to two entity rectangles, like student-course enrolment. 
#### Ternary Relationship
Involves three entity types participating simultaneously where all entities must be present for meaningful relationships. Depicted by a diamond connected to three entities.
#### N-ary Relationship
The most complex associations involving more than three entity types (n > 3) participating simultaneously. Symbolised by diamonds connected to multiple entities.
### `DEPENDENCY-BASED RELATIONSHIPS`
_Classifications based on entity independence and existence dependency_
#### Strong Relationship
Connects strong entities that exist independently without creating existence dependencies. Depicted by solid-bordered diamonds.
#### Weak Relationship
Involves weak entities whose existence depends fundamentally on relationships with strong entities. Represented by double diamonds.
### `KEY-INHERITANCE RELATIONSHIPS`
_Classifications based on how primary keys are inherited and managed_
#### Identifying Relationship
Creates strong coupling by incorporating parent entity primary keys as part of child entity primary keys. Represented by diamonds with solid connecting lines.
#### Non-Identifying Relationship
Maintains entity independence by using parent primary keys as foreign keys without incorporating them into child primary keys. Depicted by diamonds with dashed connecting lines.
### `RELATIONSHIP INTERACTION MATRIX`
| **Relationship Type** | **Can Combine With**                | **Common Patterns**   | **Implementation Notes**     |
| --------------------- | ----------------------------------- | --------------------- | ---------------------------- |
| **1:1 + Binary**      | Strong, Identifying/Non-Identifying | Person-Passport       | Simple foreign key           |
| **1:M + Binary**      | Strong, Non-Identifying             | Department-Employee   | Most common pattern          |
| **M:M + Binary**      | Strong, Non-Identifying             | Student-Course        | Requires junction table      |
| **1:M + Recursive**   | Strong, Non-Identifying             | Employee hierarchy    | Self-referencing foreign key |
| **M:M + Ternary**     | Strong, Non-Identifying             | Supplier-Part-Project | Complex junction table       |
| **1:M + Weak**        | Weak, Identifying                   | Order-OrderItem       | Composite primary key        |

### `DESIGN DECISION FLOWCHART`

```
CHOOSING RELATIONSHIP TYPE
│
├── How many entities involved?
│   ├── 1 entity → Recursive/Unary
│   ├── 2 entities → Binary
│   ├── 3 entities → Ternary
│   └── 4+ entities → N-ary
│
├── What's the cardinality?
│   ├── 1:1 → One-to-One
│   ├── 1:M → One-to-Many
│   ├── M:1 → Many-to-One
│   └── M:M → Many-to-Many
│
├── Are entities independent?
│   ├── Yes → Strong Relationship
│   └── No → Weak Relationship
│
└── How should keys be handled?
    ├── Parent key part of child key → Identifying
    └── Parent key as foreign key → Non-Identifying
```

![](https://www.conceptdraw.com/How-To-Guide/picture/Entity-Relationship-Diagram-Symbols.png)
