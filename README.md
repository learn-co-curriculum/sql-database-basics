# Database Basics 

## Overview

We'll cover how to create and delete database tables in SQLite as well as how to add columns to an existing table.  

## Objectives

1. Describe how relational databases store data in tables composed of columns and rows
2. Use lower case and snake_case conventions for column names
3. Use the `CREATE TABLE` keyword to create a new table with columns, including the `id` column
4. Use the `.help` command to get a complete list of SQL commands
5. Use the `.tables` command to list all the tables in a database
6. Use the `.schema` command to look at the structure of a database
7. Use the `ALTER TABLE` keywords to add columns to a table
8. Use the `DROP TABLE` keywords to delete a table

## Database Structure

Relational Databases like SQLite store data in a structure we refer to as a table. You can think of a table in a database a lot like you would a spreadsheet. We define specific columns in our table, and then we store any number of what we refer to as 'records' as rows in our database. A record is just information referring to one specific entity. For instance, if you had a table called "People" you could imagine a structure like this:

<table border="1" cellpadding="4" cellspacing="0">
  <tr>
    <th>name</th>
    <th>age</th>
    <th>email</th>
  </tr>
  
  <tr>
    <td>Bob</td>
    <td>29</td>
    <td>bob@flatironschool.com</td>
  </tr>
  <tr>
    <td>Avi</td>
    <td>28</td>
    <td>avi@flatironschool.com</td>
  </tr>
  <tr>
    <td>Adam</td>
    <td>28</td>
    <td>adam@flatironschool.com</td>
  </tr>
</table>

Each column has a name, and each row contains the corresponding information about a person.

### Note on Column Names

When we name columns in our database, there are a couple of conventions we will follow. The first is that we will always use lowercase letters when referring to columns in our database. SQLite isn't case sensitive about its commands or column names, but it is generally best practice for us to stick to lowercase for our column names.

The second convention we want to follow is more important. That is, when we have multiple words in a column name, we link them together using underscores rather than spaces. We call this convention "snake_case". So, for instance, if we wanted to be more specific with our email column above, we can name it email\_address. If we wanted to split up name to first and last we might have columns called first\_name and last\_name.

## Database Tables

In the following sections, we'll cover how to create, alter, and delete database tables. This reading is accompanied by a code along exercise that you can do in your terminal. You don't need to fork this repository, and there are no tests to pass. Follow along with the reading and code along instructions. 

### Create Table

When we create a new database, it comes like a sort of blank slate.

Once you create a database (which you can do with the `sqlite3 database_name.db` command), we create a table using the following statement: 

```sql
CREATE TABLE table_name;
```

But before we're able to store any actual data in a table, we'll need to define the columns in the table as well as the specific type of data each column will store. 

Let's give it a shot. For the purposes of this code along, you'll be typing these commands into your terminal. 

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

Let's try that table statement again:

```sql
sqlite> CREATE TABLE cats (
  id INTEGER PRIMARY KEY,
  name TEXT, 
  age INTEGER
);
```
Let's break down the above code: 

1. Use the `CREATE TABLE` command to create a new table called "cats"
2. Include a list of column names along with the type of data they will be storing. `TEXT` means we'll be storing plain old text, `INTEGER` means we'll store a number. Note that the use of capitalization is arbitrary, but it is a convention to help separate the SQL commands from the names we make up for our tables and columns. 
3. Every table we create, regardless of the other column names and data types, should be defined with an id INTEGER PRIMARY KEY column, including data type and primary key designation. Our SQLite database tables *must be indexed by a number*. We want each row in our table to have a number, which we'll call "id", just like in an Excel spreadsheet. Numbering our table rows makes our data that much easier to access, update, and organize. SQLite comes with a data type designation called "Primary Key". Primary keys are unique and auto-incrementing, meaning they start at 1 and each new row automatically gets assigned the next numeric value. 

Okay, let's check and make sure that we successfully created that table. To do this we'll be using SQL commands. To get a complete list of commands, you can type `.help` into the sqlite prompt. 

```
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
  age INTEGER
);
```

Let's move on to altering our table. 

### Alter Table

Let's say that, after creating a database and creating a table to live inside that database, we decide we want to add or remove a column. We can do so with the `ALTER TABLE` statement. 

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

Notice that the `ALTER` statement isn't here, but instead SQLite has updated our original CREATE statement. The schema reflects the current structure of the database, which is reflected as the CREATE statement necessary to create that structure.

* Unfortunately, altering a column name and/or deleting a column can be tricky in SQLite3. There are workarounds, however. We're not going to get into that right now, but you can explore the documentation on this topic [here](https://www.sqlite.org/lang_altertable.html).

Fortunately, SQLite still supports most of what we'll need to use it for one way or another. For now, if you need to change a column name, it's best to simply delete the table and re-create it. 

### Drop Table

Lastly, we'll discuss how to delete a table from a database with the `DROP TABLE` statement.

### Code Along III: Deleting a Table 

Deleting a table is very simple: 

```sql
sqlite> DROP TABLE cats;
```

And that's it! You can exit out of the sqlite prompt with the `.quit` command. 

<a href='https://www.sqlite.org/lang_keywords.html'>SQL KEYWORDS</a>
