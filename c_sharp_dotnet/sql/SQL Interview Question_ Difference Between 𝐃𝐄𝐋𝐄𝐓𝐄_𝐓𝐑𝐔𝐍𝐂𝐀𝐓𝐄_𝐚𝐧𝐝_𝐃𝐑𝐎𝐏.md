💡 SQL Interview Question: Difference Between 𝐃𝐄𝐋𝐄𝐓𝐄, 𝐓𝐑𝐔𝐍𝐂𝐀𝐓𝐄, 𝐚𝐧𝐝 𝐃𝐑𝐎𝐏

1️⃣ 𝐃𝐄𝐋𝐄𝐓𝐄
 - Deletes rows one by one.
 - Doesn’t delete the table structure.
 - Logs each deleted row, which allows the operation to be rolled back if used within a transaction.
 - Best used for Deleting specific rows with conditions (e.g., DELETE FROM table WHERE condition).

2️⃣ 𝐓𝐑𝐔𝐍𝐂𝐀𝐓𝐄
 - Deletes rows by de-allocating the data pages storing the data.
 - Retains the table structure
 - rollback depends on the database system , ex:
 - SQL Server: Can rollback within an explicit transaction.
 - MySQL: Cannot rollback.
 - Best used for Quickly removing all rows from a large table without logging each deletion.

3️⃣ 𝐃𝐑𝐎𝐏
 - Deletes the entire table along with its structure and dependencies.
 - Cannot be rolled back.
 - Best used for Permanently removing a table and freeing up storage space.

   ![image](https://github.com/user-attachments/assets/f84262f3-0f83-4a0a-878a-1b9161acc7f1)
SQL Interview Question_ Difference Between 𝐃𝐄𝐋𝐄𝐓𝐄_𝐓𝐑𝐔𝐍𝐂𝐀𝐓𝐄_𝐚𝐧𝐝_𝐃𝐑𝐎𝐏.md
