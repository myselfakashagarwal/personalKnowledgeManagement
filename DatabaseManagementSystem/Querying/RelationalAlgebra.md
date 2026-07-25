## Relation Algebra
Relational algebra is a theoretical model for querying which is composed of _“What to do”_ and _“How to do”_ on structured data.  It is composed of mathematical expressions for representing data operations and is fundamentally based on set theory.
### Operators, Operands, and Their Types
The operands are the relations (tables) of the database, and the operators are predefined functions that perform actions on those relations.  Each operator takes one or more relations as input and produces a new relation as output maintaining closure of the algebra.
#### `SAMPLE DATA FOR THE EXAMPLE`
##### Employee
| EmpID | Name  | DeptID | Salary |
| ----- | ----- | ------ | ------ |
| 1     | Alice | 10     | 60000  |
| 2     | Bob   | 20     | 55000  |
| 3     | Carol | 10     | 70000  |
| 4     | David | 30     | 40000  |
| 5     | Eve   | 20     | 75000  |
##### TempEmployee
| EmpID | Name  | DeptID | Salary |
| ----- | ----- | ------ | ------ |
| 6     | Frank | 20     | 50000  |
| 7     | Grace | 10     | 72000  |
##### Department
| DeptID | DeptName | Location  |
| ------ | -------- | --------- |
| 10     | HR       | Delhi     |
| 20     | IT       | Mumbai    |
| 30     | Finance  | Bangalore |
#### Projection (π)
Projection selects specific attributes (columns) from a relation, removing duplicates by default.
In order to project two relation as one they have to be performed through joins, product or union. 
$$
\pi_{Name,Salary}(Employee)
$$

| Name  | Salary |
| ----- | ------ |
| Alice | 60000  |
| Bob   | 55000  |
| Carol | 70000  |
| David | 40000  |
| Eve   | 75000  |
#### Selection (σ)
Selection retrieves tuples (rows) from a relation that satisfy a given predicate (condition). In order to pass two tables they must have a relation. 
$$
\sigma_{Salary > 60000}(Employee)
$$

| EmpID | Name  | DeptID | Salary |
| ----- | ----- | ------ | ------ |
| 3     | Carol | 10     | 70000  |
| 5     | Eve   | 20     | 75000  |

#### Union (∪)
Union gives the combined tuples of the tables. 

$$
R \cup S
$$
$$
\text{degree}(R \cup S) = \text{degree}(R) = \text{degree}(S)
$$
$$
|R \cup S| \le |R| + |S|
$$
$$
(EmpID, Name, DeptID, Salary)
$$
$$
Employee \cup TempEmployees
$$
$$
(EmpID, Name, DeptID, Salary)
$$

| EmpID | Name  | DeptID | Salary |
| :---- | :---- | :----- | :----- |
| 1     | Alice | 10     | 60000  |
| 2     | Bob   | 20     | 55000  |
| 3     | Carol | 10     | 70000  |
| 4     | David | 30     | 40000  |
| 5     | Eve   | 20     | 75000  |
| 6     | Frank | 20     | 50000  |
| 7     | Grace | 10     | 72000  |
#### Cross Product (×)
Cartesian or Cross Product combines every tuple of one relation with every tuple of another.
$$
Employee \times Department
$$
$$
degree(R \times S) = degree(R) + degree(S)
$$
$$
Cardaniality |R \times S| = |R| \times |S|
$$
$$
(EmpID,\ Name,\ Emp.DeptID,\ Salary,\ Dept.DeptID,\ DeptName,\ Location)
$$
$$
\sigma_{Employee.DeptID = Department.DeptID}(Employee \times Department)
$$
$$
(EmpID, Name, Emp.DeptID, Salary, Dept.DeptID, DeptName, Location)
$$

| EmpID | Name    | Emp.DeptID | Salary | Dept.DeptID | DeptName  | Location  |
| ----- | ------- | ---------- | ------ | ----------- | --------- | --------- |
| 1     | Alice   | 10         | 60000  | 10          | HR        | Delhi     |
| 2     | Bob     | 20         | 55000  | 20          | IT        | Mumbai    |
| 3     | Charlie | 10         | 70000  | 10          | HR        | Delhi     |
| 4     | David   | 30         | 50000  | 30          | Marketing | Bangalore |
| 5     | Eve     | 20         | 80000  | 20          | IT        | Mumbai    |
#### Set Difference (−)
Set Difference returns tuples that are in one relation but not in another.
$$
\pi_{DeptID}(Department) - \pi_{DeptID}(Employee)
$$

