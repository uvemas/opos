# Tema 12 Oracle. SQl. PL/SQL

[Tutorial](https://www.oracletutorial.com/plsql-tutorial/what-is-plsql/)

[Apex workspace gratuito](https://apex.oracle.com)
## Datatypes

PL/SQL has two kinds of data types: scalar and composite.

PL/SQL divides the scalar data types into four families:

- Number
- Boolean
- Character
- Datetime

A scalar data type may have subtypes. A subtype is a data type that is a subset of another data type, which is its base type. 

Note that PL/SQL scalar data types include SQL data types and its own data type such as Boolean:

- Boolean: three valued, TRUE, FALSE, NULL
- PLS_INTEGER: uses hardware arithmetic. {NULL, natural numbers}
  - NATURAL: {NULL, enteros no negativos}
  - NATURALN: {enteros no negativos}
  - POSITIVE: {NULL, enteros positivos}
  - POSITIVEN: {enteros positivos}
  - SIGNTYPE: tree valued, {-1, 0, +1}
  - SIMPLE_INTEGER: {natural numbers}
- Character data types: CHAR, VARCHAR2, LONG, RAW, LONG RAW, ROWID, and UROWID
- Datetime data types: DATE, TIMESTAMP, TIMESTAMP WITH TIME ZONE, TIMESTAMP WITH LOCAL TIME ZONE, INTERVAL YEAR TO MONTH, 
  and INTERVAL DAY TO SECOND
