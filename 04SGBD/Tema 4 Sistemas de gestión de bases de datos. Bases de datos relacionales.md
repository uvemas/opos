# Tema 04 Sistemas de Gestión de Bases de Datos

Connolly and Begg define database management system (DBMS) as a "software system that enables users to define, create, maintain and 
control access to the database".[24] Examples of DBMS's include MySQL, PostgreSQL, Microsoft SQL Server, Oracle Database, and 
Microsoft Access.

The DBMS acronym is sometimes extended to indicate the underlying database model, with RDBMS for the relational, OODBMS for the object 
(oriented) and ORDBMS for the object-relational model. Other extensions can indicate some other characteristics, such as DDBMS for a 
distributed database management systems.

The functionality provided by a DBMS can vary enormously. The core functionality is the storage, retrieval and update of data. Codd 
proposed the following functions and services a fully-fledged general purpose DBMS should provide:[25]

- Data storage, retrieval and update
- User accessible catalog or data dictionary describing the metadata
- Support for transactions and concurrency
- Facilities for recovering the database should it become damaged
- Support for authorization of access and update of data
- Access support from remote locations
- Enforcing constraints to ensure data in the database abides by certain rules

It is also generally to be expected the DBMS will provide a set of utilities for such purposes as may be necessary to administer the 
database effectively, including import, export, monitoring, defragmentation and analysis utilities.[26] The core part of the DBMS 
interacting between the database and the application interface sometimes referred to as the database engine.

Often DBMSs will have configuration parameters that can be statically and dynamically tuned, for example the maximum amount of main 
memory on a server the database can use. The trend is to minimize the amount of manual configuration, and for cases such as embedded 
databases the need to target zero-administration is paramount.

The large major enterprise DBMSs have tended to increase in size and functionality and can have involved thousands of human years of 
development effort throughout their lifetime.[a]

Early multi-user DBMS typically only allowed for the application to reside on the same computer with access via terminals or terminal 
emulation software. The client-server architecture was a development where the application resided on a client desktop and the database 
on a server allowing the processing to be distributed. This evolved into a multitier architecture incorporating application servers and 
web servers with the end user interface via a web browser with the database only directly connected to the adjacent tier.[27]

A general-purpose DBMS will provide public application programming interfaces (API) and optionally a processor for database languages 
such as SQL to allow applications to be written to interact with and manipulate the database. A special purpose DBMS may use a private 
API and be specifically customized and linked to a single application. For example, an email system performs many of the functions of a 
general-purpose DBMS such as message insertion, message deletion, attachment handling, blocklist lookup, associating messages an email 
address and so forth however these functions are limited to what is required to handle email.
## APIs

External interaction with the database will be via an application program that interfaces with the DBMS.[28] This can range from a 
database tool that allows users to execute SQL queries textually or graphically, to a website that happens to use a database to store 
and search information.

A programmer will code interactions to the database (sometimes referred to as a datasource) via an application program interface (API) 
or via a database language. The particular API or language chosen will need to be supported by DBMS, possibly indirectly via a 
preprocessor or a bridging API. Some API's aim to be database independent, ODBC being a commonly known example. Other common API's 
include JDBC and ADO.NET.

All those APIs are intended for accessing (i.e. connecting) to DBMSs.
### ODBC

ODBC is aimed to be independent of the DBMS and OS. An application written using ODBC can be ported to other platforms, both on the 
client and server side, with few changes to the data access code.
 
ODBC accomplishes DBMS independence by using an ODBC driver as a translation layer between the application and the DBMS. The 
application uses ODBC functions through an ODBC driver manager with which it is linked, and the driver passes the query to the DBMS. An 
ODBC driver can be thought of as analogous to a printer driver or other driver, providing a standard set of functions for the 
application to use, and implementing DBMS-specific functionality. An application that can use ODBC is referred to as "ODBC-compliant". 
Any ODBC-compliant application can access any DBMS for which a driver is installed. Drivers exist for all major DBMSs, many other data 
sources like address book systems and Microsoft Excel, and even for text or comma-separated values (CSV) files.

Ejemlo: la aplicación DBeaver utiliza un driver ODBC para conectarse al DBMS MariaDB.
### JDBC

[JDBC](https://en.wikipedia.org/wiki/Java_Database_Connectivity)

Java Database Connectivity (JDBC) is an application programming interface (API) for the programming language Java, which defines how a 
client may access a database. It is a Java-based data access technology used for Java database connectivity. It is part of the 
Java Standard Edition platform, from Oracle Corporation. [In addition to connectivity to the DBMS] It provides methods to query and 
update data in a database [^1], and is oriented toward relational databases. A JDBC-to-ODBC bridge enables connections to any 
ODBC-accessible data source in the Java virtual machine (JVM) host environment.

[^1]: Lo que significa que un programa Java que use JDBC puede no solo conectarse a un SGBD sino atacar a la base de datos haciendo 
queries y otras operaciones con la sintaxis proporcionada por JDBC.
## Languages

Database languages are *special-purpose languages*, which allow one or more of the following tasks, sometimes distinguished as 
sublanguages:

- Data control language (DCL): controls access to data;
- Data definition language (DDL): defines data types such as creating, altering, or dropping tables and the relationships among them;
- Data manipulation language (DML): performs tasks such as inserting, updating, or deleting data occurrences;
- Data query language (DQL): allows searching for information and computing derived information.

Database languages are specific to a particular data model. Notable examples include:

- SQL combines the roles of data definition, data manipulation, and query in a single language
- OQL
- XQL
