# TrimSpreadsheet Browser Compatibility #
{ [TrimSpreadsheet home](http://code.google.com/p/trimpath/wiki/TrimSpreadsheet) -- [download](http://code.google.com/p/trimpath/downloads/list) | [community](http://code.google.com/p/trimpath/wiki/TrimSpreadsheetDiscussion) }

Does TrimSpreadsheet work everywhere?  Here's what we know...

| '''Environment''' | '''Runs?''' | '''Notes''' |
|:------------------|:------------|:------------|
| Windows — Mozilla Firefox 1.0 | yes         | my main browser - SteveYen |
| Windows — Internet Explorer 6.0 | yes         | SteveYen    |
| OS X.4 - Mozilla Firefox 1.5 | Yes         || Isaac Vetter|
| OS X.4 - Mozilla Camino .8 | Yes         || Isaac Vetter|
| Solaris 8 - Mozilla Firefox 1.0.7 | Yes         || Isaac Vetter|
| Solaris 8 - Netscape 7 | Yes         || Isaac Vetter|
| OS X — Safari 1.2 | No          | I've attached a screenshot - SimonWillison |
| Windows — Opera V8beta3 (build: 7522/WinXP) | No          | CSS issues & data corruption |


---

On OS X Safari, it looks like the CSS isn't getting picked up, and you're seeing the graceful degradation into an accessible HTML table.  Maybe.  Hey, now I have the perfect excuse to tell my wife why I simply just gotta get that PowerBook.  And, hold up the Mac mini as my plan B if I get shot down.  :-)  -- SteveYen

For Opera (V8.0 is soon to be formally released, but the demo is available from the home page), it seems the table is hidden beneath the mis-sized row and column headers (first screenshot). The original content is hidden; it flashes briefly before the javascript kicks in. What is worse though, is that the javascript rewrites the table content to NaN's and 0's in Opera, because even toggling CSS off does not degrade to the real table content (second screenshot). The CSS issues seem easily fixable, by removing the width attribute rather than just setting them to "". See this forum thread for more info: http://my.opera.com/forums/showthread.php?s=&postid=876256

'''Opera Update:''' Andrew Gregory on the Opera forum has [looked at why data corruption occurs](http://my.opera.com/forums/showthread.php?s=&postid=881254#post881254) in the spreadsheet, and has the following to say:
I finally got around to having a look at why the sheet was blanking out. It's pretty simple. Here's the problem: `TableCell.prototype.getFormula = function() { return this.getTd().getAttribute("formula"); };`
The code that calls that function assumes it returns null if there is no formula set. Opera follows the [W3C DOM](http://www.w3.org/TR/DOM-Level-2-Core/core.html#ID-666EE0F9) spec to the letter and returns an empty string ('') instead. Quick workaround: `TableCell.prototype.getFormula = function() { var ret = this.getTd().getAttribute("formula"); return (ret == '') ? null : ret; };`
However, I can see the same assumption is made in many other places..


---

{ [TrimSpreadsheet home](http://code.google.com/p/trimpath/wiki/TrimSpreadsheet) -- [download](http://code.google.com/p/trimpath/downloads/list) | [community](http://code.google.com/p/trimpath/wiki/TrimSpreadsheetDiscussion) }