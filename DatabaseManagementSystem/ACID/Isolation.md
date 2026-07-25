## Serialisation  & Parallelism
Multiple different transaction working on their own unique data at same time (Parallelism) causes no harm but strain schedules on resources, however when multiple on one (which definitely includes write) cause data inconsistency to dodge it serial schedules can be made which are multiple transaction bind together such that there is data inconsistency. But there's still one transaction at a time. 
### Types of Serializability
This answers the question: “How do we know if running transactions together is still as safe as running them one by one?”

| Type                         | Exact Meaning                                                                                                                                                |
| ---------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| Conflict Serializability     | A schedule is conflict serializable if it can be transformed into a serial schedule by swapping only non-conflicting operations without changing the result. |
| View Serializability         | A schedule is view serializable if it is view equivalent to a serial schedule, meaning same read-from relationships and same final writes.                   |
| Final State Serializability  | A schedule is final state serializable if it results in the same final database state as some serial schedule, regardless of intermediate operations.        |
| Commit Order Serializability | A schedule is commit-order serializable if the serial order of transactions matches the order in which they commit.                                          |
### Types of Serializable Schedules
This answers the question: **“What kinds of safe transaction timelines exist?”**

| Schedule Type                     | Exact Meaning                                                                                                                                        |
| --------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------- |
| Conflict Serializable Schedule    | A schedule that can be transformed into a serial schedule by reordering only non-conflicting operations, preserving the result.                      |
| View Serializable Schedule        | A schedule that is view equivalent to a serial schedule, meaning each read gets the same value and the final writes match a serial order.            |
| Strict Serializable Schedule      | A schedule that respects real time order: if one transaction completes before another starts, it must appear before it in the serial order.          |
| Recoverable Serializable Schedule | A schedule where a transaction commits only after all transactions whose data it has read have committed, preventing cascading rollbacks.            |
| Strict 2PL Serializable Schedule  | A schedule generated using strict two phase locking where all locks are held until commit, guaranteeing no dirty reads and conflict serializability. |
## Concurrency & Synchronisation
Multiple transactions on single data at a time without conflict, synchronised. 
### Synchronisation mechanisms
#### Locking  
Lock based protocols enforce concurrency control by requiring transactions to acquire locks before reading or writing data. Locks are managed by a concurrency control manager and ensure isolation by synchronizing access to shared data items. A lock is a variable associated with a data item defining permitted operations.
##### Binary Lock 
In binary lock the data resources are either locked (in use) or not , a transaction can have these take as needed and release as their work is done. 
##### Shared and Exclusive lock
In shared lock the resources can be accessed at same time i.e multiple transaction can lock the resource (for read purposed only) while in exclusive lock the sources is processed by write operations first then followed with all shared those who need to read. 
##### Pre claim locking 
in pre claim locking transactions waits for all data resources to be free , then acquires lock on all of them then proceed with the execution. 
##### Two phase locking
in two phase locking there's growing phase in which the transaction acquires lock but may not release it, after all are acquired it executes then, however in shrinking phase the locks are released as soon as their work gets over. To prevent this strict two phase locking is implemented which release all locks at once.  In rigorous locks are maintained if the transactions is not committed. 
#### Time stamp 
In timestamp the system uses system time as logical counter with aspects such as first come first server with respect to first finishes first wins. i.e If both comes at same time and first finisher wins, if both finishes at the same time the first comer wins. 
### Dead locks
A deadlock is a situation in which one waits for other event to happen while the other cannot happen before the one. 
#### Deadlock preventions
| Deadlock Prevention Technique | Exact Meaning                                                                                                            |
| ----------------------------- | ------------------------------------------------------------------------------------------------------------------------ |
| Lock Ordering                 | Transactions must acquire locks in a predefined global order, eliminating circular wait.                                 |
| Pre Claiming                  | Transaction requests all required locks before execution; if any lock is unavailable, it waits without holding any lock. |
| Wait Die                      | Timestamp based scheme where older transactions wait and younger transactions abort when requesting a locked item.       |
| Wound Wait                    | Timestamp based scheme where older transactions abort younger ones, and younger transactions wait.                       |
| Conservative 2PL              | Transaction acquires all locks before starting; execution begins only if all locks are granted.                          |
#### Deadlock detection 
| Deadlock Detection Technique | Exact Meaning                                                                                                 |
| ---------------------------- | ------------------------------------------------------------------------------------------------------------- |
| Wait For Graph               | Constructs a graph of transactions waiting for locks and detects cycles to identify deadlocks.                |
| Timeout                      | Assumes a transaction is deadlocked if it waits longer than a fixed threshold.                                |
| Probe Based Detection        | Sends probe messages through waiting transactions; cycle detection indicates deadlock in distributed systems. |
| Centralized Detection        | A single site collects wait information and detects deadlocks globally.                                       |
| Distributed Detection        | Each site detects local waits and cooperates to identify global deadlocks.                                    |
### Validation Based Protocol
**Validation based Protocol** in DBMS also known as Optimistic Concurrency Control Technique is a method to avoid concurrency in transactions. In this protocol, the local copies of the transaction data are updated rather than the data itself, which results in less interference while execution of the transaction. With phases includes: 

| Phase            | Exact Meaning                                                                                                                            |
| ---------------- | ---------------------------------------------------------------------------------------------------------------------------------------- |
| Read Phase       | Transaction reads data from the database and performs updates only on local copies, without modifying the actual database.               |
| Validation Phase | Transaction checks whether applying its updates would violate serializability with respect to other concurrent transactions.             |
| Write Phase      | If validation succeeds, local updates are written to the database; otherwise, the transaction is rolled back and no updates are applied. |
