---
title: "Psql Common Issues"
date: 2024-05-29T01:16:51+05:30
tags: ["postgres", "unix"]
description: "Today I learned some psql common issues"
---

Whenever I install there are a few issues that are common but I wasn't aware why they occured. Today I also had some issues which now I understand, so I am writing these notes for future reference. Hopefully I don't require these in future.

# psql
Beginning with the command psql, let's understand what it is and what it does.
It is one of the utitlity that is installed when you install postgres along with other programs such createdb, dropdb, createuser, and more.

```
$ psql
psql: error: FATAL:  database "karan.y" does not exist
```

When you run this command you get dropped into a psql shell which connects to a database having the same name as current user by default. You can specifiy the database as follows -

`psql <database>`

You can list all the databases as follows - 
```
$ psql -l                          
                                  List of databases
       Name        |  Owner   | Encoding | Collate | Ctype |    Access privileges    
-------------------+----------+----------+---------+-------+-------------------------
 postgres          | karan.y  | UTF8     | C       | C     | 
 template0         | karan.y  | UTF8     | C       | C     | =c/"karan.y"           +
                   |          |          |         |       | "karan.y"=CTc/"karan.y"
 template1         | karan.y  | UTF8     | C       | C     | =c/"karan.y"           +
                   |          |          |         |       | "karan.y"=CTc/"karan.y"
(5 rows)
```

You can drop into the postgres database with the current user and create a new role `postgres`. It can also be created the createuser postgres utitlity. Then you can drop into psql shell with postgres role
```
$ createuser postgres
$ psql -U postgres my_database
```



# server error
Another error that you might receive is this - 

```
$ psql                              
psql: error: could not connect to server: No such file or directory
        Is the server running locally and accepting
        connections on Unix domain socket "/tmp/.s.PGSQL.5432"?
```

Let's understand what this error means -

```
In Unix and Unix-like operating systems, a Unix domain socket (also known as an IPC socket) is a type of socket that is used for inter-process communication (IPC). A Unix domain socket is similar to a network socket, but it is used to communicate between processes on the same system, rather than between processes on different systems.

The PostgreSQL database server uses the Unix domain socket to listen for incoming connections from PostgreSQL client applications. A PostgreSQL client application is a process that runs on a Unix or Unix-like operating system and is responsible for interacting with the PostgreSQL database.

When a PostgreSQL client application wants to connect to the PostgreSQL database server, it can use the Unix domain socket to establish a connection. The PostgreSQL database server will then accept the connection and allow the PostgreSQL client application to interact with the PostgreSQL database.
```

This will probably be fixed by starting the postgres service -

`brew services start postgresql@14`

