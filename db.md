### 1. **What is Normalization? Explain Different Normal Forms with Examples**

**Normalization** is the process of organizing data in a database to reduce redundancy and improve data integrity. It involves dividing large tables into smaller, more manageable pieces and defining relationships between them. The main goal is to eliminate data anomalies (insertion, update, and deletion anomalies) and ensure data dependencies make sense.

**Normal Forms:**

1. **First Normal Form (1NF):**
   - A table is in 1NF if it contains atomic (indivisible) values and each column contains only one value per row.
   - Example:
     ```
     Before 1NF:
     Student (ID, Name, Courses)
     1, John, Math, Science
     
     After 1NF:
     Student (ID, Name, Course)
     1, John, Math
     1, John, Science
     ```

2. **Second Normal Form (2NF):**
   - A table is in 2NF if it is in 1NF and all non-key attributes are fully functionally dependent on the primary key.
   - Example:
     ```
     Before 2NF:
     Orders (OrderID, ProductID, ProductName, Quantity)
     
     After 2NF:
     Orders (OrderID, ProductID, Quantity)
     Products (ProductID, ProductName)
     ```

3. **Third Normal Form (3NF):**
   - A table is in 3NF if it is in 2NF and there are no transitive dependencies (non-key attributes depend only on the primary key).
   - Example:
     ```
     Before 3NF:
     Employees (EmployeeID, Name, Department, DepartmentLocation)
     
     After 3NF:
     Employees (EmployeeID, Name, DepartmentID)
     Departments (DepartmentID, DepartmentLocation)
     ```

4. **Boyce-Codd Normal Form (BCNF):**
   - A table is in BCNF if it is in 3NF and for every functional dependency, the determinant is a candidate key.
   - Example:
     ```
     Before BCNF:
     Enrollments (StudentID, CourseID, Instructor)
     
     After BCNF:
     Enrollments (StudentID, CourseID)
     CourseInstructors (CourseID, Instructor)
     ```

5. **Fourth Normal Form (4NF):**
   - A table is in 4NF if it is in BCNF and has no multi-valued dependencies.
   - Example:
     ```
     Before 4NF:
     Employees (EmployeeID, Skill, Language)
     
     After 4NF:
     EmployeeSkills (EmployeeID, Skill)
     EmployeeLanguages (EmployeeID, Language)
     ```

---

### 2. **Difference Between Primary Key and Unique Key**

| **Primary Key**                          | **Unique Key**                          |
|------------------------------------------|-----------------------------------------|
| A primary key uniquely identifies each record in a table. | A unique key also uniquely identifies each record but can have one NULL value. |
| Only one primary key is allowed per table. | Multiple unique keys can exist in a table. |
| It cannot contain NULL values.            | It can contain one NULL value.          |
| It is used to enforce entity integrity.   | It is used to enforce unique data.      |

---

### 3. **ACID Properties in DBMS**

ACID properties ensure reliable transaction processing in a database:

1. **Atomicity:**
   - Ensures that a transaction is treated as a single unit. Either all operations within the transaction are completed, or none are.
   - Example: If a bank transfer fails midway, the entire transaction is rolled back.

2. **Consistency:**
   - Ensures that a transaction brings the database from one valid state to another, maintaining data integrity.
   - Example: The total balance before and after a transfer must remain consistent.

3. **Isolation:**
   - Ensures that concurrent transactions do not interfere with each other.
   - Example: Two users transferring money from the same account should not affect each other’s transactions.

4. **Durability:**
   - Ensures that once a transaction is committed, it remains permanent, even in the event of a system failure.
   - Example: After a successful transfer, the changes are saved permanently.

---

### 4. **Difference Between DELETE, TRUNCATE, and DROP in SQL**

| **DELETE**                                | **TRUNCATE**                            | **DROP**                                |
|-------------------------------------------|-----------------------------------------|-----------------------------------------|
| Removes specific rows from a table based on a condition. | Removes all rows from a table.          | Deletes the entire table structure and data. |
| Can be rolled back (if used with a transaction). | Cannot be rolled back.                  | Cannot be rolled back.                  |
| Does not reset the auto-increment counter. | Resets the auto-increment counter.      | Removes the table and its structure.    |
| Slower than TRUNCATE as it logs each row.  | Faster than DELETE as it deallocates data pages. | Removes the table and its metadata.    |

---

### 5. **Types of Joins in SQL with Examples**

1. **INNER JOIN:**
   - Returns only the rows that have matching values in both tables.
   - Example:
     ```sql
     SELECT Employees.Name, Departments.DepartmentName
     FROM Employees
     INNER JOIN Departments ON Employees.DepartmentID = Departments.DepartmentID;
     ```

