## View 
Views are just virtual relation, defined by stored query expression, it does not store the data by itself but act as an abstraction over multiple aspects such as joins. The operation on the view is just same as multiple operations on different base tables.  

The view is compiled, thus change in the schema of the base tables will not be reflected, in that case view have to be recompiled, this behaviour is DBMS specific 