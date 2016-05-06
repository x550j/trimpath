# TrimSpreadsheet Community Wiki #
{ [TrimSpreadsheet home](http://code.google.com/p/trimpath/wiki/TrimSpreadsheet) -- [download](http://code.google.com/p/trimpath/downloads/list) | [community](http://code.google.com/p/trimpath/wiki/TrimSpreadsheetDiscussion) }


---

'''UPDATE: Please see the [TrimPath Forum](http://trimpath.com/forum/index.php) for the latest place to discuss stuff.'''

We'll keep these community discussion wiki pages around, though, for reference.  -- SteveYen 2005/07/10


---

Hi all.  This is your shared, community space, hopefully mostly about TrimSpreadsheet.  -- SteveYen

How can a user save the table? -- EliotLandrum
  * Congrats on having the 1st question.  That's something I'm still contemplating.  In general, I think any solution will require cooperation from some server-side CGI-like program, like PHP/ASP/Java.  You could, for example, have some more JavaScript that takes the edited table and POST's it up to the server, or use XMLHttpRequest techniques.  Related, in the demos, you should find a button labeled "Show Table Source".  The event handling code behind those buttons shows how to extract the table HTML which should be sent to the server. If you're familiar with the TiddlyWiki story, it'll be a very similar solution. Thanks. -- SteveYen
  * Followup, I've put up an app that shows off spreadsheet saving at http://numsum.com -- SteveYen


---

Hi!  Is there a way to enable a formula to recognize a number as such, even when the cell includes html tags?  For example, allow Sum() to include <b>34</b> in it's calculation?

ans: not right now for arbitrary tags.  By the way, for your example of a bold (<b>) 34, you can use the alternative of <td>34</td>, and that will work.  -- SteveYen<br>
<hr />
Hi! Is there a way to enable a user to add or remove columns to the spreadsheet (i.e. add a menu option to add a column?)?<br>
<br>
<ul><li>I'll take a shot at this one.  As far as your HTML page is concerned, the spreadsheet is just a TABLE element.  If you can use JavaScript to add a row to a table, then you can add a row to the spreadsheet.  For a column you could add/remove a COL element to the table head and add/remove a TD element to each row in the table body.  Once you're done modifying the table structure, call "TrimPath.spreadsheet.initDocument()" and voila.  If you don't know how to do what I'm describing in JavaScript, google JavaScript DOM programming or JavaScript DHTML.</li></ul>

<hr />
I'm trying a bit of fun by combining TrimSpreadsheet with some features of Michael Schwarz' excellent AJAX.NET package.  I can get an asynchronously generated DataTable into javascript and I want to dump it into a spreadsheet (values only for now, formulas later.)<br>
<br>
If I use DOM to build the table with class "spreadsheet", append it to the innermost of the three standard DIVs, and call TrimPath.spreadsheet.initDocument(), I get an error:<br>
<br>
<ul><li>'spreadsheetBarTop.rows.0.cells' is null or not an object</li></ul>

From spreadsheet_ui.js, Line 176.  Any advice?<br>
<br>
Thanks for sharing this great component, i'm learning a lot from it.<br>
<br>
<ul><li>Update: I answered my own question.  If you're assembling the table using DOM, the COL nodes need to be children of a COLGROUP which is the first child of the TABLE, and the TR's need to be children of a TBODY which is a subsequent child of the TABLE.  The browser fills in these blanks when it reads other markup but with DOM you need to provide it yourself.<br>
{ <a href='http://code.google.com/p/trimpath/wiki/TrimSpreadsheet'>TrimSpreadsheet home</a> -- <a href='http://code.google.com/p/trimpath/downloads/list'>download</a> | <a href='http://code.google.com/p/trimpath/wiki/TrimSpreadsheetDiscussion'>community</a> }</li></ul>

<hr />
'''UPDATE: Please see the <a href='http://trimpath.com/forum/index.php'>TrimPath Forum</a> for the latest place to discuss stuff.'''<br>
<br>
We'll keep these community discussion wiki pages around, though, for reference.  -- SteveYen 2005/07/10<br>
<br>
<hr />
Hi,<br>
These is any way save the spreadsheet in DB and load it from DB.<br>
<br>
I need create the web application as like this:<br>
<br>
The tree:<br>
<br>
Node 1<br>
<blockquote>-- Node 1 -1<br>
-- Node 1 -2<br>
Node 2<br>
-- Node 2 - 1<br>
-- ...</blockquote>

Each node is one spread sheet, When user click on tree node -> the system will display the spreadsheet (data) of the selected node.<br>
<br>
Thanks for help.<br>
<br>
Wallness