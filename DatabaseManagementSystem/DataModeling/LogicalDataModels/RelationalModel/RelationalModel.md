## Relation Model
In relational model the data is organised in rows(tuple) and columns(degree, attribute) together making a table(relation) these relation can be referenced([[Joins]]) to each other using [[Keys]] 
## Codd 12 rules 

### **Rule 0 – Foundation Rule**
The DBMS must store, access, and manipulate data exclusively through relational structures and operators. Any storage method or API outside relational abstraction disqualifies the system.
### **Rule 1 – Information Rule**
All data at the logical level must be represented solely as scalar values within tuples grouped into relations. No alternative logical representations such as pointers, records, or hierarchical paths may be used.
### **Rule 2 – Guaranteed Access Rule**
Data retrieval must depend only on a relation name, a tuple primary key, and an attribute name. No physical navigation paths or positional references may be required.
### **Rule 3 – Systematic Treatment of Nulls**
NULL must serve as a uniform placeholder for absence or inapplicability. It must propagate through relational operators as defined and be type-independent but semantically consistent.
### **Rule 4 – Dynamic Online Catalog Based on Relational Model**
Metadata including schemas, constraints, authorization, and storage definitions must be represented as relations and queryable using the same relational language utilized for user data.
### **Rule 5 – Comprehensive Data Sublanguage Rule**
The system must provide at least one declarative relational language that supports data definition, view definition, data manipulation, integrity constraints, authorization, and transaction handling in a uniform syntax.
### **Rule 6 – View Updating Rule**
Views derived relationally must be updatable when their definition creates an unambiguous mapping back to base relations. The system must automate update propagation without custom procedural code.
### **Rule 7 – High-Level Insert, Update, Delete**
The DBMS must support set-oriented operations. Manipulations must target entire relations or sets of tuples, not single-record imperative operations.
### **Rule 8 – Physical Data Independence**
Changing file layouts, indexing structures, block sizes, compression, or hardware configurations must not invalidate application queries or require code changes.
### **Rule 9 – Logical Data Independence**
Schema changes such as attribute addition, deletion, or splitting should not affect existing applications or queries as long as semantics remain consistent.
### **Rule 10 – Integrity Independence**
All integrity constraints must reside within the relational catalog and be enforced by the DBMS kernel, not embedded in external application logic.
### **Rule 11 – Distribution Independence**
Whether data is centralized or partitioned, replicated, or distributed across nodes, query semantics and results must remain invariant and unaffected by data location.
### **Rule 12 – Non subversion Rule**
Low-level procedural access interfaces must not bypass relational constraints, triggers, or security rules; they must respect the same integrity semantics as high-level relational operations.

[[Joins]] [[Keys]] [[Constraints]] [[Normalisation]] [[View]] [[Trigger]] [[Querying]] [[ACID]]