2. **LEFT JOIN (or LEFT OUTER JOIN):**
   - Returns all rows from the left table and the matched rows from the right table. If no match, NULL values are returned for columns from the right table.
   - Example:
     ```sql
     SELECT Employees.Name, Departments.DepartmentName
     FROM Employees
     LEFT JOIN Departments ON Employees.DepartmentID = Departments.DepartmentID;
     ```

3. **RIGHT JOIN (or RIGHT OUTER JOIN):**
   - Returns all rows from the right table and the matched rows from the left table. If no match, NULL values are returned for columns from the left table.
   - Example:
     ```sql
     SELECT Employees.Name, Departments.DepartmentName
     FROM Employees
     RIGHT JOIN Departments ON Employees.DepartmentID = Departments.DepartmentID;
     ```

4. **FULL JOIN (or FULL OUTER JOIN):**
   - Returns all rows when there is a match in either the left or right table. If no match, NULL values are returned for missing sides.
   - Example:
     ```sql
     SELECT Employees.Name, Departments.DepartmentName
     FROM Employees
     FULL JOIN Departments ON Employees.DepartmentID = Departments.DepartmentID;
     ```

5. **CROSS JOIN:**
   - Returns the Cartesian product of the two tables (all possible combinations of rows).
   - Example:
     ```sql
     SELECT Employees.Name, Departments.DepartmentName
     FROM Employees
     CROSS JOIN Departments;
     ```

6. **SELF JOIN:**
   - A table is joined with itself. Useful for comparing rows within the same table.
   - Example:
     ```sql
     SELECT A.Name AS Employee1, B.Name AS Employee2
     FROM Employees A, Employees B
     WHERE A.DepartmentID = B.DepartmentID AND A.ID <> B.ID;
     ```

---

### 6. **How Does Indexing Work in a Database? What Are Clustered and Non-Clustered Indexes?**

**Indexing** is a database optimization technique used to speed up data retrieval operations. An index is a data structure (like a B-tree or hash table) that stores a subset of the table's columns and a pointer to the corresponding row. Indexes are used to quickly locate data without scanning the entire table.

#### **Clustered Index:**
- A clustered index determines the physical order of data in a table. There can be only one clustered index per table because the data rows themselves are stored in the order of the clustered index.
- Example: A phone book is ordered by last name, so the last name acts as a clustered index.
- Syntax:
  ```sql
  CREATE CLUSTERED INDEX idx_employee_id ON Employees (EmployeeID);
  ```

#### **Non-Clustered Index:**
- A non-clustered index is a separate structure that stores a copy of the indexed columns and a pointer to the actual data rows. Multiple non-clustered indexes can exist on a table.
- Example: An index at the back of a book that points to specific pages.
- Syntax:
  ```sql
  CREATE NONCLUSTERED INDEX idx_employee_name ON Employees (Name);
  ```

**Key Differences:**

| **Clustered Index**                     | **Non-Clustered Index**                 |
|-----------------------------------------|-----------------------------------------|
| Determines the physical order of data.  | Does not affect the physical order of data. |
| Only one clustered index per table.     | Multiple non-clustered indexes per table. |
| Faster for range queries.               | Slower for range queries compared to clustered indexes. |
| Requires no extra storage for data.     | Requires additional storage for the index structure. |

---

### 7. **Differences Between Relational and Non-Relational Databases**

| **Relational Databases (RDBMS)**        | **Non-Relational Databases (NoSQL)**    |
|-----------------------------------------|-----------------------------------------|
| Data is stored in tables with rows and columns. | Data is stored in collections, documents, key-value pairs, or graphs. |
| Uses SQL (Structured Query Language).   | Uses query languages specific to the database (e.g., MongoDB uses JSON-like queries). |
| Schema is fixed and predefined.         | Schema is dynamic and flexible.         |
| Supports ACID properties for transactions. | May not fully support ACID properties (depends on the database). |
| Examples: MySQL, PostgreSQL, Oracle.    | Examples: MongoDB, Cassandra, Redis.   |
| Best for structured data and complex queries. | Best for unstructured or semi-structured data and scalability. |

---

### 8. **What Are Views in SQL? How Are They Different from Tables?**

**Views** are virtual tables created by a query. They do not store data physically but display data from one or more tables based on the query.

#### **Key Differences:**

