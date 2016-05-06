# TrimSpreadsheet #
{ [TrimSpreadsheet home](http://code.google.com/p/trimpath/wiki/TrimSpreadsheet) -- [download](http://code.google.com/p/trimpath/downloads/list) | [community](http://code.google.com/p/trimpath/wiki/TrimSpreadsheetDiscussion) }

'''TrimSpreadsheet''' is a lightweight [open-source](http://code.google.com/p/trimpath/wiki/TrimPathLicense) JavaScript spreadsheet engine.  It let's you work with spreadsheet data right in your web browser.  As with all TrimPath components, the '''TrimSpreadsheet''' engine is written as 100% standards-based JavaScript and CSS.  Spreadsheets are stored as HTML <table> markup, so it's easy to understand and embed into your own web pages.  Merely assign a class of "spreadsheet" to your <table>, and the TrimSpreadsheet engine will do the rest.  Anyone familiar with CSS can also easily create their own spreadsheet themes.<br>
<br>
<h2>Demos</h2>
Want to see TrimSpreadsheet in action?  Here's some demos...<br>
<ul><li><a href='http://trimpath.com/demos/test1/trimpath/spreadsheet_demo0.html'>software analysis spreadsheet</a>.<br>
</li><li><a href='http://trimpath.com/demos/test1/trimpath/spreadsheet_demo1.html'>product comparison spreadsheet</a>.<br>
</li><li><a href='http://trimpath.com/demos/test1/trimpath/spreadsheet_blank.html'>blank spreadsheet</a>.<br>
Note: these demos run the latest, most featureful development code.  Please see the <a href='http://code.google.com/p/trimpath/downloads/list'>download</a> page if you want a stable prior release.</li></ul>

<h2>Features</h2>
<ul><li>Formulas.<br>
<ul><li>SUM, COUNT, AVERAGE, MIN, MAX.<br>
</li><li>Math operators <b>/+-, parenthesis.<br>
</li><li>Any JavaScript expression.<br>
</li></ul></li><li>Styling And Fonts.<br>
<ul><li>CSS styling fully supported.<br>
</li><li>Bold, italic, underline, left/center/right, etc.<br>
</li></ul></li><li>Recalculation Engine Features.<br>
<ul><li>Automated recalculations (by default).<br>
</li><li>API-controllable recalculations, for firing off big recalculations just when you want.<br>
</li><li>Cycle detection, so no infinite loops during recalculation.<br>
</li><li>The recalculation engine is '''cooperative''', so you can optionally programmatically split a large recalculation into multiple phases.  If you expect users to have huge spreadsheets, this can help ensure that the engine does not lock up the browser UI.<br>
</li><li>The recalculation engine is a separate sub-component that can be used without the UI component.<br>
</li></ul></li><li>Simple<br>
<ul><li>TrimSpreadsheet follows unobtrusive JavaScript design principles, so a spreadsheet is as simple as an HTML</b><table>.<br>
</li><li>TrimSpreadsheet markup is human readable, highly semantic and search indexer friendly.<br>
</li><li>The TrimSpreadsheet engine is smaller than most Excel spreadsheets.<br>
</li></ul></li><li>Hypertext Cell Contents.<br>
<ul><li>HTML fragments are supported in your spreadsheet cells.<br>
</li></ul></li><li>Cross-browser.<br>
</li><li>Configurable.<br>
<ul><li>Scrollbars, row/column header bars, and formula controls are all controllable thru simple HTML markup.<br>
</li></ul></li><li>Cool Only In A Browser<br>
<ul><li>Only those who've programmed against DHTML/DOM would think this is neat, but regular spreadsheet users will yawn...<br>
<ul><li>Header Row & Column Bars that (try) to stay put during scrolling.<br>
</li><li>Resizable Rows & Columns through mouse dragging.</li></ul></li></ul></li></ul>

<h2>More information</h2>
<ul><li><a href='http://code.google.com/p/trimpath/wiki/TrimSpreadsheetBasicPrimer'>A Basic Primer To Using TrimSpreadsheet</a>
</li><li><a href='http://code.google.com/p/trimpath/downloads/list'>TrimSpreadsheet Downloads</a>
</li><li><a href='http://code.google.com/p/trimpath/wiki/TrimSpreadsheetDiscussion'>TrimSpreadsheet Community Wiki</a>
</li><li><a href='http://code.google.com/p/trimpath/wiki/TrimSpreadsheetRunsIn'>TrimSpreadsheet Browser Compatibility</a></li></ul>

<hr />
{ <a href='http://code.google.com/p/trimpath/wiki/TrimSpreadsheet'>TrimSpreadsheet home</a> -- <a href='http://code.google.com/p/trimpath/downloads/list'>download</a> | <a href='http://code.google.com/p/trimpath/wiki/TrimSpreadsheetDiscussion'>community</a> }