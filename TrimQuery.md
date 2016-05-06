# TrimQuery #
{ [TrimQuery home](http://code.google.com/p/trimpath/wiki/TrimQuery) -- [API](http://code.google.com/p/trimpath/wiki/TrimQueryAPI) | [syntax](http://code.google.com/p/trimpath/wiki/TrimQuerySyntax) | [download](http://code.google.com/p/trimpath/downloads/list) | [community](http://code.google.com/p/trimpath/wiki/TrimQueryDiscussion) }

For rich web application developers, the '''TrimQuery''' engine from TrimPath is a lightweight [GPL/APL open-source](http://code.google.com/p/trimpath/wiki/TrimPathLicense) component that lets you have the power of SQL queries while running in a web browser.

Here's an online [demo](http://trimpath.com/demos/test1/trimpath/query_demo.html).

  * The '''TrimQuery''' engine is written entirely in standard JavaScript.
  * The engine features a SQL-like query language with support for...
    * INSERT, UPDATE, DELETE
    * SELECT ... FROM
    * WHERE clauses
    * LIKE support
    * ORDER BY (sorting on multiple columns, ASC and DESC)
    * AS (aliases)
    * GROUP BY, HAVING aggregation
    * SUM, COUNT, AVG aggregate functions
    * self joins
    * LIMIT and offsets
    * parameterization
  * By "lightweight", we mean that TrimQuery is less than 700 lines of JavaScript code (as of 2005/03/03, release 1.0.28).

Here's a sample query in TrimQuery...
```
   SELECT Customer.id, Customer.acctBalance, Invoice.total
          FROM Customer, Invoice
          WHERE Customer.id = Invoice.custId
          ORDER BY Customer.id ASC
```

NEWS:
  * 2007/08/15 - TrimQuery 1.1.14 released with big features (INSERT, UPDATE, DELETE, and more) contributed by Brian Moschel from http://jupiterit.com
    * See the [blog entry about version 1.1.14](http://trimpath.com/blog/?p=76).
  * 2006/01/18 - TrimQuery + TiwyWiki (Offline Flash tech by Julien Couvreur) == interesting possibilities??
    * http://blog.monstuff.com/archives/000272.html
  * 2005/10/22 - TrimQuery + AMASS == wow
    * http://www.sysbotz.com/articles/jsdb/index.htm
    * http://codinginparadise.org/weblog/2005/10/javascript-sql-database-with-permanent.html
  * 2005/07/18 - New release - 1.0.38
    * Allow tables to be undefined/null in the dataTables even though they might be defined in the schema.
    * Fixed an ORDER BY bug when comparing undefined or null values.
    * Exposed LEFT OUTER JOIN feature to SQL, not just to TQL.
    * Works with other libraries like [Prototype](http://prototype.conio.net), which affect Object.prototype.

If you're building the next modern, brilliantly rich web application (like GMail/OddPost/Bloglines), you may find yourself caching lots of domain objects in your browser's memory.  If you want to '''slice and dice''' that local client-side data cache, try TrimQuery.  It's the lightweight alternative to approaches like loading in a Java applet (perhaps with HSQL or Apache Derby) or going with an ActiveX control.

Or worse, you could manually write tons of code to search, join, group-by, aggregate and sort your in-browser data.  Instead, why can't you just leverage your familiarity with SQL?  Don't leave SQL just in your server side toolbox!  TrimQuery lets you bring your SQL knowledge into the web browser.

More information:
  * '''10 Minute Introduction''' -- see below.
  * [TrimQuery API](http://code.google.com/p/trimpath/wiki/TrimQueryAPI)
  * [TrimQuery SQL Syntax](http://code.google.com/p/trimpath/wiki/TrimQuerySyntax)
  * [TrimQuery Downloads](http://code.google.com/p/trimpath/downloads/list)
  * [TrimQuery Community Wiki](http://code.google.com/p/trimpath/wiki/TrimQueryDiscussion)
  * [TrimQuery Browser Compatibility](http://code.google.com/p/trimpath/wiki/TrimQueryRunsIn)
  * [TrimQuery demo page](http://trimpath.com/demos/test1/trimpath/query_demo.html)

## 10 Minute Introduction To TrimQuery ##
First, in our HTML page, we load the TrimQuery component into our web browser...
```
  <html>
    <head>
      <script language="javascript" src="trimpath/query.js"></script>
      ...
    </head>
    ...
  </html>
```
Next, we create some structured JavaScript data records...
```
  <script language="javascript">
    var tableData = { 
        Invoice  : [ { id: 1, total: 100, custId: 10 }, 
                     { id: 2, total: 200, custId: 10 }, 
                     { id: 3, total: 300, custId: 10 }, 
                     { id: 4, total: 400, custId: 20 } ],
        Customer : [ { id: 10, acctBalance: 1000 }, 
                     { id: 20, acctBalance: 2000 }, 
                     { id: 30, acctBalance: 3000 } ]
    };
  </script>
```
We also create some structured JavaScript to define our tables (or schema)...
```
  <script language="javascript">
    var columnDefs = {
        Invoice  : { id          : { type: "String" },
                     total       : { type: "Number" },	
                     custId      : { type: "String" } },
        Customer : { id          : { type: "String" },
                     acctBalance : { type: "Number" } }
    };
  </script>
```
Next we define and execute a query using the [API](http://code.google.com/p/trimpath/wiki/TrimQueryAPI)...
```
  <script language="javascript">
    // First we precompile the query language object with the schema...
    var queryLang = TrimPath.makeQueryLang(columnDefs);

    // Next, we do a SELECT statement.
    var statement = queryLang.parseSQL(
      "SELECT Customer.id, Customer.acctBalance, Invoice.total " +
          "FROM Customer, Invoice " +
          "WHERE Customer.id = Invoice.custId " +
          "ORDER BY Customer.id ASC");

    // Here we run the query...
    var results = statement.filter(tableData);

    // Tada!  That's it -- the results now holds the joined,
    // filtered, and sorted output of our first query.

    // Also, we can convert the statement back to a SQL string...
    statement.toString() == 
       "SELECT Customer.id, Customer.acctBalance, Invoice.total " + 
              "FROM Customer, Invoice " +
              "WHERE Customer.id = Invoice.custId " +
              "ORDER BY Customer.id ASC"
  </script>
```
And the value of the results variable would be...
```
    [ { id: 10, acctBalance: 1000, total: 100 },
      { id: 10, acctBalance: 1000, total: 200 },
      { id: 10, acctBalance: 1000, total: 300 },
      { id: 20, acctBalance: 2000, total: 400 } ]
```

You can try this right now using the online [TrimQuery demo](http://trimpath.com/demos/test1/trimpath/query_demo.html)

If you want to convert the results into HTML or into a fragment of HTML, take a look at TrimPath's [JavaScript Templates](http://code.google.com/p/trimpath/wiki/JavaScriptTemplates) technology.  The JST component lets you have template based programming (like PHP/ASP/JSP) but runs entirely in your web browser.

Here's another query that shows off GROUP BY, HAVING, SUM, aliases, and parameters...
```
  <script language="javascript">
    statement = queryLang.parseSQL(
        "SELECT Customer.id, SUM(Invoice.total) AS invoiceTotals " +
               "FROM Customer, Invoice " +
               "WHERE Customer.id = Invoice.custId AND Customer.acctBalance > ? " +
               "GROUP BY Customer.id " +
               "HAVING invoiceTotals > ? ",
        [100, 500]);
  </script>  
```


---

## TrimQuery On The Server-Side ##
We designed TrimQuery to run in a web browser AND also in any standalone JavaScript interpreter (such as Mozilla Rhino or Spider Monkey).  The core TrimQuery engine is meant to have no critical DOM/DHTML/browser dependencies.


---

{ [TrimQuery home](http://code.google.com/p/trimpath/wiki/TrimQuery) -- [API](http://code.google.com/p/trimpath/wiki/TrimQueryAPI) | [syntax](http://code.google.com/p/trimpath/wiki/TrimQuerySyntax) | [download](http://code.google.com/p/trimpath/downloads/list) | [community](http://code.google.com/p/trimpath/wiki/TrimQueryDiscussion) }