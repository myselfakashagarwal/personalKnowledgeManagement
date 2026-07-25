## Failure & Recovery 
Failure refer the case of denial of service, or data harm to the database. After the database fails the database boots into recovery mode in which it redo all the last committed transactions and undo all the failed ones. 
### Recovery Mechanisms 
#### Logging and Write ahead logging 
Log is just a written  evidence of the event (happens after the event) with the logs, this is succeed with write ahed logging in which the log is written before the data page update , the log itself contains the old and updated value, in case of the failure after the log itself the values can be restored with the help of old.   
#### Update strategy
##### `Deferred update`
In deferred update the data pages are untouched but all the logs are written first and force commit log is written. In case the db turns back to recovery mode the commits are applied, in midst of transaction if it fails there's nothing to do cause  pages are untouched.
##### `Immediate update`
In immediate update the data pages may be written on the go, thus undo and redo are applied.  
#### Paging
The paging is just taking the input from the user and update it to real table, During paging the two tables are maintained 
##### `Shaow page` 
Shadow page is the last stable committed page. 
##### `current page` 
Current page is just a fork of the shadow page, the transactions are updated init if the commit fails, the current is destroyed. But if the current succeeds the shadow is updated to point the current pages.  
### ARIES 
ARIES is a logging based recovery algorithm used in modern DBMSs that supports immediate update and write ahead logging while enabling high concurrency. It assigns log sequence numbers to log records and stores the last applied LSN in each page. After a crash, recovery proceeds in three phases: analysis identifies active transactions and dirty pages, redo repeats all logged updates from the earliest dirty page to reconstruct the exact pre crash state, and undo rolls back all uncommitted transactions using compensation log records that ensure idempotence. The core principle of ARIES is repeat history and then undo losers, which guarantees atomicity and durability without sacrificing concurrency.