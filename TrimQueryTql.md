# TrimQuery Language (TQL) #

{ [TrimQuery home](http://code.google.com/p/trimpath/wiki/TrimQuery) -- [API](http://code.google.com/p/trimpath/wiki/TrimQueryAPI) | [syntax](http://code.google.com/p/trimpath/wiki/TrimQuerySyntax) | [download](http://code.google.com/p/trimpath/downloads/list) | [community](http://code.google.com/p/trimpath/wiki/TrimQueryDiscussion) }

If you want to experiment and get under the hood of TrimQuery, then let's take a closer look at the TrimQuery Language (TQL).  TQL is accessed through the queryLang Object that is returned by ''TrimPath.makeQueryLang()''.

Underneath the hood, the queryLang.parseSQL() method converts SQL into TQL in order to do its job, and there may be interesting features of TQL that are not exposed to through queryLang.parseSQL().

Warning: unlike SQL, the TQL syntax and mechanisms might change, so it's not really wise to have your projects depend on straight TQL.

To experiment with straight TQL instead of SQL, just use the queryLang Object as the parameter for a ''with (queryLang) {...}'' block in JavaScript.  Various SQL keywords are available as JavaScript functions in TQL.  For example...
```
  <script language="javascript">
    // First we precompile the query language with the schema...
    var myQueryLang = TrimPath.makeQueryLang(columnDefs);

    var selectStatement1, selectStatement2;

    with (myQueryLang) {
        selectStatement1 = SELECT (Customer.id, Customer.acctBalance, FROM (Customer));
        selectStatement2 = SELECT (Customer.id, Customer.acctBalance, Invoice.total,
                                   FROM (Customer, Invoice));
    }
  </script>
```

As you can see, using the same queryLang Object, you can define multiple queries using invocations of the SELECT() method.

The queryLang.SELECT() method returns a selectStatement Object, just like the queryLang.parseSQL() method.


# Detailed TQL Information #

The TrimQuery SQL-like query language (or TQL) is a language embedded into the host language of JavaScript (or ECMAScript).  TQL works by relying on the ''with () {...}'' block statement of JavaScript.  Because of this technique, TQL must follow the language rules of JavaScript.

The major benefit of this embedded language approach (as opposed to using opaque strings) is that the TrimQuery engine can leverage the JavaScript parser and evaluator to do the hard work.  So, the TrimQuery engine itself can be pretty lightweight.

The upshot of all this is that SELECT statements in TQL look like nested function calls.

## Converting SQL Into TQL ##

You can convert a SQL SELECT statement into TQL SELECT statement with the following transformation steps.
  * First, concatenate any multi-word keywords with underscore ('_') characters._| '''Multi-word SQL Keywords''' | '''TQL Keyword''' |
|:------------------------------|:------------------|
| GROUP BY                      | GROUP\_BY         |
| ORDER BY                      | ORDER\_BY         |
| IS NULL                       | IS\_NULL          |
| IS NOT NULL                   | IS\_NOT\_NULL     |
| LEFT OUTER JOIN               | LEFT\_OUTER\_JOIN |
| INNER JOIN                    | INNER\_JOIN       |
| CROSS JOIN                    | CROSS\_JOIN       |

  * Next, add "natural" parenthesis to associate parameters with keywords.  For example...
    * SELECT X.a AS Foo, Y.**FROM X, Y WHERE X.a = Y.a  ORDER\_BY Foo ASC ...becomes...
    * SELECT'''('''X.a AS'''('''Foo''')''', Y.** FROM'''('''X, Y''')''' WHERE'''('''X.a = Y.a''')''' ORDER\_BY'''('''Foo ASC'''))'''

  * Next, convert tableName.**to tableName.ALL...
    * SELECT(X.a.AS(Foo), '''Y.ALL''' FROM(X, Y) WHERE(X.a = Y.a) ORDER\_BY(Foo ASC))**

  * Next, add a dot or period in front of every AS, ASC, DESC, ON, and USING keyword...
    * SELECT(X.a'''.AS(Foo)''', Y.ALL FROM(X, Y) WHERE(X.a = Y.a) ORDER\_BY(Foo'''.ASC'''))

  * Make sure AS and USING take strings...
    * SELECT(X.a.AS('''"Foo"'''), Y.ALL FROM(X, Y) WHERE(X.a = Y.a) ORDER\_BY(Foo.ASC))

  * Convert expression operators (like **, +, -, <, > =, ...) and their infix notation to function call style...
    * SELECT(X.a.AS("Foo"), Y.ALL FROM(X, Y) WHERE('''EQ(X.a, Y.a)''') ORDER\_BY(Foo.ASC))**

  * Next, add commas so that it's all one giant set of nested function calls...
    * SELECT(X.a.AS("Foo"), Y.ALL''',''' FROM(X, Y)''',''' WHERE(EQ(X.a, Y.a))''',''' ORDER\_BY(Foo.ASC))

That's it, you should have usable TQL now...
```
  var queryLang = TrimPath.makeQueryLang(tableColumnDefinitions);
  with (queryLang) {
      stmt = SELECT(X.a.AS("Foo"), Y.ALL, FROM(X, Y), WHERE(EQ(X.a, Y.a)), ORDER_BY(Foo.ASC))
  }
```

The ''queryLang.parseSQL()'' method does pretty much all the above for you automatically, converting SQL into TQL and returing a statement Object.

## TQL Variable Binding ##

Bound variables in TQL are easy, since it's all embedded in JavaScript...
```
  var accountNum = document.accountTransferForm.acctNum;

  with (queryLang) {
     stmt = SELECT (Account.balance, FROM (Account), WHERE (EQ(Account.num, accountNum)));
  }
```
If a variable like accountNum is a String, any quotes or backslashes are automatically backslashed by the TrimQuery engine, to prevent SQL Injection attacks.

## Alias For Expressions ##

TQL queries that have computed expressions as results need to have aliases assigned to those expressions.  You use the AS() method to assign alias names.  Aliases are needed so that computed values are accessible in the results array and are accessible in HAVING and ORDER\_BY clauses.  For example...
```
   // Here, the computation is not accessible in the results array, nor in HAVING and ORDER_BY clauses.
   statement = SELECT (SUBTRACT (Product.sellingPrice, Product.cost), 
                       FROM (Product));

   // Here, an alias of productProduct is assigned...
   statement = SELECT (SUBTRACT (Product.sellingPrice, Product.cost).AS("productProfit"), 
                       FROM (Product),
                       ORDER_BY (productProfit.DESC));

   highestProductProfit = statement.filter(tableData)[0].productProfit;
```

## Alias References ##

Because JavaScript features eager evaluation of expressions, we must be careful about defining and using aliases in TQL using the AS keyword.  The following will throw an error in TQL...
  * SELECT (inv.id, inv.date, inv.total, FROM (Invoice.AS("inv")));
Above, from JavaScript's point of view, we've referred to the "inv" variable before it's been defined.  So, to remedy this, we put the column-list after the FROM clause, ensuring that the alias reference comes after the alias definition...
  * SELECT (FROM (Invoice.AS("inv")), inv.id, inv.date, inv.total);

## Operator Expressions ##

All operators in SQL have function name equivalents in TQL...

| '''SQL Operator''' | '''TQL Operator Function Name''' | '''Number Of TQL Arguments''' |
|:-------------------|:---------------------------------|:------------------------------|
| x AND y            | AND(x, y)                        | 2 or more                     |
| x OR y             | OR(x, y)                         | 2 or more                     |
| NOT x              | NOT(x)                           | 1                             |
| x = y              | EQ(x, y)                         | 2                             |
| x != y             | NEQ(x, y)                        | 2                             |
| x < y              | LT(x, y)                         | 2                             |
| x > y              | GT(x, y)                         | 2                             |
| x <= y             | LTE(x, y)                        | 2                             |
| x >= y             | GTE(x, y)                        | 2                             |
| x IS NULL          | IS\_NULL(x)                      | 1                             |
| x IS NOT NULL      | IS\_NOT\_NULL(x)                 | 1                             |
| x + y              | ADD(x, y)                        | 2 or more                     |
| x - y              | SUBTRACT(x, y)                   | 2 or more                     |
| - x                | NEGATE(x)                        | 1                             |
| x **y**| MULTIPLY(x, y)                   | 2 or more                     |
| x / y              | DIVIDE(x, y)                     | 2 or more                     |
| (x)                | PAREN(x) -- note: PAREN is hardly used | 1                             |

  * Some operator functions can take "2 or more" arguments.  That means you can convert SQL expressions like "x AND y AND z AND w" into a single TQL method like AND(x, y, z, w).

## Reserved Keywords ##

Besides the usual reserved keywords that are derived from SQL (such as SELECT, FROM, GROUP\_BY, ...), the TQL engine also has reserved words of toString, toSql, toJs.  This means you cannot have columns with those names.

## TQL And SQL Examples ##
In this section, we show example pairs of TQL queries and their equivalent SQL queries...
```
SELECT(Invoice.ALL, FROM(Invoice));               // TQL query.
SELECT Invoice.* FROM Invoice                     // SQL query string.

SELECT(Invoice.id, Invoice.total, FROM(Invoice)); // TQL query.
SELECT Invoice.id, Invoice.total FROM Invoice     // SQL query string.

SELECT(Invoice.id, Invoice.total, Customer.acctBalance, FROM(Invoice, Customer));       
SELECT Invoice.id, Invoice.total, Customer.acctBalance FROM Invoice, Customer

SELECT(Invoice.id, Invoice.total, Customer.acctBalance, FROM(Invoice, CROSS_JOIN(Customer)));       
SELECT Invoice.id, Invoice.total, Customer.acctBalance FROM Invoice CROSS JOIN Customer

SELECT(Invoice.ALL, Customer.ALL, FROM(Invoice, Customer));
SELECT Invoice.*, Customer.* FROM Invoice, Customer

SELECT(MULTIPLY(NEGATE(DIVIDE(ADD(Invoice.total, 10, 20), 30, 40)), 50, 60), 
        PAREN(SUBTRACT(Customer.acctBalance, 70)), 
        FROM(Invoice, Customer));
SELECT (- ((Invoice.total + 10 + 20) / 30 / 40)) * 50 * 60, 
        (Customer.acctBalance - 70) 
        FROM Invoice, Customer

// In TQL, we cannot use an alias before it is defined.
// So, we move the alias reference (i.ALL) to after the FROM clause...
SELECT(FROM(Invoice.AS("i")), i.ALL);
SELECT i.* FROM Invoice AS i

SELECT(FROM(Invoice.AS("i")), i.id.AS("ID_NUM"));
SELECT i.id AS ID_NUM FROM Invoice AS i

SELECT(FROM(Invoice, Invoice.AS("i2")), Invoice.ALL, i2.ALL);
SELECT Invoice.*, i2.* FROM Invoice, Invoice AS i2

SELECT(ADD(Invoice.id, Invoice.total).AS("Foo"), FROM(Invoice));
SELECT (Invoice.id) + (Invoice.total) AS Foo FROM Invoice

SELECT(Customer.id, Invoice.custId, Invoice.total, Customer.acctBalance,
              FROM(Invoice, Customer),
              WHERE(EQ(Invoice.custId, Customer.id)));
SELECT Customer.id, Invoice.custId, Invoice.total, Customer.acctBalance 
              FROM Invoice, Customer 
              WHERE (Invoice.custId) = (Customer.id)

SELECT(Customer.id, Invoice.custId, Invoice.total, Customer.acctBalance,
              FROM(Invoice, Customer),
              WHERE(AND(LT(Invoice.total, 250),
                        EQ(Invoice.custId, Customer.id))));
SELECT Customer.id, Invoice.custId, Invoice.total, Customer.acctBalance 
              FROM Invoice, Customer 
              WHERE ((Invoice.total) < (250)) AND ((Invoice.custId) = (Customer.id))

var injectionVal = "' OR 1=1 -- ha ha ";
SELECT(Invoice.id, FROM(Invoice), WHERE(EQ(Invoice, injectionVal)));
SELECT Invoice.id FROM Invoice WHERE (Invoice) = ('\\' OR 1=1 -- ha ha ')

SELECT(Invoice.id, FROM(Invoice), ORDER_BY(Invoice.id));
SELECT Invoice.id FROM Invoice ORDER BY Invoice.id"

SELECT(Invoice.id, FROM(Invoice), ORDER_BY(Invoice.id.ASC));
SELECT Invoice.id FROM Invoice ORDER BY Invoice.id ASC

SELECT(Invoice.id, FROM(Invoice), ORDER_BY(Invoice.id.DESC));
SELECT Invoice.id FROM Invoice ORDER BY Invoice.id DESC

SELECT(Invoice.custId, Invoice.id, FROM(Invoice), ORDER_BY(Invoice.custId.DESC, Invoice.id));
SELECT Invoice.custId, Invoice.id FROM Invoice ORDER BY Invoice.custId DESC, Invoice.id

SELECT(Refund.ALL, FROM(Refund), LIMIT(100));
SELECT Refund.* FROM Refund LIMIT 100

SELECT(Refund.ALL, FROM(Refund), LIMIT(100, 50));
SELECT Refund.* FROM Refund LIMIT 100 OFFSET 50

SELECT(Invoice.total, 
              SUM(Invoice.total).AS("SUM_total"), 
              COUNT(Invoice.total).AS("COUNT_total"), 
              AVG(Invoice.total).AS("AVG_total"), 
              FROM(Invoice));
SELECT Invoice.total, 
              SUM (Invoice.total) AS SUM_total,
              COUNT (Invoice.total) AS COUNT_total,
              AVG (Invoice.total) AS AVG_total 
              FROM Invoice

SELECT(Invoice.total, 
              SUM(Invoice.total).AS("SUM_total"), 
              COUNT(Invoice.total).AS("COUNT_total"), 
              AVG(Invoice.total).AS("AVG_total"), 
              FROM(Invoice), 
              GROUP_BY(Invoice.custId));
SELECT Invoice.total, 
              SUM (Invoice.total) AS SUM_total,
              COUNT (Invoice.total) AS COUNT_total,
              AVG (Invoice.total) AS AVG_total 
              FROM Invoice 
              GROUP BY Invoice.custId

SELECT(Invoice.total, 
              SUM(Invoice.total).AS("SUM_total"), 
              FROM(Invoice), GROUP_BY(Invoice.custId), ORDER_BY(SUM_total.ASC));

SELECT Invoice.total, 
              SUM (Invoice.total) AS SUM_total
              FROM Invoice GROUP BY Invoice.custId ORDER BY SUM_total ASC

SELECT(Invoice.total.AS("TOT"), 
              SUM(Invoice.total).AS("SUM_total"), 
              FROM(Invoice), 
              GROUP_BY(Invoice.custId), 
              HAVING(LT(TOT, 150)),
              ORDER_BY(SUM_total.ASC));
SELECT Invoice.total AS TOT, 
              SUM (Invoice.total) AS SUM_total
              FROM Invoice
              GROUP BY Invoice.custId 
              HAVING (TOT) < (150)
              ORDER BY SUM_total ASC

SELECT(Invoice.total.AS("TOT"), 
              SUM(Invoice.total).AS("SUM_total"), 
              FROM(Invoice), 
              GROUP_BY(Invoice.custId), 
              HAVING(GT(SUM_total, 550)),
              ORDER_BY(SUM_total.ASC));
SELECT Invoice.total AS TOT, 
              SUM (Invoice.total) AS SUM_total
              FROM Invoice
              GROUP BY Invoice.custId 
              HAVING (SUM_total) > (550)
              ORDER BY SUM_total ASC

SELECT(Invoice.total.AS("TOT"), 
              SUM(Invoice.total).AS("SUM_total"), 
              FROM(Invoice), 
              GROUP_BY(Invoice.custId), 
              HAVING(GT(SUM_total, 500000)),
              ORDER_BY(SUM_total.DESC));
SELECT Invoice.total AS TOT, 
              SUM (Invoice.total) AS SUM_total
              FROM Invoice
              GROUP BY Invoice.custId 
              HAVING (SUM_total) > (500000)
              ORDER BY SUM_total DESC

SELECT(Customer.id, Invoice.total, FROM(Customer, INNER_JOIN(Invoice)));
SELECT Customer.id, Invoice.total FROM Customer INNER JOIN Invoice

SELECT(Customer.id, Invoice.total, 
             FROM(Customer, INNER_JOIN(Invoice).ON(EQ(Customer.id, Invoice.custId))));
SELECT Customer.id, Invoice.total 
             FROM Customer INNER JOIN Invoice ON (Customer.id) = (Invoice.custId)

// Note due to alias rules, the columns-list (which refers to the I alias) comes last.
SELECT(FROM(Customer, INNER_JOIN(Invoice.AS("I")).ON(EQ(Customer.id, I.custId))), 
             Customer.id, I.total);
SELECT Customer.id, I.total 
             FROM Customer INNER JOIN Invoice AS I ON (Customer.id) = (I.custId) 
            
SELECT(Invoice.id, Invoice.total, Refund.amount, 
             FROM(Invoice, INNER_JOIN(Refund).USING("custId")));
SELECT Invoice.id, Invoice.total, Refund.amount 
             FROM Invoice INNER JOIN Refund USING (custId)
            
SELECT(FROM(Invoice.AS("I"), INNER_JOIN(Refund.AS("R")).USING("custId")),
              I.id, I.total, R.amount);
SELECT I.id, I.total, R.amount 
             FROM Invoice AS I INNER JOIN Refund AS R USING (custId)
            
SELECT(Customer.id, Invoice.total, 
             FROM(Customer, LEFT_OUTER_JOIN(Invoice).ON(EQ(Customer.id, Invoice.custId))));
SELECT Customer.id, Invoice.total 
             FROM Customer LEFT OUTER JOIN Invoice ON (Customer.id) = (Invoice.custId)
```

## Unsupported SQL Features ##

Features of SQL SELECT that are not (yet?) supported in TQL include...
  * BETWEEN
  * IN (for lists and for sub-queries or nested SELECT's)
  * ANY
  * EXISTS
  * DISTINCT
  * AVG\_DISTINCT, SUM\_DISTINCT, COUNT\_DISTINCT
  * RIGHT\_OUTER\_JOIN
  * FULL\_OUTER\_JOIN
  * UNION, EXCEPT, INTERSECT

[SteveYen: Please don't hesitate to jump right with your own implementations or contributions in if you wish.  There's nothing like implementing a small SQL-like engine to helping one's understanding and appreciation for SQL! ](.md)


---

{ [TrimQuery home](http://code.google.com/p/trimpath/wiki/TrimQuery) -- [API](http://code.google.com/p/trimpath/wiki/TrimQueryAPI) | [syntax](http://code.google.com/p/trimpath/wiki/TrimQuerySyntax) | [download](http://code.google.com/p/trimpath/downloads/list) | [community](http://code.google.com/p/trimpath/wiki/TrimQueryDiscussion) }