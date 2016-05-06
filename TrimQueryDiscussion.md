# TrimQuery Discussion #
{ [TrimQuery home](http://code.google.com/p/trimpath/wiki/TrimQuery) -- [API](http://code.google.com/p/trimpath/wiki/TrimQueryAPI) | [syntax](http://code.google.com/p/trimpath/wiki/TrimQuerySyntax) | [download](http://code.google.com/p/trimpath/downloads/list) | [community](http://code.google.com/p/trimpath/wiki/TrimQueryDiscussion) }


---

'''UPDATE: Please see the [TrimPath Forum](http://trimpath.com/forum/index.php) for the latest place to discuss stuff.'''

We'll keep these community discussion wiki pages around, though, for reference.  -- SteveYen 2005/07/10


---

Ok, this is an open wiki page.  Please don't hold back your effusive praise, awe or indignant shock about TrimQuery.

As for me, I was familiar with SQL and relational queries before doing TrimQuery.  Now, though, after writing my own table joiner, aggregate accumulator, and other parts of a small relational query engine, I feel I '''grok''' SQL query theory now.  I have a much larger appreciation the hard work than RDBMS kernel guys go through and how much work the RDBMS has to do against bad SQL that "SQL tourists" like me used to put out.

-- SteveYen

(first congrats on the project, those things are really cool)

It's about TrimQuery 1.0.6

As the Trac Tickets for TrimQuery are not yet alive, I'm posting here:
In IE 6.0.28/Win2k query.js chokes in line 279 (the final of NodeType / before "var setSSFunc...").

solution: closing the "join" function there's a "," (ln 277) -- chop it. IE thinks that there will be more definitions and all-of-the-sudden :) it ends.

Hope that helps.

-- BJessen

Thanks much! I've put up a new version of TrimQuery (1.0.18) with your fix [for download](http://code.google.com/p/trimpath/downloads/list).

-- SteveYen

Make a server-side component that defines the data structure, exports it, and saves changes, all via Ajax and a new "COMMIT" statement. ;)

-- ThomasLackner (whom you met yesterday)

Howdy everybody.  I'm considering getting in on this, using and also eventually contributing to the source base.  Quick questions:
  * What's the user count, roughly (maybe unique IP's downloading)?
    * Don't know, but I humbly surmise it would be tiny. -- SteveYen
  * How many developers are working on this?
    * Mostly me.  Or, honestly, less than 100% of me. -- SteveYen
  * Anybody using this against 100+, hopefully 1000+, objects yet?  How's the performance?
    * I haven't really pushed it in the nastiest ways I can think of yet, but using it against tables with 1000's of records has been fine for me.  YMMV.  -- SteveYen

And is there a Date type?  Or do we rough up some string like "2005.06.12.18.03" (for June 12, 6:03pm)?  Would that work with ORDER BY?
  * Answer: just as with SQLite, there's no Date type.  The string encoding is the normal answer, which works for ORDER BY.
-- ChristopherGaltenberg

Glad you stopped by and are taking a look.  I've added some quick answers above.  Also, good news: this stuff is young.  Kinda like jumping onto an undiscovered, promising stock.  And contributions can have major impact at this point.  Bad news: this stuff is young, which bites if you want something with all the risk and surprise hammered out of it already.

Also, here's a javascript date-to-string function that I use...
```
        toSQLDateString = function(date) {
            if (date == null)
                date = new Date();
            return leftZeroPad(date.getUTCFullYear(), 4) + '/' + 
                   leftZeroPad(date.getUTCMonth() + 1, 2) + '/' + 
                   leftZeroPad(date.getUTCDate(), 2) + ' ' + 
                   leftZeroPad(date.getUTCHours(), 2) + ':' + 
                   leftZeroPad(date.getUTCMinutes(), 2) + ':' + 
                   leftZeroPad(date.getUTCSeconds(), 2) + ' UTC';
        }
        leftZeroPad = function(val, minLength) {
            if (typeof(val) != "string")
                val = String(val);
            return (MANY_ZEROS.substring(0, minLength - val.length)) + val;
        }
        var MANY_ZEROS = "000000000000000000";
```

-- SteveYen

Many thanks, Steve.  I'm going to start with it, and I'll check back in.  I won't be taxing it beyond the 1000s of records count for awhile, but I will be taking a shot at creating a page that also runs on Pocket PC (which has full javascript 1.5, but a crappy DOM), so should have some interesting times ahead.  Thanks again for this great thing - will be a tremendous help in this ajax endeavor.  -- ChristopherGaltenberg


---


Very sweet code..
Not to compound your workload or anything but any thought as to implementing INSERT,UPDATE queries. I have the issue of wanting to update parts of the dataset instead of reloading on every change. Anyway, maybe i'll have a deeper look at the code..

-- TiM

