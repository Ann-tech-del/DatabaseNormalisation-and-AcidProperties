# DATABASE NORMALIZATION AND ACID PROPERTIES

## DATABASE NORMALIZATION

**Normalization** in database  is a systematic approach to organize data within a database to reduce redundancy and eliminate undesirable characteristics such as **insertion**, **update**, and **deletion** anomalies.

### Why is Noramalization important in database.

1. Reduces data Rendundancy : Duplicate data is stored efficiently
2. Improves data intergrity : Ensures the accuracy and consistency of data by organizing it in a structured manner.
3. Simplifies Database Design: By following a clear structure, database designs become easier to maintain and update.
4. Optimizes Performance : Reduces the chance of anomalies and increases the efficiency of database operations.

### Normal Forms
Normal forms is a technique used in database design to reduce data redundancy and improve data intergrity by organizing data into tables and ensuring proper relationship.

### First Normal Form (1NF) : Eliminating Duplicate Records

#### Rules of 1NF 
- All columns contain atomic values (i.e., indivisible values).
- Each row is unique (i.e., no duplicate rows).
- Each column has a unique name.
- The order in which data is stored does not matter.

**Example of 1NF Violation :** If a table has a column "Units" that stores multiple Units in a single cell, it violates 1NF. To bring it into 1NF, you need to separate units into individual rows.

 ### 2. Second Normal Form (2NF) : Eliminating Partial Dependency
 #### Rules Of 2NF

 - It should be in 1NF.
 - And it should not have any **partial dependencies.**

**Partial dependency in a database** occurs when:

A non-prime attribute is functionally dependent on part of a candidate key, rather than the whole candidate key.

**Example of 2NF Violation :** For a composite key (StudentID, CourseID), if the StudentName depends only on StudentID and not on the entire key, it violates 2NF. To normalize, move StudentName into a separate table where it depends only on StudentID.

### 3. Third Normal Form (3NF) : Eliminating Transitive Dependency
#### Rules of 3NF
- It should be in its 2NF 
- And it should not have **transitive dependency.**

**Transitive dependency** in a database occurs when :

A non-prime attribute depends on another non-prime attribute, which in turn depends on a candidate key.

 **Example of 3NF Violation :** Consider a table with (StudentID, CourseID, departmentName). If departmentName depends on CourseID, and CourseID depends on StudentID, then departmentName indirectly depends on StudentID, which violates 3NF. To resolve this, place deparmentName in a separate table linked by CourseID.

 ## ACID

 ACID stands for **Atomicity**, **Consistency**, **Isolation**, and **Durability**. These four key properties define how a transaction should be processed in a reliable and predictable manner, ensuring that the database remains consistent, even in cases of failures or concurrent accesses.

 ### ATOMICITY :"All or Nothing"

 **Atomicity** ensures that a transaction is atomic, it means that either the entire transaction completes fully or doesn't execute at all.
 #### Scenario :

 **Ann transfer 100 shillings to Morris** 

 **Example of Atomicity is** :Let say If system crashes after deducting from Alice but before adding to Bob, the whole transaction is rolled back.

### 2. Consistency : Maintaining Valid Data States

Consistency ensures that a database remains in a valid state before and after a transaction.
Let us use the above scenario.

**Example of consistency** : Total money in the system stays the same before and after the transaction.

### 3. Isolation : Ensuring Concurrent Transactions Don't Interfere

This property ensures that multiple transactions can occur concurrently without leading to the inconsistency of the database state. Transactions occur independently without interference. 

**Example**: If two people transfer money at the same time, the final balances should be as if one ran after the other.

### 4. Durability : Persisting Changes
This property ensures that once the transaction has completed execution, the updates and modifications to the database are stored in and written to disk and they persist even if a system failure occurs.

**Example** : After successfully transferring money from Account A to Account B, the changes are stored on disk. Even if there is a crash immediately after the commit, the transfer details will still be intact when the system recovers, ensuring durability.
