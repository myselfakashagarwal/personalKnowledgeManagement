## Transaction
A transaction is a operational unit of work on the data || schema of the database. The data is the resource on which transaction works but the database is a shared resource two transactions executed at the same on same data can result in data inconsistencies for client and database also a database might become unavailable making transaction incomplete thus need for recovery.  `Refering transaction as a process`
### Transaction states 
| **State**           | **Transaction types**                                                                                                                                                   |
| ------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Active State        | A transaction enters into an active state when the execution process begins. During this state read or write operations can be performed.                               |
| Partially Committed | A transaction goes into the partially committed state after the end of a transaction.                                                                                   |
| Committed State     | When the transaction is committed to state, it has already completed its execution successfully. Moreover, all of its changes are recorded to the database permanently. |
| Failed State        | A transaction considers failed when any one of the checks fails or if the transaction is aborted while it is in the active state.                                       |
| Terminated State    | State of transaction reaches terminated state when certain transactions which are leaving the system can’t be restarted.                                                |
