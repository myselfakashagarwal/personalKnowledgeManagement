## Normalisation
Normalisation can be defined as the framework with set of rules that can be implemented to reduce data redundancies & improve data integrity. The purpose of normalisation is to remove anomalies including `Insertion anomaly` - unable to insert data due to absence of particular attributes, `Deletion anomaly`- deletion of one result in deletion of many `Update anomaly` - same values are repeated but not related to each other. By core intuition: reducing the dependence of an attribute on other attribute make sure the element get as much as distinguishable as possible. 
### Types of dependencies
| **Dependency Type**    | **Definition**                                                                    | **Explanation**                                                                     |
| ---------------------- | --------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------- |
| **Full Functional**    | A non-key attribute depends on the **entire composite key**.                      | You need all parts of the composite key to determine the attribute.                 |
| **Partial Functional** | A non-key attribute depends on **part of a composite key**.                       | Only a portion of the composite key determines the attribute.                       |
| **Multivalued**        | One attribute determines **independent multi-valued sets** of another attribute.  | Not all attributes depend on each other; e.g., Teacher →→ Subject, Teacher →→ City. |
| **Transitive**         | An attribute depends on the primary key **indirectly through another attribute**. | Example: PK → DeptID, DeptID → DeptName, so PK → DeptName.                          |
| **Trivial**            | Dependent attribute is a **subset of the determinant**.                           | Example: {StudentID, Name} → StudentID. Always true, not useful for normalization.  |
### Types of normal forms
| **Normal Form**                    | **Rule / Condition**                                                                                       | **What it Removes**                                                 |
| ---------------------------------- | ---------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------- |
| **1NF (First Normal Form)**        | Table must have only **atomic (indivisible) values**. No repeating groups, no arrays.                      | Removes repeating columns & multi-valued attributes.                |
| **2NF (Second Normal Form)**       | Must be in 1NF **and** no **Partial Dependency** (non-key attribute depending on part of a composite key). | Removes dependency on part of a key.                                |
| **3NF (Third Normal Form)**        | Must be in 2NF **and** no **Transitive Dependency** (non-key depending indirectly on key).                 | Removes indirect dependencies.                                      |
| **BCNF (Boyce–Codd Normal Form)**  | Must be in 3NF **and** every determinant must be a **candidate key**.                                      | Removes anomalies where non-keys determine keys.                    |
| **4NF (Fourth Normal Form)**       | Must be in BCNF **and** no **Multivalued Dependencies (MVDs)**.                                            | Removes independent multi-valued facts.                             |
| **5NF (Fifth Normal Form / PJNF)** | Must be in 4NF **and** no **Join Dependencies** unless implied by candidate keys.                          | Removes redundancy from reconstructable joins.                      |
| **6NF (Sixth Normal Form)**        | Must be in 5NF **and** deals with **non-trivial join dependencies** + **temporal/interval data**.          | Breaks down tables to handle time-varying / highly decomposed data. |

