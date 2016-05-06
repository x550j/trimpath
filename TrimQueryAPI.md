# TrimQuery API #
{ [TrimQuery home](http://code.google.com/p/trimpath/wiki/TrimQuery) -- [API](http://code.google.com/p/trimpath/wiki/TrimQueryAPI) | [syntax](http://code.google.com/p/trimpath/wiki/TrimQuerySyntax) | [download](http://code.google.com/p/trimpath/downloads/list) | [community](http://code.google.com/p/trimpath/wiki/TrimQueryDiscussion) }

## Using TrimQuery ##
First, in your HTML/JSP/PHP/ASP file, include the trimpath/query.js JavaScript file.
```
  <script language="javascript" src="trimpath/query.js"></script>
```

The trimpath/query.js file can live anywhere you want and may be renamed.  It has no dependencies to other files.  We suggest using a subdirectory named trimpath to help you remember what it's about and also give you a space to place more (future) TrimPath components.  Future TrimPath components might depend on living in the same directory as trimpath/query.js.

After including trimpath/query.js, a variable named TrimPath will be ready for you to use.


---

## The TrimPath Object ##
The TrimPath Object is the global singleton object that is the access point for all TrimPath components.  Except for this TrimPath Object, we try to keep the global variable namespace clean for you.

The TrimPath Object will have one TrimQuery-related method...

### TrimPath.makeQueryLang ( tableColumnDefinitions ) ###
This method compiles the '''tableColumnDefinitions''' and returns a queryLang Object.

The '''tableColumnDefinitions''' parameter should be a 2-level nested data structure that defines the schema of the tables.  The first-level keys are table names.  The table names are case sensitive so "Invoice" is a different table name than "invoice".  The values are column definition information.
For example...
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
Invoice is the name of the table with the columns of id, total, and custId.  The Customer table has columns of id and acctBalance.

NOTE: future versions of TrimQuery might feature deeper usage of the '''tableColumnDefinitions''' structure, such as defining more information in the column definition.

The TrimPath.makeQueryLang() method returns a value known as a queryLang Object.  Caching of these returned queryLang Objects is up to you.  The TrimPath.makeQueryLang() method is stateless, so that multiple invocations of TrimPath.makeQueryLang() on the same '''tableColumnDefinitions''' Object will each return a different, newly constructed queryLang Object.

You can now use the queryLang.parseSQL() method to parse SQL strings into selectStatement Objects.

### queryLang.parseSQL ( sqlString, optionalParamsArray ) ###
The parseSQL() method on the queryLang Object takes a SQL SELECT string (the '''sqlString'''), integrates any optional parameters (the '''optionalParamsArray'''), parses it all, and returns a selectStatement Object on success.  It throws an exception on any parsing error.

Some examples:
```
var selectStatement;
selectStatement = queryLang.parseSQL("SELECT Customer.* FROM Customer");

selectStatement = queryLang.parseSQL(
  "SELECT Customer.* FROM Customer ORDER BY Customer.acctBalance");
```

Parameter placeholders in the '''sqlString''' are specified with a '?' character.  String parameters are automatically quoted and escaped to prevent SQL injection attacks.
```
selectStatement = queryLang.parseSQL(
  "SELECT Customer.* FROM Customer WHERE Customer.acctBalance > ?", [ 500 ]);

selectStatement = queryLang.parseSQL(
  "SELECT Customer.* FROM Customer " +
                    "WHERE Customer.acctBalance > ? " +
                      "AND Customer.acctBalance < ?", [ minBalance, maxBalance ]);
```

Please see the [query language syntax](http://code.google.com/p/trimpath/wiki/TrimQuerySyntax) page for more information on the specific subset of SQL SELECT syntax that's supported by the parseSQL() method.

If you're interested in the underlying parse language implementation of TrimQuery, please see [TrimQuery TQL](http://code.google.com/p/trimpath/wiki/TrimQueryTql).


---

## selectStatement Object ##
A selectStatement Object is the return value of any successfully parsed SELECT query definitions from parseSQL().  A selectStatement Object has the following methods...

### selectStatement.filter ( tableData, options ) ###
The '''selectStatement.filter()''' method performs the query represented by the selectStatement on the '''tableData''' input.  In the results, each record's keys will be defined according to the SELECT column list and aliases.  The returned results array will already be sorted and truncated as determined by any ORDER BY and LIMIT clauses.

The '''selectStatement.filter()''' method is stateless.  It may be invoked multiple times over the same or different '''tableData''', and a new results array will be returned every time.  Caching of results is up to you.

The '''tableData''' input should be a 2-level nested data structure.  The keys are table names.  The values are arrays of records for each table.  The structure of the '''tableData''' records must match the '''tableColumnDefinitions'''.

The '''options''' are an optional hash or map to control the filtering.  Valid options are '''with\_table''', which is a boolean (defaults to false).  Setting with\_table option to true in that hash will return the data in the format table\_name+’.’+field\_name as the keys in the resulting hash. This is useful for SELECT queries containing LEFT OUTER JOINs or other joins where there will be multiple tables represented in the results.

For example...
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

    var myResults = selectStatement1.filter(tableData);

    for (var i = 0; i < myResults.length; i++) {
        var record = myResults[i];
        // ...
    }
  </script>
```

An example of the with\_table:
```
  var rs = stmt.filter(tableData, null, {with_table: true});
  // The rs will contain a records resembling:
  // [{Invoice.id: 1, Customer.id: 2, Customer.name: ‘Pete’, ...},
  //  {Invoice.id: ...}]
```

### selectStatement.toString () ###

The '''selectStatement.toString()''' method returns a String of the SQL text that represents the selectStatement Object.  This SQL string is meant to be legal-ish SQL and is often used as a debugging aid.


---

{ [TrimQuery home](http://code.google.com/p/trimpath/wiki/TrimQuery) -- [API](http://code.google.com/p/trimpath/wiki/TrimQueryAPI) | [syntax](http://code.google.com/p/trimpath/wiki/TrimQuerySyntax) | [download](http://code.google.com/p/trimpath/downloads/list) | [community](http://code.google.com/p/trimpath/wiki/TrimQueryDiscussion) }