| **Views**                               | **Tables**                              |
|-----------------------------------------|-----------------------------------------|
| Do not store data physically.           | Store data physically.                  |
| Created using a SQL query.              | Created using `CREATE TABLE`.           |
| Can be updated (if the view is updatable). | Can always be updated.                 |
| Used for simplifying complex queries, security, or abstraction. | Used for storing actual data. |
| Example:
```sql
CREATE VIEW EmployeeView AS
SELECT Name, Department
FROM Employees
WHERE Department = 'HR';
```

---

### 9. **How Does Transaction Management Work in DBMS? Explain with an Example**

**Transaction Management** ensures that a set of database operations are executed reliably and consistently. A transaction is a sequence of operations performed as a single logical unit of work.

#### **Steps in Transaction Management:**
1. **Begin Transaction:** Marks the start of a transaction.
2. **Execute Operations:** Perform the required database operations (e.g., INSERT, UPDATE, DELETE).
3. **Commit:** Saves the changes permanently if all operations succeed.
4. **Rollback:** Reverts the changes if any operation fails.

#### **Example:**
```sql
BEGIN TRANSACTION;

UPDATE Accounts SET Balance = Balance - 100 WHERE AccountID = 1;
UPDATE Accounts SET Balance = Balance + 100 WHERE AccountID = 2;

-- If both updates succeed:
COMMIT;

-- If any update fails:
ROLLBACK;
```

#### **ACID Properties in Transaction Management:**
- **Atomicity:** Ensures the transaction is completed entirely or not at all.
- **Consistency:** Ensures the database remains in a valid state.
- **Isolation:** Ensures concurrent transactions do not interfere.
- **Durability:** Ensures committed changes are permanent.

---

### 10. **What Are Different Types of Anomalies in DBMS? How Do They Arise?**

Anomalies are problems that occur in a database due to poor design, leading to inconsistencies or redundancies. They arise primarily in unnormalized databases.

#### **Types of Anomalies:**

1. **Insertion Anomaly:**
   - Occurs when you cannot insert data into a table without adding unrelated data.
   - Example: In a table `Students (StudentID, Name, Course)`, you cannot add a new student if they haven’t enrolled in a course yet.

2. **Update Anomaly:**
   - Occurs when updating one record requires updating multiple rows, leading to inconsistencies.
   - Example: In a table `Employees (EmployeeID, Name, Department, DepartmentLocation)`, updating the location of a department requires updating all rows for that department.

3. **Deletion Anomaly:**
   - Occurs when deleting a record unintentionally deletes unrelated data.
   - Example: In a table `Orders (OrderID, ProductID, ProductName)`, deleting an order for a product also deletes the product name, making it unavailable for other orders.

#### **How Do They Arise?**
- Anomalies arise due to **redundant data** and **improper dependencies** in unnormalized tables.
- They are resolved by **normalizing** the database into higher normal forms (1NF, 2NF, 3NF, etc.).

#### **Example:**
```
Unnormalized Table:
Students (StudentID, Name, Course, Instructor)

Insertion Anomaly: Cannot add a new instructor without assigning them to a student.
Update Anomaly: Changing an instructor's name requires updating multiple rows.
Deletion Anomaly: Deleting a student removes the instructor's information.
```

By normalizing the table into `Students (StudentID, Name)` and `Courses (CourseID, Course, Instructor)`, these anomalies can be eliminated.

---

### 11. **How Does Deadlock Occur in DBMS? How Can We Prevent It?**

**Deadlock** occurs when two or more transactions are waiting indefinitely for one another to release locks on resources, creating a cycle of dependencies. 

#### **Example of Deadlock:**
- Transaction T1 locks Row A and requests Row B.
- Transaction T2 locks Row B and requests Row A.
- Both transactions wait indefinitely for each other to release locks.

#### **How to Prevent Deadlock:**
1. **Lock Ordering:**
   - Ensure all transactions acquire locks in a predefined order.
   - Example: Always lock Row A before Row B.

2. **Timeouts:**
   - Set a maximum wait time for a transaction to acquire a lock. If the timeout is reached, the transaction is rolled back.

3. **Deadlock Detection and Resolution:**
   - Use a wait-for graph to detect cycles (deadlocks). If a cycle is found, abort one of the transactions to break the deadlock.

4. **Avoiding Locks:**
   - Use techniques like MVCC (Multiversion Concurrency Control) to avoid locking altogether.

---

### 12. **Explain the Concept of Multiversion Concurrency Control (MVCC)**

**MVCC** is a concurrency control method that allows multiple transactions to access the same data simultaneously without blocking each other. It achieves this by maintaining multiple versions of a data item.

