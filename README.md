# Database Basics 

## Objectives

1. Describe how relational databases store data in tables composed of columns and rows.
2. Use lower case and snake_case conventions for column names.
3. Use the `CREATE TABLE` command to create a new table and include the `id INTEGER PRIMARY KEY`.
4. Identify the different types of data you can store in a database.
5. Use the `.help` command to get a complete list of SQL commands.
6. User the `.tables` command to list all the tables in a database.
7. Use the `.schema` command to look at the structure of a database.
8. Use the `ALTER TABLE` command to add columns to a table.
9. Use the `DROP TABLE` command to delete a table.
10. Write SQL in a text editor. 

## Database Structure

Relational Databases like SQLite store data in a structure we refer to as a table. You can think of a table in a database a lot like you would a spreadsheet. We define specific columns in our table, and then we store any number of what we refer to as 'records' as rows in our database. A record is just information referring to one specific entity. For instance, if you had a table called "People" you could imagine a structure like this:

| name | age | email |
|------|-----|-------|
| Bob | 29 |  bob@flatironschool.com |
| Avi  |  28   | avi@flatironschool.com  |
| Adam |  28   | adam@flatironschool.com |

Each column has a name, and each row contains the corresponding information about a person.

### Note on Column Names

When we name columns in our database, there are a couple of conventions we will follow. The first is that we will always use lowercase letters when referring to columns in our database. SQLite isn't case sensitive about its commands or column names, as we will discuss below, but it is general best practice for us to stick to lowercase for our column names.

The second convention we want to follow is more important. That is that when we have multiple words in a column name we link the together use underscores rather than spaces. We call this convention "snake_case". So, for instance, if we wanted to be more specific with our email column above, we would have called it something like email_address. If we wanted to split up name to first and last we might have columns called first_name and last_name.

## Database Tables

In the following sections, we'll cover how to create, alter and delete database tables. This reading is accompanied by a code along exercise. There are no tests to pass, but fork and clone this repository by clicking on the Github link on the top of the page. Follow along with the reading and code along instructions to learn some SQL. 

### Create Table

When we create a new database, it comes like a sort of blank slate. We need to define the tables where we store the records, the columns that hold the data for our records, and the specific type of data we are going to store in each column before we are able to store any actual data. 

Once you create a database (which you can do with the `sqlite3 database_name.db` command), we create a table with the following line of code: 

```sql
CREATE TABLE table_name;
```

Let's give it a shot. For the purposes of this code along, we'll be creating a manipulating a database full of (you guessed it) cats!

### Code Along I: Creating a Table

* Open up your terminal and create our new database:

```sql
sqlite3 pet_database.db
```

* Now, let's create our table:

```bash
sqlite> CREATE TABLE cats;
``` 

You should see the following error:

```sql
sqlite> CREATE TABLE cats;
Error: near ";": syntax error
```

SQLite expects us to include at least some definition of the structure of this table as well. In other words, when we create database tables, we need to specify some column names, along with the type of data we are planning to store in each column. More on data types later. 

Let's try that table definition again:

```sql
sqlite> CREATE TABLE cats (
        id INTEGER PRIMARY KEY,
				name TEXT, 
				age INTEGER
			);
```
Let's break down the above code: 

1. use that `CREATE TABLE` command to create a new table called "cats"
2. Pass that command a list of column names along with the type of data they will be storing. `TEXT` means we'll be storing plain old text, `INTEGER` means we'll store a number. Note that the use of capitalization is arbitrary, but it is a convention to help separate the SQL commands from the names we make up for our tables and columns. 

**A note on the "id" column:** Our SQLite database tables *must be indexed by a number*. We want each row in our table to have a number, which we'll call "id", just like in an excel spreadsheet. Numbering our table rows makes our data that much easier to access, update, and organize. SQLite comes with a data type designation called "Primary Key". Primary keys are unique and auto-incrementing, meaning they start at 1 and each new row automatically gets assigned the next numeric value. Every table we create, regardless of the other column names and data types, should be defined with an `id INTEGER PRIMARY KEY` column + data type. 

Okay, let's check and make sure that we successfully created that table. To do this we'll be using SQL commands. To get a complete lis of commands, you can type `.help` into the sqlite prompt. 

