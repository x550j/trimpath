# TrimSpreadsheet Basic Primer #
{ [TrimSpreadsheet home](http://code.google.com/p/trimpath/wiki/TrimSpreadsheet) -- [download](http://code.google.com/p/trimpath/downloads/list) | [community](http://code.google.com/p/trimpath/wiki/TrimSpreadsheetDiscussion) }

''Major acknowledgements to Eric Meyer, whose terrific [S5 web presentation software](http://www.meyerweb.com/eric/tools/s5/) inspired the TrimSpreadsheet project, and whose ideas and content were liberally and lazily cribbed.  Thanks for the genesis! -- SteveYen''

For a newcomer, creating a spreadsheet in TrimSpreadsheet may seem a bit daunting at first. Don't panic! Writing your own spreadsheet is very, very easy. In order to help smooth the way, here's a short primer on what to change and what to leave alone in an TrimSpreadsheet spreadsheet file.

## First Steps ##

First, [download](http://code.google.com/p/trimpath/downloads/list) the TrimSpreadsheet spreadsheet archive. This archive that contains everything you'll need to create a spreadsheet: the spreadsheet file itself, the style sheets that make it look pretty, and the JavaScript that drives the actual spreadsheet interface and formula recalculations.

Once you've downloaded the archive and uncompressed it, amongst the new files you should find a file named spreadsheet\_blank.html. All the other files are more sample files or are CSS and JavaScript bits that make the spreadsheet work. We're not going to mess with those.

For those of you who haven't downloaded, but want to see the spreadsheet in action anyways, here's an [online version](http://trimpath.com/demos/test1/trimpath/spreadsheet_blank.html).

Now, we're going to edit the file spreadsheet\_blank.html. Go ahead and load it up into your favorite editor, whether that's Dreamweaver, Notepad, emacs, TextMate or BBEdit.

## Straight To The Head ##

The first part of the spreadsheet\_blank.html file, at least the part after the DOCTYPE and the 

&lt;html&gt;

 tag, is the head element, which looks like...
```
<head>
    <title>[spreadsheet title]</title>
    <script language="javascript" src="spreadsheet.js"></script>
    <link rel="stylesheet" type="text/css" href="spreadsheet.css"/>
    <link rel="stylesheet" type="text/css" href="spreadsheet_grey.css"/>
</head>
```

For the most part, you shouldn't have to ever touch this part of the file. In fact, it's better to avoid it. If you changed any of the link elements, for example, you might break the slide show! So you'll want to just skip past it... except for one thing. See the title element? Change its contents from "[title](spreadsheet.md)" to the title of the spreadsheet. So if you're doing a spreadsheet on some product comparison, you might call the presentation "Price Research". Go ahead and change the title contents to say that...
```
<head>
    <title>Price Research: Bang For The Buck</title>
    <script language="javascript" src="spreadsheet.js"></script>
    <link rel="stylesheet" type="text/css" href="spreadsheet.css"/>
    <link rel="stylesheet" type="text/css" href="spreadsheet_grey.css"/>
</head>
```

Other than that, you don't need to make any changes to this part of the file. Let's move on to the next part.

## Spreadsheet Decorations ##

If you move downward in the spreadsheet\_blank.html file, you'll find several <div> tags.  These control the decorations around the spreadsheet, such as whether we want editing controls (a place to type formulas), whether we want a scrolling window, and and whether we want header bars along the top and left of the spreadsheet.<br>
<pre><code> &lt;div class="spreadsheetEditor"&gt;<br>
  &lt;div class="spreadsheetScroll" style="width:500px; height:200px;"&gt;<br>
   &lt;div class="spreadsheetBars"&gt;<br>
   ...<br>
   &lt;/div&gt;<br>
  &lt;/div&gt;<br>
 &lt;/div&gt;<br>
</code></pre>
At the bottom of the file, we see that the <div> tags are closed off with matching </div> close tags.<br>
<br>
In the <div>, the width and height style attributes define the viewing portal for the spreadsheet.  For example, if our full spreadsheet ends up being larger than 500 pixels wide or 200 pixels tall, the viewing portal (which is only 500 pixels wide and 200 pixels tall) will automatically get scrollbars.<br>
<br>
If you don't want all or some of these decorations, just remove the respective <div> tag and its matching close </div> tag.<br>
<br>
<h2>The Table</h2>

Now, for the table...<br>
<pre><code>      &lt;table class="spreadsheet" width="700" border="1"&gt;<br>
        &lt;col width="100"&gt;<br>
        &lt;col width="100"&gt;<br>
        &lt;col width="100"&gt;<br>
        &lt;col width="100"&gt;<br>
        &lt;col width="100"&gt;<br>
        &lt;col width="100"&gt;<br>
        &lt;col width="100"&gt;<br>
        &lt;tr&gt;&lt;td&gt;&lt;/td&gt;&lt;td&gt;&lt;/td&gt;&lt;td&gt;&lt;/td&gt;&lt;td&gt;&lt;/td&gt;&lt;td&gt;&lt;/td&gt;&lt;td&gt;&lt;/td&gt;&lt;td&gt;&lt;/td&gt;<br>
            &lt;/tr&gt;<br>
        &lt;tr&gt;&lt;td&gt;&lt;/td&gt;&lt;td&gt;&lt;/td&gt;&lt;td&gt;&lt;/td&gt;&lt;td&gt;&lt;/td&gt;&lt;td&gt;&lt;/td&gt;&lt;td&gt;&lt;/td&gt;&lt;td&gt;&lt;/td&gt;<br>
            &lt;/tr&gt;<br>
        &lt;tr&gt;&lt;td&gt;&lt;/td&gt;&lt;td&gt;&lt;/td&gt;&lt;td&gt;&lt;/td&gt;&lt;td&gt;&lt;/td&gt;&lt;td&gt;&lt;/td&gt;&lt;td&gt;&lt;/td&gt;&lt;td&gt;&lt;/td&gt;<br>
            &lt;/tr&gt;<br>
        ...<br>
      &lt;/table&gt;<br>
</code></pre>

The table is just a regular HTML <table>, except it has a class of "spreadsheet".  Also, the total width of 700 is explicitly declared and the column widths are explicitly declared (with the <br>
<br>
<COL><br>
<br>
 tags).  These width declarations are needed by the TrimSpreadsheet engine.<br>
<br>
(Aside for skilled web developers: if you like XHTML, yes, you can add THEAD, TBODY, TFOOT, COLGROUP tags, too.  But, as yet, the TrimSpreadsheet engine doesn't handle rowspan/colspan and nested spreadsheets.)<br>
<br>
In the TrimSpreadsheet user interface, you can use mousing drag-&-drop to resize your column widths and row heights.  To do this, in the header bars, just use your mouse to click and drag on the little lines that separate column and row labels.<br>
<br>
You can now put data into the <td></td> tags.  This data can be numbers, text, or even more HTML.<br>
<br>
And, you can also put formulas into attributes, like <td>.<br>
<br>
A prebuilt example would help show this better.  In the the downloaded archive, another file, spreadsheet_demo0.html, contains a table with some filled in information.  Here's a part of that file..<br>
<pre><code>      &lt;table ...&gt;<br>
        ...<br>
        &lt;tr&gt;&lt;td style="font-weight:bold;"&gt;Component Name&lt;/td&gt;<br>
            &lt;td style="font-weight:bold;"&gt;Description&lt;/td&gt;<br>
            &lt;td style="font-weight:bold;"&gt;LOC&lt;/td&gt;<br>
            &lt;td style="font-weight:bold;"&gt;Link&lt;/td&gt;<br>
            &lt;td&gt;&lt;/td&gt;&lt;td&gt;&lt;/td&gt;&lt;td&gt;&lt;/td&gt;<br>
            &lt;/tr&gt;<br>
        &lt;tr&gt;&lt;td&gt;&lt;a href="http://trimpath.com/project/wiki/JavaScriptTemplates"&gt;JavaScript Templates&lt;/a&gt;&lt;/td&gt;<br>
            &lt;td&gt;A template engine (like PHP/ASP/JSP).&lt;/td&gt;<br>
            &lt;td&gt;300&lt;/td&gt;<br>
            &lt;td&gt;&lt;a href="http://trimpath.com/project/wiki/JavaScriptTemplates"&gt;http://trimpath.com/project/wiki/JavaScriptTemplates&lt;/a&gt;&lt;/td&gt;<br>
            &lt;td&gt;&lt;/td&gt;&lt;td&gt;&lt;/td&gt;&lt;td&gt;&lt;/td&gt;<br>
            &lt;/tr&gt;<br>
        &lt;tr&gt;&lt;td&gt;&lt;a href="http://trimpath.com/project/wiki/TrimQuery"&gt;TrimQuery&lt;/a&gt;&lt;/td&gt;<br>
            &lt;td&gt;A feature-rich, SQL-like query engine.&lt;/td&gt;<br>
            &lt;td&gt;600&lt;/td&gt;<br>
            &lt;td&gt;&lt;a href="http://trimpath.com/project/wiki/TrimQuery"&gt;http://trimpath.com/project/wiki/TrimQuery&lt;/a&gt;&lt;/td&gt;<br>
            &lt;td&gt;&lt;/td&gt;&lt;td&gt;&lt;/td&gt;&lt;td&gt;&lt;/td&gt;<br>
            &lt;/tr&gt;<br>
        &lt;tr&gt;&lt;td&gt;&lt;a href="http://trimpath.com/project/wiki/TrimSpreadsheet"&gt;TrimSpreadsheet&lt;/a&gt; Engine&lt;/td&gt;<br>
            &lt;td&gt;A spreadsheet recalculation engine.&lt;/td&gt;<br>
            &lt;td&gt;300&lt;/td&gt;<br>
            &lt;td&gt;&lt;a href="http://trimpath.com/project/wiki/TrimSpreadsheet"&gt;http://trimpath.com/project/wiki/TrimSpreadsheet&lt;/a&gt;&lt;/td&gt;<br>
            &lt;td&gt;&lt;/td&gt;&lt;td&gt;&lt;/td&gt;&lt;td&gt;&lt;/td&gt;<br>
            &lt;/tr&gt;<br>
        &lt;tr&gt;&lt;td&gt;&lt;a href="http://trimpath.com/project/wiki/TrimSpreadsheet"&gt;TrimSpreadsheet&lt;/a&gt; UI&lt;/td&gt;<br>
            &lt;td&gt;A spreadsheet user interface.&lt;/td&gt;<br>
            &lt;td&gt;700&lt;/td&gt;<br>
            &lt;td&gt;&lt;a href="http://trimpath.com/project/wiki/TrimSpreadsheet"&gt;http://trimpath.com/project/wiki/TrimSpreadsheet&lt;/a&gt;&lt;/td&gt;<br>
            &lt;td&gt;&lt;/td&gt;&lt;td&gt;&lt;/td&gt;&lt;td&gt;&lt;/td&gt;<br>
            &lt;/tr&gt;<br>
        &lt;tr&gt;&lt;td&gt;&lt;/td&gt;&lt;td&gt;&lt;/td&gt;&lt;td&gt;&lt;/td&gt;&lt;td&gt;&lt;/td&gt;&lt;td&gt;&lt;/td&gt;&lt;td&gt;&lt;/td&gt;&lt;td&gt;&lt;/td&gt;<br>
            &lt;/tr&gt;<br>
        &lt;tr&gt;&lt;td&gt;&lt;/td&gt;<br>
            &lt;td style="text-align:right;"&gt;Total lines of JavaScript code:&lt;/td&gt;<br>
            &lt;td formula="=SUM(C2:C5)"&gt;&lt;/td&gt;<br>
            &lt;td&gt;&lt;/td&gt;<br>
            &lt;td&gt;&lt;/td&gt;&lt;td&gt;&lt;/td&gt;&lt;td&gt;&lt;/td&gt;<br>
            &lt;/tr&gt;<br>
        &lt;tr&gt;&lt;td&gt;&lt;/td&gt;<br>
            &lt;td style="text-align:right;"&gt;Average lines of JavaScript code:&lt;/td&gt;<br>
            &lt;td formula="=AVERAGE(C2:C5)"&gt;&lt;/td&gt;<br>
            &lt;td&gt;&lt;/td&gt;<br>
            &lt;td&gt;&lt;/td&gt;&lt;td&gt;&lt;/td&gt;&lt;td&gt;&lt;/td&gt;<br>
            &lt;/tr&gt;<br>
        ...<br>
      &lt;/table&gt;<br>
</code></pre>
Again, for those who have not downloaded the archive, you can also view the <a href='http://trimpath.com/demos/test1/trimpath/spreadsheet_demo0.html'>spreadsheet_demo0.html online</a>.<br>
<br>
As you can see, some cells have '''formula''' attributes, such as formula="=AVERAGE(C2:C5)".  These formulas are recalculated by the spreadsheet engine automatically on startup and whenever data changes (when you use the formula edit controls).<br>
<br>
Other cells having '''style''' attributes, such as style="font-weight:bold;".  You can use regular old CSS style syntax here.<br>
<br>
In the TrimSpreadsheet user interface, you can use the buttons near the formula bar to toggle the bold, italic, underline, and left/right/centered style attributes of your cells.<br>
<br>
And, some table data cells have HTML text, such as the following which has a link and some text...<br>
<pre><code>  &lt;td&gt;&lt;a href="http://trimpath.com/project/wiki/TrimSpreadsheet"&gt;TrimSpreadsheet&lt;/a&gt; Engine&lt;/td&gt;<br>
</code></pre>

So, replace the formulas with your own, add more formula attributes, add more rows and columns, replace the data (inside the <td></td> pairs) with your own.  There's a world of numbers out there to slice and dice.<br>
<br>
<h2>In Summary</h2>

As you've seen in this quick primer, creating your own spreadsheet is pretty simple. In the majority of cases, all you need is a a few <div>'s and a <table> with explicitly defined column widths.<br>
<br>
Hopefully this primer has helped you get started creating your own presentation. If there's anything missing or unclear, please let me know.<br>
<br>
<hr />
{ <a href='http://code.google.com/p/trimpath/wiki/TrimSpreadsheet'>TrimSpreadsheet home</a> -- <a href='http://code.google.com/p/trimpath/downloads/list'>download</a> | <a href='http://code.google.com/p/trimpath/wiki/TrimSpreadsheetDiscussion'>community</a> }