#### **How MVCC Works:**
- Each transaction sees a snapshot of the database at the time it started.
- When a transaction updates a row, a new version is created instead of overwriting the existing data.
- Older versions are retained until no active transactions need them.

#### **Advantages of MVCC:**
- Improves performance by reducing lock contention.
- Allows readers to access data without blocking writers and vice versa.

#### **Example:**
- Transaction T1 reads Row A (Version 1).
- Transaction T2 updates Row A, creating Version 2.
- Transaction T1 continues to read Version 1, while Transaction T2 works with Version 2.

#### **Databases Using MVCC:**
- PostgreSQL, Oracle, and MySQL (with InnoDB).

---

### 13. **What Are the Different Types of Database Architectures?**

1. **Single-Tier Architecture:**
   - The database and application reside on the same system.
   - Example: A local desktop database like Microsoft Access.

2. **Two-Tier Architecture:**
   - Consists of a client and a server.
   - The client handles the user interface, and the server manages the database.
   - Example: A Java application connecting to a MySQL database.

3. **Three-Tier Architecture:**
   - Consists of three layers: Presentation, Application, and Database.
   - The presentation layer handles the user interface, the application layer contains business logic, and the database layer stores data.
   - Example: A web application with a front-end (HTML/JavaScript), back-end (Node.js), and database (MongoDB).

4. **N-Tier Architecture:**
   - Extends the three-tier architecture by adding more layers (e.g., caching, messaging).
   - Example: A microservices-based application with multiple services interacting with a central database.

5. **Distributed Database Architecture:**
   - Data is spread across multiple physical locations, often managed by different DBMS instances.
   - Example: Google Spanner, Cassandra.

---

### 14. **Explain Two-Phase Locking (2PL) and Its Role in Concurrency Control**

**Two-Phase Locking (2PL)** is a concurrency control protocol that ensures serializability by dividing a transaction's lock acquisition and release into two phases:

1. **Growing Phase:**
   - A transaction acquires all the locks it needs but cannot release any locks.
   - Example: Transaction T1 locks Row A and then Row B.

2. **Shrinking Phase:**
   - A transaction releases locks but cannot acquire new ones.
   - Example: Transaction T1 releases Row B and then Row A.

#### **Role in Concurrency Control:**
- Ensures that transactions do not interfere with each other, maintaining data consistency.
- Prevents anomalies like dirty reads, lost updates, and unrepeatable reads.

#### **Strict 2PL:**
- A variant where all locks are released only after the transaction commits or aborts.
- Ensures higher isolation and avoids cascading rollbacks.

#### **Example:**
```sql
-- Transaction T1
BEGIN;
LOCK Row A;
LOCK Row B;
-- Perform operations
COMMIT; -- Release all locks
```

---

### 15. **How Does Query Optimization Work in a Database System?**

**Query Optimization** is the process of selecting the most efficient execution plan for a query. It involves analyzing multiple ways to execute a query and choosing the one with the lowest cost (e.g., CPU, I/O, memory usage).

#### **Steps in Query Optimization:**
1. **Parsing:**
   - The query is parsed to check for syntax errors and convert it into an internal representation (e.g., a parse tree).

2. **Logical Optimization:**
   - The query is transformed into an equivalent but more efficient form using rules like predicate pushdown, join reordering, and eliminating redundant operations.

3. **Physical Optimization:**
   - The optimizer evaluates different execution plans (e.g., index scans, full table scans, join algorithms) and estimates their cost.

4. **Execution Plan Selection:**
   - The optimizer selects the plan with the lowest estimated cost.

5. **Execution:**
   - The database engine executes the chosen plan.

#### **Techniques Used in Query Optimization:**
- **Indexing:** Uses indexes to speed up data retrieval.
- **Join Algorithms:** Chooses the best join method (e.g., nested loop, hash join, merge join).
- **Cost Estimation:** Estimates the cost of each operation based on statistics (e.g., table size, index selectivity).

#### **Example:**
```sql
SELECT * FROM Employees WHERE Department = 'HR' ORDER BY Salary DESC;
```
- The optimizer might:
  - Use an index on the `Department` column to filter rows.
  - Use an index on the `Salary` column to avoid sorting.

#### **Tools for Query Optimization:**
- **EXPLAIN:** Provides insights into the execution plan of a query.
  ```sql
  EXPLAIN SELECT * FROM Employees WHERE Department = 'HR';
  ```
- **Query Profiling:** Measures the actual performance of a query.

By optimizing queries, databases can significantly improve performance and reduce resource usage.