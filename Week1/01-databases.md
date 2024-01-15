# Relational Databases

Recap [basics of the relational databases](https://github.com/mattpe/ucad/blob/main/05-databases.md)

## Example database

Starting point is [the database](https://github.com/mattpe/ucad/blob/main/06-database-api.md) created on the previous course ([download the script](https://raw.githubusercontent.com/mattpe/ucad/main/assets/media-db.sql)).

## Transactions

SQL transactions are used to ensure that a series of SQL statements are executed in a way that either all of them are committed (i.e., their changes are saved to the database), or none of them are (in case of an error or failure). Transactions are crucial for maintaining the integrity of a database.

NOTE: All [database storage engines](https://dev.mysql.com/doc/refman/8.2/en/storage-engines.html) do not support transactions. For example, MySQL MyISAM engine does not support transactions, whereas the default InnoDB engine supports transactions. You can check the engine of a table using the following command: `SHOW TABLE STATUS WHERE Name = '<TableName>';`.

**Basic Transaction**: Suppose you're updating user details and want to ensure that either all updates are applied, or none at all if an error occurs.

```sql
START TRANSACTION;

UPDATE Users SET email = 'newemail@example.com' WHERE user_id = 1;
UPDATE Users SET username = 'newusername' WHERE user_id = 1;

COMMIT;
```

In this example, if both UPDATE statements are executed without any error, the COMMIT command will save the changes. If any error occurs in between, no changes will be saved permanently. The transaction can be rolled back to undo any changes made in the transaction using the `ROLLBACK;` command.

**Transaction with ROLLBACK**: In this example, we handle a situation where you might want to explicitly rollback the transaction in case of an error.

```sql
START TRANSACTION;

INSERT INTO Users (username, password, email, user_level_id) VALUES ('User123', 'pass123', 'user123@example.com', 2);
INSERT INTO MediaItems (user_id, filename, filesize, media_type, title) VALUES (LAST_INSERT_ID(), 'image.jpg', 1024, 'image/jpeg', 'User123 Image');

-- Suppose you want to rollback the transaction if the second INSERT fails
ROLLBACK;
```

In this example, if an error occurs during either of the INSERT operations, the transaction is rolled back, undoing any changes made in the transaction. Otherwise, it's committed.

**Using a SAVEPOINT** allows part of a transaction to be rolled back without affecting the rest.

```sql
START TRANSACTION;

INSERT INTO Users (username, password, email, user_level_id) VALUES ('User456', 'pass456', 'user456@example.com', 2);
SAVEPOINT UserInsertSave;

UPDATE Users SET email = 'updatedemail@example.com' WHERE username = 'User456';

-- Suppose you want to rollback just the email update
ROLLBACK TO SAVEPOINT UserInsertSave;

COMMIT;
```

In this scenario, if you decide to undo the email update, you can rollback to the savepoint UserInsertSave, which undoes the UPDATE but keeps the INSERT.

Notes:

- The exact syntax and capabilities may vary depending on the SQL database you are using (MySQL, PostgreSQL, SQL Server, etc.).
- It's important to handle errors and rollbacks appropriately to maintain the integrity of your data.
- In a production environment, transactions should be combined with proper error handling and possibly with isolation level settings depending on the requirements for consistency and concurrent access.

Extra reading: <https://www.freecodecamp.org/news/how-to-use-mysql-transactions/>

### Deleting data with multiple references

When deleting data from a table that is referenced by other tables, you easily run in to foreign key constraint errors. This is because the database system needs to ensure that the data is not referenced by any other tables before it can be deleted.

Example SQL script to delete a user with `user_id = 1` and all related data:

```sql
-- Start a transaction
START TRANSACTION;

-- Delete related data from Comments where the user_id is 1
DELETE FROM Comments WHERE user_id = 1;

-- Delete related data from Likes where the user_id is 1
DELETE FROM Likes WHERE user_id = 1;

-- Delete related data from Ratings where the user_id is 1
DELETE FROM Ratings WHERE user_id = 1;

-- Since MediaItems are also related to the user, first delete data from tables that reference MediaItems

-- Delete related data from Comments where the media_id belongs to user_id 1
DELETE FROM Comments WHERE media_id IN (SELECT media_id FROM MediaItems WHERE user_id = 1);

-- Delete related data from Likes where the media_id belongs to user_id 1
DELETE FROM Likes WHERE media_id IN (SELECT media_id FROM MediaItems WHERE user_id = 1);

-- Delete related data from Ratings where the media_id belongs to user_id 1
DELETE FROM Ratings WHERE media_id IN (SELECT media_id FROM MediaItems WHERE user_id = 1);

-- Delete related data from MediaItemTags where the media_id belongs to user_id 1
DELETE FROM MediaItemTags WHERE media_id IN (SELECT media_id FROM MediaItems WHERE user_id = 1);

-- Now, delete the media items themselves
DELETE FROM MediaItems WHERE user_id = 1;

-- Finally, delete the user
DELETE FROM Users WHERE user_id = 1;

-- Commit the transaction
COMMIT;
```

Notes:

- Transaction: This script uses a transaction to ensure that all deletes are treated as a single unit of work. If an error occurs during one of the delete statements, the transaction can be rolled back to prevent partial deletion and maintain database integrity.
- Referential Integrity: The order of deletion is important. You must delete from the tables with foreign key references first before deleting the main record in the Users table.
- Testing: It's crucial to thoroughly test such a script in a safe, non-production environment before applying it to your live database to ensure it behaves as expected and doesn't accidentally delete more data than intended.
- Backup: Always ensure you have a backup of your database before performing bulk delete operations.

#### CASCADE Delete

If your foreign keys were set up with CASCADE delete, some of these steps would be automatically handled by the database. However, it's important to understand and control these processes explicitly, especially in complex databases.

To perform the same operation with CASCADE delete setup, you first need to ensure that the foreign key constraints in your database are defined with the ON DELETE CASCADE action. This setup automatically deletes all related records in the child tables when the corresponding record in the parent table is deleted.

Given your database structure, you would define the foreign key constraints with ON DELETE CASCADE like this:

```sql
-- Example for creating the Comments table with CASCADE DELETE
CREATE TABLE Comments (
    comment_id INT AUTO_INCREMENT PRIMARY KEY,
    media_id INT NOT NULL,
    user_id INT NOT NULL,
    comment_text TEXT NOT NULL,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    FOREIGN KEY (media_id) REFERENCES MediaItems(media_id) ON DELETE CASCADE,
    FOREIGN KEY (user_id) REFERENCES Users(user_id) ON DELETE CASCADE
);
```

You would need to set up similar ON DELETE CASCADE constraints for Likes, Ratings, MediaItemTags, and any other table that references Users or MediaItems.

Once the CASCADE delete is set up, deleting a user with all related data becomes much simpler. You only need to delete the user record, and the database will automatically take care of deleting all related records in the child tables.

Here's the simplified delete operation: `DELETE FROM Users WHERE user_id = 1;`

This single command will delete the user with user_id = 1 and all data related to this user in the Comments, Likes, Ratings, MediaItems, and any other table that has a foreign key referencing Users with ON DELETE CASCADE action.

Notes:

- Use with Caution: CASCADE deletes can be powerful but dangerous, as they will automatically delete all related data. It's crucial to fully understand the relationships in your database to avoid unintended data loss.
- Backup: Always make sure to backup your database before running delete operations by hand, especially ones that can affect multiple tables like CASCADE deletes.
- Testing: As always, test your CASCADE delete setup in a non-production environment to ensure it behaves as expected.
- Restructuring Existing Tables: If you need to add ON DELETE CASCADE to an existing foreign key constraint, you typically have to drop and recreate the constraint. Be careful with this in a production environment.

## Views

SQL views are virtual tables that are based on the result set of an SQL query. They encapsulate complex queries, provide a way to simplify data access, and can be used for security purposes by restricting access to a predetermined set of rows and/or columns. Here are some examples illustrating the creation and usage of views:

**Simple View**: Suppose you frequently need to retrieve the username and email of all users in your Users table. You can create a view to simplify this:

```sql
CREATE VIEW UserContactInfo AS
SELECT username, email
FROM Users;
```

You can then retrieve this data simply by querying the view:

```sql
SELECT * FROM UserContactInfo;
```

**View with a Join**: If you need to frequently access media items along with the user's name who uploaded them, you can create a view that joins the Users and MediaItems tables:

```sql
CREATE VIEW MediaItemDetails AS
SELECT MediaItems.media_id, MediaItems.title, Users.username
FROM MediaItems
JOIN Users ON MediaItems.user_id = Users.user_id;
```

Query the view like this:

```sql
SELECT * FROM MediaItemDetails;
```

**View with Aggregation**: To get a count of how many media items each user has uploaded, you can create a view that uses a grouping and aggregation:

```sql
CREATE VIEW UserMediaCount AS
  SELECT Users.user_id, Users.username, COUNT(MediaItems.media_id) AS MediaCount
  FROM Users
  LEFT JOIN MediaItems ON Users.user_id = MediaItems.user_id
  GROUP BY Users.user_id;
```

Then, to use the view:

```sql
SELECT * FROM UserMediaCount;
```

**View with Filtering**: If you need a list of users who are classified as 'Admin', assuming you have user levels set up in your UserLevels table, you can create a view that filters this data:

```sql
CREATE VIEW AdminUsers AS
  SELECT Users.user_id, Users.username, Users.email
  FROM Users
  JOIN UserLevels ON Users.user_level_id = UserLevels.level_id
  WHERE UserLevels.level_name = 'Admin';
```

Query the view:

```sql
SELECT * FROM AdminUsers;
```

**Updatable View**: [Some views are updatable](https://dev.mysql.com/doc/refman/8.2/en/view-updatability.html), meaning you can use them to perform UPDATE, INSERT, and DELETE operations. Whether a view is updatable depends on the SQL database system and the view's definition. For instance:

```sql
CREATE VIEW BasicUserInfo AS
  SELECT user_id, username, email
  FROM Users;
```

Assuming this view is updatable, you could perform an operation like:

```sql
UPDATE BasicUserInfo SET email = 'john.newemail@example.com' WHERE username = 'JohnDoe';
```

Notes:

- The view does not store data itself. When you query a view, you are querying the underlying tables.
- Views can be used to simplify complex joins and calculations, present a different representation of the data, and restrict access to certain rows or columns.
- The syntax for creating and using views might vary slightly depending on the specific SQL database you are using (like MySQL, PostgreSQL, SQL Server, etc.).

## Indexes

SQL indexes are used to improve the performance of queries by reducing the number of rows that need to be scanned. They are typically created on columns that are frequently used in WHERE clauses.

If you often query the `Users` table by `username`, an index on `username` would be beneficial:

```sql
CREATE INDEX idx_username ON Users(username);
```

This would speed up queries like:

```sql
SELECT * FROM Users WHERE username = 'JohnDoe';
```

Since foreign keys are often used in JOIN operations, indexing them can improve performance. For example, `user_id` in `MediaItems`:

```sql
CREATE INDEX idx_mediaitems_userid ON MediaItems(user_id);
```

This index would be useful for queries joining the Users and MediaItems tables:

```sql
SELECT Users.username, MediaItems.title 
FROM Users 
JOIN MediaItems ON Users.user_id = MediaItems.user_id;
```

Considerations for using indexes:

- Indexes take up storage space.
- Indexes need to be updated when the underlying data changes, which can slow down data modification operations like INSERT, UPDATE, and DELETE.
- The performance of the queries should always be measured before and after adding indexes to see if they are actually improving performance. If there is no significat improvement then it may be better to leave the extra indexes out.

## Temporal databases

A temporal database stores data relating to time instances. It offers temporal data types and stores information relating to past, present, and future time. Temporal databases are particularly useful for applications like booking systems, historical data analysis, and tracking changes over time.

Two main types of temporal data:

- **System-Versioned Temporal Data:** tracks when data is stored in the database. It's automatically generated and maintained by the database system.
- **Application-Time Temporal Data:** records the time period during which the data is considered valid in the real world, and is defined and managed by the application.

Reading: <https://www.dolthub.com/blog/2023-08-07-temporal-database/>

---

## Task

Think about the example database. Which of the introduced concepts could be applied to it? How would you implement them? Write down some ideas and examples.
