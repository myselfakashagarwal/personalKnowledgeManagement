## Keys
Keys are attributes or attribute sets used for tuple identification and relationship enforcement. They provide unique identification within relations and establish links between entity sets through referential mechanisms.
### Types of keys
#### **Primary Key**
A primary key is the designated unique identifier for each tuple in a relation. It enforces entity integrity by prohibiting NULL values and ensuring uniqueness across all tuples. A relation has exactly one primary key, which may be a single attribute or composite key. Selected from candidate keys based on stability, simplicity, and business requirements.
#### **Candidate Key**
A candidate key is any minimal attribute set capable of uniquely identifying tuples in a relation. Multiple candidate keys may exist per relation. One is selected as the primary key while others become alternate keys. Candidate keys are minimal super keys removing any attribute destroys uniqueness.
#### **Super Key**
A super key is any attribute set that uniquely identifies tuples, potentially containing redundant attributes beyond minimal requirements. Every candidate key is a super key, but not every super key is a candidate key. Forms the theoretical foundation for understanding uniqueness constraints.
#### **Foreign Key**
A foreign key is an attribute or attribute set in one relation that references a candidate key (typically primary key) in another relation. Establishes inter-relational dependencies and enforces referential integrity constraints. Permits NULL values if the relationship is optional, creating the foundation for relationship implementation in relational databases.
#### **Alternate Key**
An alternate key is a candidate key not selected as the primary key. Retains unique identification capability and is typically enforced via UNIQUE constraints. Useful for alternate access paths and business rules requiring additional uniqueness guarantees beyond the primary key.
#### **Surrogate Key**
A surrogate key is an artificially generated identifier (auto-increment integer, UUID, GUID) without inherent business meaning. Provides stability when natural keys are composite, mutable, or semantically complex. Simplifies foreign key references and join operations while isolating logical design from physical implementation changes.
#### **Composite Key**
A composite key is a key formed by combining two or more attributes where the combination guarantees uniqueness though individual attributes may not. Can serve as primary, candidate, or foreign keys. Common when no single attribute uniquely identifies tuples or when modeling weak entities dependent on parent relations.
#### **Compound Key**
A compound key is a specialized composite key where each constituent attribute is also a foreign key referencing different parent relations. Primarily used in associative (junction) tables modeling many-to-many relationships, where the composite key simultaneously serves as the primary key and references multiple parent entities.