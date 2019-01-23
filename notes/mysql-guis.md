---
title: MySQL GUIs
layout: note
---

# MySQL GUIs

There are several good options for using a graphical user interface (GUI) to connect to MySQL rather than the text-based command line. Whether you use a GUI or not is your preference, but it is important that you know they are available. These notes will show you how to set up MySQL Workbench for access to the londo MySQL database.

## MySQL Workbench

First, [download MySQL Workbench](https://www.mysql.com/products/workbench/).


Upon starting it the first time, you'll need to create a new connection by clicking the + icon:

![MySQL Workbench](/images/mysql-workbench-1.png)

Type in the connection details exactly as shown, except change the username 'jeckroth' to your own username:

![MySQL Workbench](/images/mysql-workbench-2.png)

After clicking OK, you'll have a connection (like a bookmark) that can be used whenever you open the Workbench. Double click the connection to connect. You'll need to provide your londo password followed by your database password (which you can find in your .my.cnf file on londo).

![MySQL Workbench](/images/mysql-workbench-3.png)

Assuming the connection was set up properly, you will now see several panes. On the lower-left is a list of databses which, when expanded, will show tables in each database. In the middle you can type and execute SQL queries. On the right is a help display or "snippets" (saved bits of SQL code).

![MySQL Workbench](/images/mysql-workbench-4.png)

Double-click a database to activate it in the left pane. Type a SQL query in the middle pane, and then click the lightning-bolt icon to execute it. The results will appear in the bottom pane.

![MySQL Workbench](/images/mysql-workbench-5.png)

