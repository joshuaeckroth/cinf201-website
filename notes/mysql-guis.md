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

After clicking OK, you'll have a connection (like a bookmark) that can be used whenever you open the Workbench. Double click the connection to connect.

![MySQL Workbench](/images/mysql-workbench-3.png)

Assuming the connection was set up properly, you will now see several panes. On the lower-left is a list of databses which, when expanded, will show tables in each database. In the middle you can type and execute SQL queries. On the right is a help display or "snippets" (saved bits of SQL code).

![MySQL Workbench](/images/mysql-workbench-4.png)

Expand a database in the lower-left pane and select a table. Right-click and choose to select the first 1000 rows. The middle pane will update to show the SQL query generated and executed, plus the results in a spreadsheet form.

![MySQL Workbench](/images/mysql-workbench-5.png)


## phpMyAdmin

A second option is phpMyAdmin, which works in your browser. Thus, you do not need to install any software.

Connect to phpMyAdmin using this address: [https://londo.stetson.edu/phpmyadmin/](https://londo.stetson.edu/phpmyadmin/). Log in with your MySQL credentials (not londo credentials).

![phpMyAdmin Login](/images/phpmyadmin-login.png)

Next, select your database and table from the tree on the left sidebar:

![phpMyAdmin Select Database](/images/phpmyadmin-selectdb.png)

After selecting a table, you can execute SQL queries using the SQL menu link, then typing some SQL code and clicking Go. Modify your query with the "Edit inline" link below the input box.

![phpMyAdmin Query](/images/phpmyadmin-query.png)