| EmpID | Name | DeptID | Salary |
| ----- | ---- | ------ | ------ |
#### Intersection (∩)

$$R \cap S$$
$$\text{degree}(R \cap S) = \text{degree}(R) = \text{degree}(S)$$
$$|R \cap S| \le \min(|R|, |S|)$$
$$(EmpID, Name, DeptID, Salary)$$
$$Employee \cap TempEmployees$$
$$(EmpID, Name, DeptID, Salary)$$
| EmpID | Name | DeptID | Salary |
| ----- | ---- | ------ | ------ |
#### Rename (ρ)
Rename changes the name of a relation or its attributes, allowing reusability or self-joins.
Only mention attributes are kept; unmentioned attribute is dropped. 
$$\rho_{newEmployee(Name,Salary)}(Employee)$$
$$\text{degree}(\rho_{newEmployee(Name,Salary)}(Employee)) = \text{degree}(Employee)$$
$$|\rho_{newEmployee(Name,Salary)}(Employee)| = |Employee|$$
$$(EmpID, Name, DeptID, Salary)$$
$$(EmpID, Name, Salary)$$

#### Join (⋈)

| **Join Type**           | **Symbol / Expression**     | **Purpose / Result**                                                                       | **Equivalent Basic Operation**                                                               |
| ----------------------- | --------------------------- | ------------------------------------------------------------------------------------------ | -------------------------------------------------------------------------------------------- |
| **Theta Join (θ-Join)** | ( R \bowtie_{\theta} S )    | Combines tuples of `R` and `S` that satisfy a general condition (e.g. `R.A > S.B`)         | ( \sigma_{\theta}(R \times S) )                                                              |
| **Equi Join**           | ( R \bowtie_{R.A = S.B} S ) | Combines tuples using equality condition only                                              | ( \sigma_{R.A = S.B}(R \times S) )                                                           |
| **Natural Join**        | ( R \bowtie S )             | Joins automatically on all common attributes and removes duplicates                        | ( \pi_{\text{all attrs except duplicate}}(\sigma_{\text{R.common = S.common}}(R \times S)) ) |
| **Left Outer Join**     | ( R ⟕ S )                   | Keeps all tuples of `R`, adds `S` where condition matches, fills unmatched `S` with `NULL` | ( (R \bowtie S) \cup (R - \pi_R(R \bowtie S)) )                                              |
| **Right Outer Join**    | ( R ⟖ S )                   | Keeps all tuples of `S`, adds `R` where condition matches, fills unmatched `R` with `NULL` | ( (R \bowtie S) \cup (S - \pi_S(R \bowtie S)) )                                              |
| **Full Outer Join**     | ( R ⟗ S )                   | Keeps all tuples from both sides, unmatched ones padded with `NULL`                        | ( (R ⟕ S) \cup (R ⟖ S) )                                                                     |
| **Left Semi Join**      | ( R ⋉ S )                   | Returns only tuples from `R` that have a match in `S`                                      | ( \pi_R(R \bowtie S) )                                                                       |
| **Right Semi Join**     | ( R ⋊ S )                   | Returns only tuples from `S` that have a match in `R`                                      | ( \pi_S(R \bowtie S) )                                                                       |
| **Anti Join**           | ( R ▷ S )                   | Returns tuples from `R` that **do not** match any tuple in `S`                             | ( R - (R ⋉ S) )                                                                              |
#### Division (÷)
For division between R1 AND R2 every attribute of R2 the divisor must be present in the R1 , matter fact the R2 must be a subset of R1. 
$$
R_1 \div R_2 = 
\Pi_{St\_Name}(R_1)
-
\Pi_{St\_Name}
\big(
(\Pi_{St\_Name}(R_1) \times R_2)
-
R_1
\big)
$$
R1 

| St_Name | C_Name |
| ------- | ------ |
| Tom     | DBMS   |
| John    | DS     |
| Tom     | DS     |
| Tom     | CN     |
| John    | DBMS   |
| Amy     | CN     |
| Amy     | DBMS   |
| Amy     | DS     |
R2

| C_Name |
| ------ |
| DBMS   |
| DS     |
| CN     |
RESULT

| St_Name |
| ------- |
| Tom     |
| Amy     |
