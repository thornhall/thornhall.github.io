---
title:  "Design Components: Databases"
date:   2024-11-25 08:00:00 -0600
excerpt: "What is a database?"
---
## What is a Database?
A database is software responsible for saving and retrieving data from a durable data source, such as a hard drive. 

## What is ACID?
ACID is an acronym used to describe qualities of a database. Some databases, such as relational databases like PostgreSQL, achieve a high level of ACID compliance. Not all databases achieve the same level of ACID compliance. For example, some databases sacrifice consistency for availability. ACID means:

1. **Atomicity**: Transactions are all-or-nothing.
2. **Consistency**: The system ensures a valid state before and after a transaction.
3. **Isolation**: Transactions do not interfere with each other.
4. **Durability**: Confirmed transactions survive permanently.
 
## What is a Transaction?
In databases, a transaction is any set of operations on the database that must be performed as one logical unit. Operations include reads, writes, and deletes. 

### Example Database Transaction: Bank Transfer Between Accounts
Consider a scenario where a customer wants to transfer $500 from their checking account to their savings account. The following operations are involved:

- Debit $500 from the checking account.
- Credit $500 to the savings account.

#### Why We Need a Transaction Here
If the operation to remove $500 from the checking account fails, we don't want to credit $500 to the savings account, as this would quite literally be an infinite money glitch. 

Similarly, if we succeed in subtracting $500 from the checking account, but we fail to credit the $500 to the savings account, we would want to "undo" the subtraction so the customer does not lose their money. 

Transactions guarantee that both of these operations are performed atomically. If one operation in a transaction fails, the entire block fails, guaranteeing we don't cause data inconsistencies due to errors.

#### Example Steps of a Transaction
1. Start Transaction: Begin the operation.
2. Check Balance: Ensure the checking account has at least $500.
3. Debit: Deduct $500 from the checking account.
4. Credit: Add $500 to the savings account.
5. Commit Transaction: Confirm the entire operation as complete and make the changes permanent.

Note that in a **transaction**, if any steps fail, all steps would fail.

## Database Types

### SQL Database
A SQL database is also known as a relational database. SQL databases enforce strict schemas on the data stored in them. Data is stored in a table-like structure of columns and rows. SQL stands for **Structured Query Language**. Data stored in SQL databases is often related, and we can store references to related data using **foreign keys**. SQL databases are best used when strong ACID guarantees are needed.

#### SQL Database Example
Here's an example relational database table. In this case, we're storing information about employees. The columns define what data is stored for each employee, and each row represents one employee.

Employees Table

| id  | name     | department   | salary  |
|------|----------|--------------|---------|
| 1    | Alice    | HR           | 45000   |
| 2    | Bob      | Engineering  | 55000   |
| 3    | Charlie  | Sales        | 40000   |
| 4    | Diana    | Engineering  | 60000   |
| 5    | Eve      | HR           | 47000   |

Here's how we might read that data using SQL:

```sql
SELECT name, salary 
FROM employees 
WHERE department = 'Engineering';
```

This would return the following results:

| name     | salary  |
|----------|---------|
| Bob      | 55000   |
| Diana    | 60000   |

#### SQL Database Example: Foreign Key and JOIN
Sometimes we have multiple tables for different types of data. To expand on the employee example, imagine we are storing the Department object in a different table, the Department table. In this case, now the Employee table is storing a reference ID to a row in the department table (a foreign key).

Employee Table

| id  | name     | department_id | salary  |
|-----|----------|---------------|---------|
| 1   | Alice    | 1             | 45000   |
| 2   | Bob      | 2             | 55000   |
| 3   | Charlie  | 3             | 40000   |
| 4   | Diana    | 2             | 60000   |
| 5   | Eve      | 1             | 47000   |

Department Table

| id  | name           | average_salary |
|-----|----------      |--------------- |
| 1   | Engineering    |  45000         |
| 2   | HR             |  50000         |
| 3   | Sales          |  50000         |


