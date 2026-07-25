## base
The size of the data block in the disk is fixed matter fact the one file dont hold all the database, for that the database divides records into data pages of fixed size one file can contain many pages and there can be many files. The data keep a mapping of which page is stored in which file too using catalogues.

When the data page is read, The data pages which might contain the record are loaded into the memory, later searched for finding the records asked. In case there’s no index all the pages are loaded into the memory which can blunder the time required for searching for records.
## Indexing for faster retrieving @relational
An index file is created to reduce the delay for the pouring data blocks in the memory, init the index file contains the value / key to data block or to record mapping, on the time of load instead of loading all the data blocks into the file to search for records the index file structure is loaded into the memory them searched.

The records are stored in the data blocks, these records can be arranged aka sorted such that they can be sequenced thus index table can have few entries gapping based on sequence, this is known as sparse indexing. However when data is unsorted each record need to be mapped in the index table to find also called as dense index.
### **Type of Indexing**
#### **Sorted + key = primary indexing** 
The index table contains primary key values as the key and addresses to data blocks as the value. Primary indexing is sparse, init the records are not stored yet data blocks. which contain multiple sequenced records
#### **Sorted + non key = clustered indexing** 
In clustered indexing the data is ordered but not on the basis of primary key, rather another attribution, so there can be continuous records but with reoccurring values which may also extend to multiple data blocks, In the index table there’s mapping to value to data block, in case there’s a data block that not fully contain the record matched linked with the help of pointers.
#### **Unsorted + key = secondary indexing** 
In the secondary indexing , data on the physical is not ordered itself in any case. One cannot search for the damn record in the haystack so in index table is created with sorted primary key index entries to mimic records storage in which the key represent the primary key values which are ordered and their record.
#### **Unsorted + non key = non clustered indexing** 
Non cluster indexing is blunder of all, there are multiple occurrence of same value , which can be found in multiple records so the index table is made like values → records that contain the value.
### Indexing structures
#### **Single Level Index **
Single level index just contain key/value pointing to records or data blocks
#### **Multilevel Index **
Muti level index is index for index in which key/value maps to another index
#### **B Trees ** 
Hierarchical organisation of entries in which a tree node can be range or key or value which is mapped to another record / data blocks
#### **B+ Trees **
B trees with modified version, containing linked list at end compose of connected leaf nodes.