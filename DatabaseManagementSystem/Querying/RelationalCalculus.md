## Relational Calculus

Relational calculus is a **declarative** query language based on mathematical logic that describes **"What to retrieve"** rather than **"How to retrieve it"**. It uses predicate logic to specify the desired properties of the result without specifying the procedure to obtain it.
## Tuple Relational Calculus (TRC)
TRC uses tuple variables that range over relations. The general form is `{ t | P(t) }` where `t` is a tuple variable and `P(t)` is a predicate that must be true.
#### `SAMPLE DATA FOR EXAMPLES`

##### Employee
| EmpID | Name  | DeptID | Salary |
| ----- | ----- | ------ | ------ |
| 1     | Alice | 10     | 60000  |
| 2     | Bob   | 20     | 55000  |
| 3     | Carol | 10     | 70000  |
| 4     | David | 30     | 40000  |
| 5     | Eve   | 20     | 75000  |
##### Department
| DeptID | DeptName | Location  |
| ------ | -------- | --------- |
| 10     | HR       | Delhi     |
| 20     | IT       | Mumbai    |
| 30     | Finance  | Bangalore |
##### Project
| ProjID | ProjName | DeptID |
| ------ | -------- | ------ |
| 101    | Alpha    | 10     |
| 102    | Beta     | 20     |
| 103    | Gamma    | 10     |
##### WorksOn
| EmpID | ProjID | Hours |
| ----- | ------ | ----- |
| 1     | 101    | 20    |
| 1     | 103    | 15    |
| 2     | 102    | 30    |
| 3     | 101    | 25    |
| 5     | 102    | 40    |
### Operations
#### Selection (Filtering Rows)
Selection retrieves tuples that satisfy a condition using predicates on tuple attributes. Find all employees with salary greater than 60000 $$ { t \mid t \in Employee \land t.Salary > 60000 } $$

| EmpID | Name  | DeptID | Salary |
| ----- | ----- | ------ | ------ |
| 3     | Carol | 10     | 70000  |
| 5     | Eve   | 20     | 75000  |
#### Projection (Selecting Columns)
Projection extracts specific attributes from tuples using existential quantifiers. Get names and salaries of all employees $$ { t \mid \exists e \in Employee (t.Name = e.Name \land t.Salary = e.Salary) } $$

| Name  | Salary |
| ----- | ------ |
| Alice | 60000  |
| Bob   | 55000  |
| Carol | 70000  |
| David | 40000  |
| Eve   | 75000  |
#### Combined Selection and Projection
Combines filtering and attribute extraction in a single query. Find names of employees in DeptID 10 $$ { t \mid \exists e \in Employee (e.DeptID = 10 \land t.Name = e.Name) } $$

|Name|
|---|
|Alice|
|Carol|
#### Existential Quantifier (∃)
Existential quantifier checks if there exists at least one tuple satisfying the condition. Find employees who work on at least one project $$ { t \mid t \in Employee \land \exists w \in WorksOn (w.EmpID = t.EmpID) } $$

| EmpID | Name  | DeptID | Salary |
| ----- | ----- | ------ | ------ |
| 1     | Alice | 10     | 60000  |
| 2     | Bob   | 20     | 55000  |
| 3     | Carol | 10     | 70000  |
| 5     | Eve   | 20     | 75000  |

#### Universal Quantifier (∀)
Universal quantifier checks if a condition holds for all tuples in a relation. Find employees who work on ALL projects in DeptID 10 $$ { t \mid t \in Employee \land \forall p \in Project (p.DeptID \neq 10 \lor \exists w \in WorksOn (w.EmpID = t.EmpID \land w.ProjID = p.ProjID)) } $$

|EmpID|Name|DeptID|Salary|
|---|---|---|---|
|1|Alice|10|60000|
#### Negation (¬)
Negation filters tuples that do not satisfy a given condition. Find employees who do NOT work on any project $$ { t \mid t \in Employee \land \neg \exists w \in WorksOn (w.EmpID = t.EmpID) } $$

|EmpID|Name|DeptID|Salary|
|---|---|---|---|
|4|David|30|40000|
#### Natural Join
Natural join combines tuples from two relations based on common attributes. Find employee names with their department names $$ { t \mid \exists e \in Employee , \exists d \in Department (e.DeptID = d.DeptID \land t.Name = e.Name \land t.DeptName = d.DeptName) } $$

|Name|DeptName|
|---|---|
|Alice|HR|
|Bob|IT|
|Carol|HR|
|David|Finance|
|Eve|IT|
#### Multiple Conditions
Multiple conditions combine several predicates using logical operators. Find names of employees in IT department with salary > 60000 $$ { t \mid \exists e \in Employee , \exists d \in Department (e.DeptID = d.DeptID \land d.DeptName = 'IT' \land e.Salary > 60000 \land t.Name = e.Name) } $$

|Name|
|---|
|Eve|

#### Division
Division finds tuples that are related to all tuples in another relation. Find employees who work on ALL projects $$ { t \mid t \in Employee \land \forall p \in Project (\exists w \in WorksOn (w.EmpID = t.EmpID \land w.ProjID = p.ProjID)) } $$

| EmpID | Name | DeptID | Salary |
| ----- | ---- | ------ | ------ |
## Domain Relational Calculus (DRC)
DRC uses domain variables that range over attribute values. The general form is `{ ⟨x₁, x₂, ...⟩ | P(x₁, x₂, ...) }` where variables represent individual attribute values.
### Operations
#### Selection
Selection filters domain values based on predicates. Find EmpID and Name of employees with salary > 60000 $$ { \langle i, n \rangle \mid \exists d , \exists s (Employee(i, n, d, s) \land s > 60000) } $$

|EmpID|Name|
|---|---|
|3|Carol|
|5|Eve|
#### Projection
Projection extracts specific domain values from relations. Get all employee names $$ { \langle n \rangle \mid \exists i , \exists d , \exists s (Employee(i, n, d, s)) } $$

|Name|
|---|
|Alice|
|Bob|
|Carol|
|David|
|Eve|
#### Join
Join combines domain values from multiple relations based on common attributes. Find employee names and their department locations $$ { \langle n, l \rangle \mid \exists i , \exists d1 , \exists s , \exists d2 , \exists dn (Employee(i, n, d1, s) \land Department(d2, dn, l) \land d1 = d2) } $$

|Name|Location|
|---|---|
|Alice|Delhi|
|Bob|Mumbai|
|Carol|Delhi|
|David|Bangalore|
|Eve|Mumbai|
#### Existential (∃)
Existential quantifier checks for existence of domain values satisfying conditions. Find names of employees who work on project 101 $$ { \langle n \rangle \mid \exists i , \exists d , \exists s , \exists p , \exists h (Employee(i, n, d, s) \land WorksOn(i, p, h) \land p = 101) } $$

|Name|
|---|
|Alice|
|Carol|
#### Universal (∀)
Universal quantifier checks if conditions hold for all domain values. Find employees who work on projects with hours ≥ 20 for ALL their assigned projects $$ { \langle i, n \rangle \mid \exists d , \exists s (Employee(i, n, d, s) \land \forall p , \forall h (WorksOn(i, p, h) \rightarrow h \geq 20)) } $$

|EmpID|Name|
|---|---|
|2|Bob|
|3|Carol|
|4|David|
|5|Eve|