```bash
sqlite> .help
.backup ?DB? FILE      Backup DB (default "main") to FILE
.bail ON|OFF           Stop after hitting an error.  Default OFF
.databases             List names and files of attached databases
.dump ?TABLE? ...      Dump the database in an SQL text format
                         If TABLE specified, only dump tables matching
                         LIKE pattern TABLE.
.echo ON|OFF           Turn command echo on or off
.exit                  Exit this program
.explain ?ON|OFF?      Turn output mode suitable for EXPLAIN on or off.
                         With no args, it turns EXPLAIN on.
.header(s) ON|OFF      Turn display of headers on or off
.help                  Show this message
.import FILE TABLE     Import data from FILE into TABLE
.indices ?TABLE?       Show names of all indices
                         If TABLE specified, only show indices for tables
                         matching LIKE pattern TABLE.
.log FILE|off          Turn logging on or off.  FILE can be stderr/stdout
.mode MODE ?TABLE?     Set output mode where MODE is one of:
                         csv      Comma-separated values
                         column   Left-aligned columns.  (See .width)
                         html     HTML <table> code
                         insert   SQL insert statements for TABLE
                         line     One value per line
                         list     Values delimited by .separator TEXT
                         tabs     Tab-separated values
                         tcl      TCL list elements
.nullvalue TEXT      Print TEXT in place of NULL values
.output FILENAME       Send output to FILENAME
.output stdout         Send output to the screen
.prompt MAIN CONTINUE  Replace the standard prompts
.quit                  Exit this program
.read FILENAME         Execute SQL in FILENAME
.restore ?DB? FILE     Restore content of DB (default "main") from FILE
.schema ?TABLE?        Show the CREATE statements
                         If TABLE specified, only show tables matching
                         LIKE pattern TABLE.
.separator TEXT      Change separator used by output mode and .import
.show                  Show the current values for various settings
.stats ON|OFF          Turn stats on or off
.tables ?TABLE?        List names of tables
                         If TABLE specified, only list tables matching
                         LIKE pattern TABLE.
.timeout MS            Try opening locked tables for MS milliseconds
.vfsname ?AUX?         Print the name of the VFS stack
.width NUM1 NUM2 ...   Set column widths for "column" mode
.timer ON|OFF          Turn the CPU timer measurement on or off
```

Woah, that's a lot. Don't worry too much about all of these different commands right now. Just know that you can always use `.help` to check out the available options. 

Okay, let's check out our new table. To list all the tables in the database we'll use the .tables command. Type it into the sqlite prompt and hit enter, you should get:

```sql
sqlite> .tables
cats
```

We can look at the structure, or "schema", of our database (i.e. the tables and their columns + column data types) with the `.schema` command:

```sql
sqlite> .schema
CREATE TABLE cats(
  id INTEGER PRIMARY KEY,
  name TEXT,
  age INTEGER,
);
```

Let's move on to altering our table. 

### Alter Table

Let's say that, after creating a database and creating a table to live inside that database, we decide we want to add or remove a column. We can do so with the `ALTER TABLE` command. 

### Code Along II: Adding, Removing and Renaming Columns

* Let's say we want to add a new column, `breed`, to our `cats` table. 

```sql
sqlite> ALTER TABLE cats ADD COLUMN breed TEXT;
```

Let's check out our schema now: 

```sql
sqlite> .schema
CREATE TABLE cats(
  id INTEGER PRIMARY KEY,
  name TEXT,
  age INTEGER,
  breed TEXT
);
```

Notice that the `ALTER` statement isn't here, but instead SQLite has updated our original CREATE statement. The schema reflects the current structure of the database, reflected as the commands necessary to create that structure.

* Unfortunately, altering a column name and/or deleting a column can be tricky in SQLite3. There are workarounds, however. We're not going to get into that right now, but you can explore the documentation on this topic [here](https://www.sqlite.org/lang_altertable.html).

Actually, there are a handful of things other SQL systems like Postgres, or MySQL can do that are not supported by SQLite. The decision to forego support for certain SQL features is not arbitrary, and a list of those features is here. Depending on how you look at it, this is a strength or a weakness for SQLite, but you shouldn't look at it in terms of some all-informing absolute truth. Every database system has its own strengths and weaknesses, and as you learn more about them you should evaluate them thoughtfully when deciding which to use for what purpose. For us, SQLite provides a low barrier to entry, and is simple to get up and running.

Fortunately, SQLite still supports most of what we'll need to use it for one way or another. For now, if you need to change a column name, it's best to simply delete the table and re-create it. 

### Drop Table

Lastly, we'll discuss how to delete a table from a database with the `DROP TABLE` command. 

### Code Along III: Deleting a Table 

Deleting a table is very simple: 

```sql
sqlite> DROP TABLE cats;
```

And that's it! You can exit out of the sqlite prompt with the `.quit` command. 

### Advanced: Writing SQL in a Text Editor

In this code along, we've been executing our SQL commands directly in the terminal. It is likely, however, that you will find yourself writing SQL in a file and executing that file in the context of your database. Here's how it works: 

1. In the terminal, create a database just like we did earlier: `sqlite3 pets_database.db`. 
2. Open up a text editor (such as Sublime Text) and create and save a file `01_create_cats_table.sql`. In this file, write your create statement: 

```sql
CREATE TABLE cats (
  id INTEGER PRIMARY KEY,
	name TEXT, 
	age INTEGER
);
```
3 . Execute that file in the command line: 

```sql
sqlite3 pets_database.db < 01_create_cats_table.sql
```

To carry out an subsequent action on this database––adding a column to the `cats` table, dropping that table, creating a new table––we would create subsequent `.sql` files in the text editor and execute them in the same way as above. For example, to add a column to our `cats` table:

1. Create a file `02_add_column_to_cats.sql` and fill it out with: 

```sql
ALTER TABLE cats ADD COLUMN breed TEXT;
```
Then, execute the file: 

```sql
sqlite3 pets_database.db < 02_add_column_to_cats.sql
```