What if we want to query for all Engineering employees in this example?

```sql
SELECT employees.name, employees.salary
FROM employees
JOIN departments
ON employees.department_id = departments.id
WHERE departments.name = 'Engineering';
```

This would return this result: 

| name     | salary  |
|----------|---------|
| Alice    | 45000   |
| Eve      | 47000   |

### NoSQL Databases
SQL databases are a popular choice. However, scaling them can be complicated, requiring partitioning and/or sharding. Furthermore, the schemas are not flexible. When data is not heavily related, does not follow a strict schema, or does not require strong ACID compliance, it is worth considering a NoSQL database. There are databases that do not use SQL out there, and next we're going to go over a few. These databases are classified as NoSQL databases.

#### Document Store DBs
Example: MongoDB.

Document databases are great for data that does not have complex relationships or structure. They're designed to scale horizontally and have high throughput. Data is usually stored as JSON. 

Here's what MongoDB data might look like: 

```json
[
  {
    "name": "Alice",
    "age": 25,
    "city": "New York",
    "hobbies": ["reading", "hiking"]
  },
  {
    "name": "Bob",
    "age": 30,
    "city": "San Francisco",
    "hobbies": ["gaming", "cooking"]
  },
  {
    "name": "Charlie",
    "age": 35,
    "city": "Los Angeles",
    "hobbies": ["sports", "traveling"]
  }
]
```

in order to query for one person with a given name:

```javascript
db.users.findOne({ name: "Alice" });
```

this would return the following results: 

```json
{
  "name": "Alice",
  "age": 25,
  "city": "New York",
  "hobbies": ["reading", "hiking"]
}
```

#### Key-Value Store DBs
Example: DynamoDB.

Key value stores are databases where each stored object has a unique key and value. You could think if it like a distributed hash map. They're ideal for read-heavy workflows and for data that doesn't require strict schemas. 

#### DynamoDB Example
DynamoDB tables have a partition key and a sort key. For this example we will have the following:


Partition Key: userId


Sort Key: lastName

Here's an example DynamoDB Users table.

| userId | lastName | age | city           | hobbies                  |
|--------|----------|-----|----------------|--------------------------|
| 1      | Smith    | 25  | New York       | ["reading", "hiking"]    |
| 2      | Johnson  | 30  | San Francisco  | ["gaming", "cooking"]    |
| 3      | Brown    | 35  | Los Angeles    | ["sports", "traveling"]  |

Here's how we might query for user with ID 1 programmatically:

```javascript
const AWS = require("aws-sdk");
const dynamoDB = new AWS.DynamoDB.DocumentClient();

const params = {
  TableName: "Users",
  KeyConditionExpression: "userId = :userId",
  ExpressionAttributeValues: {
    ":userId": "1",
  },
};

dynamoDB.query(params, (err, data) => {
  if (err) {
    console.error("Unable to query. Error:", JSON.stringify(err, null, 2));
  } else {
    console.log("Query succeeded:", JSON.stringify(data, null, 2));
  }
});
```

Here's an example result:

```json
{
  "Items": [
    {
      "userId": "1",
      "lastName": "Smith",
      "age": 25,
      "city": "New York",
      "hobbies": ["reading", "hiking"]
    }
  ],
  "Count": 1,
  "ScannedCount": 1
}
```

#### Column-Family DBs
Example: Apache Cassandra. 

Cassandra is highly write-optimized and should be used for workflows that are otherwise having trouble scaling for large amounts of writes.

#### Other DB Types
Some other types of DBs include time-series, graph, and search DBs. 

## Key Takeaways
- Databases are used to store data permanently.
- The primary types of databases are SQL and NoSQL. 
- Different databases have different ACID guarantees.
- The type of database you pick should be dependent upon your data's structure and access patterns.
- Relational databases should be used when strong ACID guarantees are needed. However, some NoSQL databases do have configurations that make them more ACID compliant.