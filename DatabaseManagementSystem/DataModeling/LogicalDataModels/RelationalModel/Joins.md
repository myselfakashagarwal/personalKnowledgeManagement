## Joins
Joins are relational algebra operations that combine tuples from multiple relations based on related attributes, implementing relationship traversal and query composition across entity sets.
### Types of joins
#### **INNER JOIN**
Returns only tuples where the join predicate evaluates true for both relations. Non-matching tuples are excluded from the result set. Represents the relational algebraic **θ-join** (theta-join) with an arbitrary condition, or **equi-join** when using equality predicates. Most common join type, producing the intersection of matching tuples.
#### **LEFT OUTER JOIN (LEFT JOIN)**
Returns all tuples from the left (preserving) relation and matching tuples from the right relation. Non-matching left tuples appear with NULL-padded right attributes. Preserves left relation cardinality while augmenting with right relation data where matches exist. Useful for including entities without related dependents.
#### **RIGHT OUTER JOIN (RIGHT JOIN)**
Returns all tuples from the right (preserving) relation and matching tuples from the left relation. Non-matching right tuples appear with NULL-padded left attributes. Preserves right relation cardinality. Semantically equivalent to LEFT JOIN with reversed operands.
#### **FULL OUTER JOIN**
Returns all tuples from both relations, joining where predicates match and padding non-matching tuples with NULLs from the opposite relation. Combines LEFT and RIGHT outer join semantics, preserving cardinality from both relations. Represents the union of inner join with unmatched tuples from both sides.
#### **CROSS JOIN (Cartesian Product)**
Returns the Cartesian product of both relations—every tuple from the left relation paired with every tuple from the right relation. Produces |R| × |S| result tuples where |R| and |S| are relation cardinalities. Typically filtered with WHERE predicates to avoid combinatorial explosion. Represents the fundamental relational algebraic product operation (R × S).
#### **SELF JOIN**
A relation joined with itself using table aliases to distinguish tuple sources. Essential for hierarchical or recursive relationships within a single entity set (employees→managers, organizational trees, graphs). Requires alias qualification to disambiguate attribute references from the same relation.
#### **NATURAL JOIN**
An implicit equi-join on all commonly-named attributes across both relations. Join condition automatically constructed by matching column names. Dangerous in practice—schema evolution introducing new same-named columns can alter join semantics unexpectedly, causing unintended matches or incorrect results. Avoids explicit ON/USING clauses but sacrifices explicitness and maintainability.
#### **EQUI-JOIN**
A join using only equality predicates (=) in the join condition. Subset of θ-join where θ is restricted to equality comparisons. Most common join form due to foreign key relationships typically using equality semantics. INNER JOINs and OUTER JOINs with equality conditions are equi-joins. Natural joins are implicit equi-joins.