Thanks.  I've been so far been able to lazily avoid needing query-based INSERTs/UPDATEs/DELETEs in my own usage by just using the simplest possible alternative -- which is just using javascript to manipulate the client-side record hashes directly.  So far, I've only needed to do primary-key-based, single-record updates/deletes rather than query-based update/deletes, so that's why I haven't gotten around to it.  I'm sure one day I'll run into needing something like delete an order record and all the orderline records that are joined to it, though.  cheers.  -- SteveYen

Those priorities sound right - When it comes to things like true INSERT and UPDATE to datasets/databases, there's 1000 different alternatives, most of which should be done on the server side.

I'd say the best features to have next are LIKE and IN.  Humbly said from (still) the outside.  Congrats again on everything so far.  Wonderful.  -- ChristopherGaltenberg

Thanks Steve. Indeed i've only needed the single record updates as well so I'll stick to manipulating the array directly. Actually the issue of syncronizing client side datasets is a bit of a hairy one so perhaps best I don't hurt my brain thinking about it, hehe - TiM :)

Yeah, that's a tough one.  I've been tackling it with TrimJunction, one small step at a time.  -- SteveYen


---


On a side note, I've really learned a lot about Javascript by studying your code, as well as thru Jeremy Ruston's TiddlyWiki.  I'm curious... how did you learn about these more advanced Javascript techniques?  Is there a book or site of patterns and practices?  Besides the Oreilly books, who's teaching the true power of the JS language?  Or does it just come with a lot of playing?  :)  Thanks again. --CmG

Oh, I'm going to sound old now -- My 2nd language after Basic was Lisp.  Recently, the lightbulb went on when I saw Douglas Crockford's writings about JavaScript.  Once you think of JavaScript as a dialect of Lisp, then lots of mental doors open.  Unfortunately, you gotta learn Lisp first.

Also, once I figured out a mental model for how "prototype" works, the road got a little smoother.  If you know C++ and about its virtual-tables (vtables), or how Java works with its method dispatching, the JavaScript's prototype object system makes sense -- at least to me.  Instead of an index-based vtable method array (like C++), imagine it's a hash-table of functions (or properties) keyed by name (then that's (handwave) kind of like Java).  Also, imagine you could programmatically have access to these hash-tables and that you could "chain" these lookup hash-tables together into a prototype lookup chain.  Then that's how JavaScript prototypes work.  I guess my head is now a JavaScript attic built on top of a house of cards of mental models of other languages with a Lisp foundation.

-- SteveYen


---


Two questions:

1)  When the '**' operator is used, if you query objects that are actually '''bigger''' than the schema (they have more properties, they are fuller objects), would it be possible to pass back ''all'' fields from those objects, rather than just what is defined in the schema?  So you'd get your same objects back (just a smaller set, as a result of the query).**

2)  Do you -really- need to differentiate between String and Number?  Are the operations not correctly interpreted, implicitly, by Javascript?  In other words, ''do you really need a schema definition'' (could you just query basic JS objects)?

I would def be willing to pay the price of qualifying the type somehow (like .asString or .asNumber suffixes) when this implicit interpretation didn't work... or even simpler, if I don't provide a schema and it doesn't work, then it just doesn't work.  Is there a possibility of removing the schema requirement?

---

Answer: The '**' operation does require the schema object in order to figure out what fields to put into the resultset.  But, a little secret: the current implementation kind of, ahem, doesn't use the type info that's passed in and which is shown in all the usage examples today.  I think.  Fair warning, this may change.**

And, the vision was to one day to allow the same hunk of JavaScript code to run on both the client AND the server where you'd be hitting against real database (bringing JavaScript/LiveScript back full circle to where Netscapers originally intended.)  So your TrimJunction logic would be "write once, run anywhere".  So, you SHOULD be passing in a nice schema that will be used in the future.

BTW, I actually had the opposite need, where I had records that I wanted to query that were '''smaller''' than the schema definition.  In that case, the schema def was necessary.

On item (2), again the current implementation ignores the column type info.  As you surmise, TrimQuery just relies on JavaScript to correctly compare Strings against Strings and numbers against numbers.

Maybe a flag or alternative method which relies on more duck typing would be a good future feature.

-- SteveYen


---

'''UPDATE: Please see the [TrimPath Forum](http://trimpath.com/forum/index.php) for the latest place to discuss stuff.'''

We'll keep these community discussion wiki pages around, though, for reference.  -- SteveYen 2005/07/10


---

{ [TrimQuery home](http://code.google.com/p/trimpath/wiki/TrimQuery) -- [API](http://code.google.com/p/trimpath/wiki/TrimQueryAPI) | [syntax](http://code.google.com/p/trimpath/wiki/TrimQuerySyntax) | [download](http://code.google.com/p/trimpath/downloads/list) | [community](http://code.google.com/p/trimpath/wiki/TrimQueryDiscussion) }