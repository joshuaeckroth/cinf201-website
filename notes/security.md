---
title: Security
layout: note
---

# Security

## SQL injection

"SQL injection" refers to the ability for an untrusted user to force your application to run unauthorized SQL code. For example, if a user can cause a "SELECT username, password FROM users" query to run, the user may be able to list all users and passwords (which should be encrypted properly; see below). An application should never allow a user to run arbitrary SQL code.

Naive coding can easily introduce SQL injection vulnerabilities. The problem lies in the fact that SQL code is usually written as strings in an application, e.g., a C++ or Java or PHP or … application will save a query in a string variable. If this query includes data from the user, the application may simply insert the user data directly into the string, like so:

```java&quot;
String username = ... // get username from user input
String query = "SELECT msgtxt, msgdate FROM messages WHERE username = \"" + username + "\" ORDER BY msgdate";
db.execute(query);
```

Since the username variable is inserted into the middle of the query, a clever user could type a specific portion of SQL code so that the resulting query is very different from what the application developer intended. E.g., the user could type:

```
" OR 1=1 UNION SELECT username as msgtxt, password as msgdate FROM users WHERE "1"="
```

After the strings are joined, the resulting query is:

```sql
SELECT msgtxt, msgdate FROM messages WHERE username = "" OR 1=1 UNION SELECT username as msgtxt, password as msgdate FROM users WHERE "1"="1" ORDER BY msgdate
```

…which is completely different and a severe security vulnerability.

![xkcd: Exploits of a Mom](http://imgs.xkcd.com/comics/exploits_of_a_mom.png)

### Avoiding SQL injections

SQL injections can be avoided in two ways:

- Rewrite user input so that all SQL operations are "escaped", e.g., so quotes look like `\"` rather than `"` and fields that should be numbers can only be numbers.
- Use "prepared statements", which are usually written as `SELECT … FROM … WHERE username = ?`, i.e., user input is indicated with a placeholder `?`. The user input is then inserted into the query with a different function, like `bind_params` or similar, which must know what kind of datatype is required for each `?`. Strings, numbers, and so on will only be allowed by `bind_params` if they have the right form.

The latter approach is usually more secure. Regardless of the approach used, your programming language and/or database library must support SQL escaping or prepared statements.

## Password storage

Suppose that somehow your users table is displayed, perhaps by a SQL injection or stolen access credentials. This is already a bad situation, but careful planning can ensure passwords are not exposed to the world.

Passwords should be stored hashed, obviously. Hashing means computing a number based on the contents of the password, and storing only that number, not the original password. Thus, the original password is not recoverable (at least, that's the intention). If they are not hashed, a SQL injection will expose them to the world. When a password is hashed, the only way to recover it is to iterate through possible passwords, hash each one, and compare the hash to the value stored in the database. When the hashes match, the password has been found.

Not all hashing techniques are equally good. A fast hashing technique, like MD5, is a bad choice for password storage, since the password can be "cracked" by trying  a lot of variations and hashing each of them. Very high hash rates, in the millions of hashes per second, are possible on commodity hardware. A 10-character password with upper and lowercase letters and numbers ranges over (10+52)^10 = 620 million combinations, so at most a few minutes will be needed to crack the password.

Thus, we want a slow hash function. Bcrypt is a common choice, since it is somewhat slower than MD5. Furthermore, we can hash the result of bcrypt, and hash the hash, etc. recursively, thousands of times, further slowing down the procedure. Now commodity hardware cannot achieve millions of hashes per second, say just thousands per second.

Finally, all 620 million password hashes can be generated ahead of time (it's not really that many), and given away on the internet. To prevent this, passwords can be "salted" with additional text before being hashed. The salt can be random or specific per application. Either way, the salt should be stored in plaintext (visible to anyone who does a SQL injection!), but that's not a security issue. Particularly if the salt is random for each password, nobody will be able to generate all combinations for every possible salt. Thus, pre-built hash tables cannot be generated if salts are used.