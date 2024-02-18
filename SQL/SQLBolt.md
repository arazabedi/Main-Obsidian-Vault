```table-of-contents
style: nestedList # TOC style (nestedList|inlineFirstLevel)
maxLevel: 0 # Include headings up to the speficied level
includeLinks: true # Make headings clickable
debugInConsole: false # Print debug info in Obsidian console
```
# 0. Cheat Sheet 

SQL (Structured Query Language) is a language used to query, manipulate, and transform data in relational databases.

Examples of SQL databases include: SQLite, MySQL, Postgres, Oracle and Microsoft SQL Server.



# 1. Relational Databases

A relational database is a collection of tables. Each table has a pre-determined number of columns, each representing an attribute or property of the table.

For example, the Vehicles table of a particular database may contain rows for id, Make/Model/, # of wheels, # of doors, and type of car (e.g. sedan, SUV etc.).

|   |   |   |   |   |
|---|---|---|---|---|
|id|Make/Model|# Wheels|# Doors|Type|
|1|Ford Focus|4|4|Sedan|
|2|Tesla Roadster|4|2|Sports|
|3|Kawakasi Ninja|2|0|Motorcycle|
|4|McLaren Formula 1|4|0|Race|
|5|Tesla S|4|4|Sedan|

SQL is used to answer questions such as: 
-  _"What types of vehicles are on the road have less than four wheels?"_
- "How many models of cars does Tesla produce?"

# 2. Retrieve (SELECT)

To retrieve data, write `SELECT` state