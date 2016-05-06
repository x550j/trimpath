# TrimQuery SQL Syntax #
{ [TrimQuery home](http://code.google.com/p/trimpath/wiki/TrimQuery) -- [API](http://code.google.com/p/trimpath/wiki/TrimQueryAPI) | [syntax](http://code.google.com/p/trimpath/wiki/TrimQuerySyntax) | [download](http://code.google.com/p/trimpath/downloads/list) | [community](http://code.google.com/p/trimpath/wiki/TrimQueryDiscussion) }

The queryLang.parseSQL() method supports a subset of the SQL SELECT syntax.

Here are the rules:
  * It's case sensitive,
    * For example, Customer.firstName is different than customer.firstname.
  * SQL keywords must be all uppercase.
    * Use SELECT, FROM, WHERE, not select, from, where.
  * Columns must be fully specified using TableNameOrAlias.columnName dotted syntax.
    * Use Invoice.custId, not just custId.
    * The exception is you may use `"SELECT * syntax"` on single table queries.
      * So `"SELECT Invoice.* FROM Invoice"` and `"SELECT * FROM Invoice"` are the same.
      * Column references in WHERE or JOIN clauses, though, must be fully specified.
        * For example, `SELECT * FROM Invoice WHERE Invoice.custId IS NOT NULL`
    * Of course, you can use aliases, too...
      * SELECT Inv.total AS total FROM Invoice AS Inv ORDER BY total
  * No expressions in the columns list.
    * Except for simple aggregate functions like COUNT(), SUM(), and AVG()
    * For example, the following won't work: SELECT Invoice.total / 1000.0 AS total FROM Invoice.
    * You'll have to do your complex math outside of TrimQuery, or use [TQL syntax](http://code.google.com/p/trimpath/wiki/TrimQueryTql).
  * No outer joins.
    * If you really want to do a left outer join, you'll have to use [TQL syntax](http://code.google.com/p/trimpath/wiki/TrimQueryTql).
  * No ON ot USING join support.
    * You'll instead have to do your joins in the WHERE clause.
  * Variable placeholders are specified with a ? character.

## TrimQuery SQL Examples ##
```
SELECT * FROM Invoice

SELECT Invoice.* FROM Invoice                    

SELECT Invoice.id, Invoice.total FROM Invoice

SELECT Invoice.id, Invoice.total, Customer.acctBalance FROM Invoice, Customer

SELECT Invoice.*, Customer.* FROM Invoice, Customer

SELECT i.* FROM Invoice AS i

SELECT i.id AS ID_NUM FROM Invoice AS i

SELECT Invoice.*, i2.* FROM Invoice, Invoice AS i2

SELECT Customer.id, Invoice.custId, Invoice.total, Customer.acctBalance 
              FROM Invoice, Customer 
              WHERE Invoice.custId = Customer.id

SELECT Customer.id, Invoice.custId, Invoice.total, Customer.acctBalance 
              FROM Invoice, Customer 
              WHERE Invoice.total < 250 AND Invoice.custId = Customer.id

SELECT Invoice.id FROM Invoice ORDER BY Invoice.id

SELECT Invoice.id FROM Invoice ORDER BY Invoice.id ASC

SELECT Invoice.id FROM Invoice ORDER BY Invoice.id DESC

SELECT Invoice.custId, Invoice.id FROM Invoice ORDER BY Invoice.custId DESC, Invoice.id

SELECT Refund.* FROM Refund LIMIT 100

SELECT Refund.* FROM Refund LIMIT 100, 50   // LIMIT 100 records, starting from offset of 50

SELECT Invoice.total, 
              SUM (Invoice.total) AS SUM_total,
              COUNT (Invoice.total) AS COUNT_total,
              AVG (Invoice.total) AS AVG_total 
              FROM Invoice

SELECT Invoice.total, 
              SUM (Invoice.total) AS SUM_total,
              COUNT (Invoice.total) AS COUNT_total,
              AVG (Invoice.total) AS AVG_total 
              FROM Invoice 
              GROUP BY Invoice.custId

SELECT Invoice.total, 
              SUM (Invoice.total) AS SUM_total
              FROM Invoice GROUP BY Invoice.custId ORDER BY SUM_total ASC

SELECT Invoice.total AS TOT, 
              SUM (Invoice.total) AS SUM_total
              FROM Invoice
              GROUP BY Invoice.custId 
              HAVING (TOT) > 15000
              ORDER BY SUM_total ASC

SELECT * FROM User WHERE User.name LIKE 'P%'

INSERT INTO Event (id, date, invoice_id) VALUES (1, '2003-01-31', 43)

UPDATE User SET name='Frank Thomas' WHERE User.id = '3'

DELETE Thing FROM Thing WHERE Thing.id = '1'
```

## Unsupported SQL Features ##

Features of SQL SELECT that are not (yet?) supported...
  * BETWEEN
  * IN (for lists and for sub-queries or nested SELECT's)
  * ANY
  * EXISTS
  * DISTINCT
  * AVG DISTINCT, SUM DISTINCT, COUNT DISTINCT
  * LEFT OUTER JOIN
  * RIGHT OUTER JOIN
  * FULL OUTER JOIN
  * UNION, EXCEPT, INTERSECT
  * Lowercase keywords like "select Invoice.**from Invoice"
  * Naked** usage, like "SELECT **FROM Invoice".
    * Instead, for now you must use "SELECT Invoice.** FROM Invoice".

[SteveYen: Please don't hesitate to jump right with your own implementations or contributions in if you wish.  There's nothing like implementing a small SQL-like engine to helping one's understanding and appreciation for SQL! ](.md)


---

{ [TrimQuery home](http://code.google.com/p/trimpath/wiki/TrimQuery) -- [API](http://code.google.com/p/trimpath/wiki/TrimQueryAPI) | [syntax](http://code.google.com/p/trimpath/wiki/TrimQuerySyntax) | [download](http://code.google.com/p/trimpath/downloads/list) | [community](http://code.google.com/p/trimpath/wiki/TrimQueryDiscussion) }