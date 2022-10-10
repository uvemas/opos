# Tema 11 BBDD relacionales. Fundamentos. Normalización

Dr Edgar F. Codd, after his extensive research on the Relational Model of database systems, came up with twelve rules of his own, which 
according to him, a database must obey in order to be regarded as a true relational database.

These rules can be applied on any database system that manages stored data using only its relational capabilities. This is a foundation 
rule, which acts as a base for all the other rules.

**Rule 1: Information Rule**
The data stored in a database, may it be user data or metadata, must be a value of some table cell. Everything in a database must be 
stored in a table format.

**Rule 2: Guaranteed Access Rule**
Every single data element (value) is guaranteed to be accessible *logically* with a combination of table-name, primary-key (row value), 
and attribute-name (column value). No other means, such as pointers, can be used to access data.

**Rule 3: Systematic Treatment of NULL Values**
The NULL values in a database must be given a systematic and uniform treatment. This is a very important rule because a NULL can be 
interpreted as one the following - data is missing, data is not known, or data is not applicable. O sea, hay que decidir como se trata
el valor NULL (elegir una de las tres opciones). Una vez decidido, *todos* los NULL de la BBDD se tratarán de esa manera.

**Rule 4: Active Online Catalog**
The structure description of the entire database must be stored in an online catalog, known as **data dictionary**, which can be 
accessed by authorized users. Users can use the same query language to access the catalog which they use to access the database itself.

**Rule 5: Comprehensive Data Sub-Language Rule**
A database can only be accessed using a language having linear syntax that supports data definition, data manipulation, and transaction
management operations. This language can be used directly or by means of some application. If the database allows access to data 
without any help of this language, then it is considered as a violation.

Rule 6: View Updating Rule
All the views of a database, which can theoretically be updated, must also be updatable by the system.

Rule 7: High-Level Insert, Update, and Delete Rule
A database must support high-level insertion, updation, and deletion. This must not be limited to a single row, that is, it must also 
support union, intersection and minus operations to yield sets of data records.

Rule 8: Physical Data Independence
The data stored in a database must be independent of the applications that access the database. Any change in the physical structure of 
a database must not have any impact on how the data is being accessed by external applications.

Rule 9: Logical Data Independence
The logical data in a database must be independent of its user's view (application). Any change in logical data must not affect the 
applications using it. For example, if two tables are merged or one is split into two different tables, there should be no impact or 
change on the user application. This is one of the most difficult rule to apply.

Rule 10: Integrity Independence
A database must be independent of the application that uses it. All its integrity constraints can be independently modified without the 
need of any change in the application. This rule makes a database independent of the front-end application and its interface.

Rule 11: Distribution Independence
The end-user must not be able to see that the data is distributed over various locations. Users should always get the impression that 
the data is located at one site only. This rule has been regarded as the foundation of distributed database systems.

Rule 12: Non-Subversion Rule
If a system has an interface that provides access to low-level records, then the interface must not be able to subvert the system and 
bypass security and integrity constraints.

Regarding rule 5, the SQL statements are divided into three categories:

- Data manipulation language (DML): target *data* stored in the DDBB.SELECT, INSERT, UPDATE, DELETE, BEGIN, COMMIT, ROLLBACK
- Data definition language (DDL): create, modify and destroy *DDBB objects* like schemas, tables, indexes and views. CREATE, ALTER, DROP
- Data control language (DCL): authorize users to access data and DDBB objects. GRANT, REVOKE
## Normalization

[Referencia](https://www.studytonight.com/dbms/database-normalization.php)

Normalization is the process of organizing the data in the database. It consists of a series of guidelines that helps to guide you in 
creating a good database structure.

Normalization divides the larger tables into smaller ones and links them using relationships. The normal form is used to eliminate 
undesirable characteristics like Insertion, Update, and Deletion Anomalies. Failure to eliminate anomalies leads to data redundancy 
and can cause data integrity and other problems as the database grows.

Data modification anomalies can be categorized into three types:

- Insertion Anomaly: Insertion Anomaly refers to when one cannot insert a new tuple into a relationship due to lack of data.
- Deletion Anomaly: The delete anomaly refers to the situation where the deletion of data results in the unintended loss of some other 
  important data.
- Updatation Anomaly: The update anomaly is when an update of a single data value requires multiple rows of data to be updated.

Normalization works through a series of stages called Normal forms. The normal forms apply to individual tables. The table is 
said to be in particular normal form if it satisfies constraints.

**First normal form, 1NF**

A table is in 1NF if it contains only atomic (from the user point of view) values. To achieve this goal we must eliminate repeating 
groups (groups with the same meaning, for instance address1 and address2).

Another way to look at the repeating groups problem is with a one-to-many relationship: do not put the one side and the many side in 
the same table.

**Second normal form, 2NF**

A table will be in 2NF if it is in 1NF and all non-key values are fully functional dependent on the primary key [^1]. It means that
all non-key values can be fetched using the full primary key (remember that primary key can be a composite key). 

If the dependency is only partial the 2NF is not fulfiled. It means that there are values that can be fetched using only part of a 
composite primary key. For instance, if we have a table with fields A, B, C, D where A+B is a composite primary key, but values of D
only need B to be fetched then D depends partially of the composite primary key A+B and the table is *not in 2FN*.

**Third normal form, 3NF**

A table will be in 3NF if it is in 2NF and no transition dependency exists. It means that all non-key values on the table deppend on 
**only** primary keys.


[^1]: But may depend also on other fields that are not primary keys.
