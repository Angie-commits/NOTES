# ACID Properties and Normalization 
## ACID Properties

ACID Properties are the fundamental principles that ensure reliable processing of database transactions. ACID stands for *Atomicity, **Consistency, **Isolation, and **Durability*.

### Atomicity
*"All or Nothing"* - This property ensures that a transaction is treated as a single unit of operation. When an update occurs to a database, either all operations within the transaction are completed successfully, or none of them are applied.

- A transaction either *commits* (all changes are saved) or *aborts* (no changes are made)
- No partial updates are allowed to persist in the database
- Ensures data integrity by preventing incomplete operations

### Consistency
This property ensures that any changes to values in a database instance are consistent with changes to other values in the same instance.

- Database must remain in a valid state before and after any transaction
- All database rules, constraints, and triggers must be satisfied
- Prevents corruption of data relationships

### Isolation
Isolation is crucial when multiple transactions occur concurrently. It ensures that concurrent transactions do not interfere with each other.

*Key Concept: Serializability*
- Transactions are serializable when the final database state is the same whether transactions are executed:
  - In serial order (one after another)
  - In an interleaved fashion (concurrently)
- Prevents issues like dirty reads, phantom reads, and non-repeatable reads

### Durability
This property ensures that once a transaction is committed, its updates are permanently stored and will survive system failures.

- Committed transaction updates must never be lost
- System must be able to recover committed transactions even if:
  - The system crashes
  - Storage media fails
- Typically implemented through transaction logs and backup mechanisms

## Normalization

Normalization is a systematic technique used to organize data in a database to reduce redundancy and improve data integrity.

### Purpose of Normalization

1. *Remove data redundancy* - Eliminate duplicate data storage
2. *Ensure proper data dependencies* - Maintain logical relationships between data

### Database Anomalies

Without normalization, three types of anomalies can occur:

*Example Student Table:*
 ![student table 1](/images/student%20table%201.jpg) 

#### Update Anomaly
- To update the address of a student who appears multiple times, you must update all rows
- Failure to update all instances leads to data inconsistency

#### Insertion Anomaly
- For new student admission, if no subject is chosen yet, NULL values must be inserted
- Creates incomplete records in the database

#### Deletion Anomaly
- If a student drops their only subject and that row is deleted, the entire student record is lost
- Loss of important student information

## Normal Forms

Normalization rules are divided into several normal forms, each building upon the previous one.

### First Normal Form (1NF)

*Rules:*
- No two rows should contain repeating data
- Each table should be organized into rows
- Each row must have a primary key that makes it unique
- Each cell should contain only atomic (indivisible) values

*Example:*

*Not in 1NF:*
![no normal form](/images/no%20normal%20form.jpg)

 *in 1NF:*
 ![Ffirst normal form](/images/first%20normal%20form.jpg)

*Note:* While 1NF eliminates repeating groups, it increases data redundancy.

### Second Normal Form (2NF)

*Rules:*
- Must meet all requirements of 1NF
- Remove partial dependencies on the primary key
- For tables with concatenated primary keys, each non-key column must depend on the entire primary key
- Create separate tables for subsets of data that apply to multiple rows
- Use foreign keys to create relationships between tables

*Example:*

*Student Table (2NF):*
![2nd normal form](/images/2nd%20normal%20form.jpg)

*Subject Table (2NF):*
![subject table](/images/subject%20table.jpg)

*Benefits:* Eliminates update anomalies and reduces redundancy.

### Third Normal Form (3NF)

*Rules:*
- Must be in 2NF
- Contains no transitive dependencies
- If A â B and B â C, then A â C (this transitive dependency should be eliminated)

*Transitive Dependency Example:*
If Student_ID â Department and Department â Department_Head, then Student_ID â Department_Head is a transitive dependency.

*Example:*

*Before 3NF:*
![new table](/images/new%20table.jpg)


*After 3NF:*

*Student_Details Table:*
![student det](/images/student%20det.jpg)

*Address Table:*
![address table](/images/address%20table.jpg)

*Advantages:*
- Reduces data duplication
- Achieves better data integrity
- Eliminates transitive dependencies

### Boyce-Codd Normal Form (BCNF)

BCNF is a higher version of 3NF that deals with certain anomalies not handled by 3NF.

*Rules:*
- Must be in 3NF
- For each functional dependency (X â Y), X should be a super key
- A 3NF table that does not have multiple overlapping candidate keys is in BCNF

*When to use BCNF:*
- When you have multiple candidate keys
- When candidate keys are composite
- When candidate keys overlap

*Benefits:*
- Eliminates all anomalies related to functional dependencies
- Provides the highest level of normalization for most practical purposes