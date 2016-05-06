# JavaScript Template Community Wiki/Discussion #
{ [JavaScript Templates (JST) home](http://code.google.com/p/trimpath/wiki/JavaScriptTemplates) | [API](http://code.google.com/p/trimpath/wiki/JavaScriptTemplateAPI) | [syntax](http://code.google.com/p/trimpath/wiki/JavaScriptTemplateSyntax) | [modifiers](http://code.google.com/p/trimpath/wiki/JavaScriptTemplateModifiers) | [download](http://code.google.com/p/trimpath/downloads/list) | [community](http://code.google.com/p/trimpath/wiki/JavaScriptTemplateDiscussion) }


---

'''UPDATE: Please see the [TrimPath Forum](http://trimpath.com/forum/index.php) for the latest place to discuss stuff.'''

We'll keep these community discussion wiki pages around, though, for reference.  -- SteveYen 2005/07/10


---


Feedback?  Suggestions?

There is a problem in the {elseif} control structure, the statment parses with a missing closing brace, in the template.js file we replaced the line defining the tag :

```
"elseif" : { delta:  0, prefix: "} else { if (", suffix: ") {", paramDefault: "true" },
```

with this :
```
"elseif" : { delta:  0, prefix: "} else if (", suffix: ") {", paramDefault: "true" }, 
```
and it works correctly now, i wonder why no one noticed this?

> -- Whoops.  I've incorporated the fix into release 1.0.34 of JST.  Thanks for the catch!  -- SteveYen


---


I found this site via someone's blog the other day and after looking over it, I really, really like the idea. I will be trying it out very soon as I'm looking into various javascript based rich-client like functionality which TrimPath could easily fit into. My main potential drawback, or better stated request, would be to support XML data in addition to tabular data (Your query engine looks pretty slick too BTW). In the system that I intend to build, we will be using XML data between the browser and server and I have been looking towards something like xmljs.sourceforge.net for implementing this in the browser. I'm assuming that if I supply a DOM object as the context to the template processor then I can navigate via the DOM API (nextChild, childNodes, etc.) but that is far from friendly compared to XPath. - Robert McIntosh robert@bull-enterprises.com

I'm glad you find the JST stuff interesting.  You should definitely be able to supply any object, including DOM objects, as ''entries'' in the context object.  Like context['invoiceDiv'] = document.getElementById('invoiceDiv');  I'm not sure (and doubt) that you can use the DOM object for the context object itself.  Once you have an XPath library, you should be able to use it by also stuffing it into the context.  Like context['xpath'] = SomeXPathUtil;  And, then in the JST do something like ${xpath(invoiceDiv, '//some/xpath/query')}  For moving data between browser and web-server, I'm more of a JSON fan myself than an XML proponent.  Good luck! - SteveYen


I'll be... That didn't even occur to me to put the XPath library into the context along with the dom object (to clarify I didn't mean to say use the DOM as the context, but as you said, put the DOM _into_ the context and use it as the 'context' data) and apply my xpaths to it that way.

I've taken a cursory look at JSON and it is yet another intriguing technique I plan on playing around with. My main reason at the moment of going for XML was that my persistence engine will soon support XUpdate changes on a DOM tree, so therefore I could do data updates in the browser and using an Ajax approach send the data to the server as XUpdates. Combine all that with client side templates and I have the potential for a really snappy UI. Gotta love it :-) -- Robert

I actually finally found some free time myself to do some larger AJAX app using JST, and am having a blast with the snappy UI thing too.  Would love to hear about your persistence engine/XUpdate solution.  Cheers -- SteveYen


---

fun for all the family :)

I'm having a big time hacking this stuff!

If someone else could confirm this (not quite sure, but here I go):

IE 6.0.28/Win2k - JST 1.0.16

when you define a macro like (note the empty line between /macro and the for)
```
{macro doTD(valueTD)}
  <TD>${valueTD}</TD>
{/macro}

{for p in rs}
$${doTD("")}
{/for}
```

poor little IE chokes to death (no macro, no problem).

My $0.2:

1) When emitting the {macro} element (line 87) the code is ...if (m) _OUT\_arr.push(m); }, };... and IE doesn't like the "extra" comma, so "(m)," -> "(m)". simple._

2) Not so simple, IE complains about some "unterminated string". I know that from this poor text is quite hard do visualize the problem, but...

Because of the empty line, the code translator emits a
```
"\n 
" 
```
or a "\n" literal with an intrisic <CR{LF}> that came in the end of the text-to-translated, and not a
```
"\n" 
```

So when is goes to the be eval-ed it chokes. the way I fixed it (not the best, but ...) is, starting in line 221: (original code)
```
        if (nlPrefix > 0) {
            funcText.push('if (_FLAGS.keepWhitespace == true) _OUT.write("');
            funcText.push(text.substring(0, nlPrefix).replace('\n', '\\n'));
            funcText.push('");');
        }

```

modified code
```
        if (nlPrefix > 0) {
            funcText.push('if (_FLAGS.keepWhitespace == true) _OUT.write("');
            var s = text.substring(0, nlPrefix).replace('\n', '\\n');
            if( s.charAt(s.length-1) == '\n'){
            	s = s.substring(0, s.length-1);
            }
            funcText.push(s);
            funcText.push('");');
        }
```

so it eats the spurious \n

-- BJessen

Wow, that's a great catch and explanation.  Thanks.  I've applied your fix to JST 1.0.20.  I spend most of my time in Firefox instead of IE nowadays, so I'm afraid IE bugs always seems to get shortchanged.

-- SteveYen


---

I made some investigation about problems with IE 5.0.

1. function cleanWhiteSpace. In IE 5.0 RegExp instances do not have the flags re.global, re.multiline.
Possibly we can live with less powerful regexp.
(line 308)
```
//        result = result.replace(/^(.*\S)[ \t]+$/gm, "$1"); // Right trim.
        result = result.replace(/^(\s*\S*(\s+\S+)*)\s*$/, '$1');
```

2. absence of Array.prototype.push/pop
It can be fixed by various technics. See http://www.crockford.com "Remedial Javascript" or
http://www.burstlib.org (search for fix\_ecma.js). So we fix it by adding somewhere before template.js
or in the beginning of template.js this code:
```
if (! Array.prototype.push) {
    Array.prototype.push = function () {
        for (var i = 0; i < arguments.length; ++i) {this[this.length] = arguments[i];}
        return this.length;
    };
}

if (! Array.prototype.pop) {
    var UNDEFINED;
    Array.prototype.pop = function () {
        if (this.length === 0) {return UNDEFINED;}
        return this[--this.length];
    };
}
```
After applying this fixes we have another problem :) but easy solvable.
The problem is modified behavior of arrays because of additional pop and push functions.
```
for (var k in exprArr)
```
now returns not only expression strings, but functions push and pop too.
So we need to fix code in function emitSectionTextLine
(line 283)
```
//            for (var k in exprArr) {
//                    exprArr[k] = exprArr[k].replace(/#@@#/g, '||');
//            }
            for (var k in exprArr) {
                if (exprArr[k].replace) {
                    exprArr[k] = exprArr[k].replace(/#@@#/g, '||');
                }
            }
```
Next we need to fix TrimPath.parseTemplate\_etc.statementDef "for" definition.
(line 78)
```
//                        return [ "var ", listVar, " = ", stmtParts[3], ";",
//                             "if ((", listVar, ") != null && (", listVar, ").length > 0) { for (var ",
//                             iterVar, "_index in ", listVar, ") { var ",
//                             iterVar, " = ", listVar, "[", iterVar, "_index];" ].join("");

                        return [ "var ", listVar, " = ", stmtParts[3], ";",
                             "if ((", listVar, ") != null && (", listVar, ").length > 0) { ",
                             "for (var ", iterVar, "_index in ", listVar, ") { ",
                             "if (typeof(", listVar, "[", iterVar, "_index]) == 'function') {continue;}",
                             "var ", iterVar, " = ", listVar, "[", iterVar, "_index];" ].join("");
```
to skip functions (push and pop).

After this modifications example page works in IE 5.01 and 5.5.

-- IgorPoteryaev

That's terrific!  Thank you much.  I've integrated your solutions and have put out a new version, 1.0.30, for JavaScript Templates.

-- SteveYen


---


Not sure if this should go here or on the TrimSpreadsheet page, but here goes:

A great extension for this language would be the easy syntax of commonly used "widgets" - maybe using using the macro keyword already implemented.  So, on top of this templating language you could also define "higher level" controls using it, and not re-implementing the wheel everytime.  As an example, I think maybe the spreadsheet "widget" is a bit higher level than a typical widget but, how about a common every-day need: The AutoComplete box.

so, (in my fanciful world) you could define a template thusly (or similarly):

```
   ${AutoCompleteBox(["waytoolongword", "anotherreallongword", "dontmakemetypethisout!"])}
```

and that would output a javascripted 

&lt;input type=text&gt;

 box that dropped down a menu or something on keypress - or using more arguments, other configurable behavior/shenanigans, or say you could pass in a function reference (instead of an array) to retrieve matches (so they could be GETed from the server)...

Off the top of my head (you know, i haven't thought about what i'd like too much) here's a few "widgets" i think would be really great and (mostly) feasible to implement, but a glance at any modern widget library also names (at least) all these:
  * Tabs/Tabbox
  * The TreeView control (with collapsible branches, etc)
  * Spellchecked Text Area (complete with visual error indication)
  * The Rich Text editing area (a bit like this wiki provides)
  * the ever-popular (and impossible) drop down menus
  * Numeric Input ("spinner" control) & Date input boxes
  * tri-state checkboxes/buttons?
  * Scrollable area
  * AutoComplete
  * Sortable tables/columned listboxes, or columned 

&lt;select&gt;

 boxes
  * More comprehensive (than the <img> tag) Image box (display only region of image, abstracted image  swapping, image scaling/zooming,<br>
<ul><li>As well as any other hare-brained lavish features that make geeks drool</li></ul>

Now, I realize this is a tall order, but it's certainly something to think about - What really is needed is a framework for developing these and letting the Community go wild on it.  Let's get a discussion going, i'm more than happy to participate in the development of such things, but I want to Do It Right.<br>
<br>
-- Thanks for the comments and ideas.  Would love to start a discussion about this, but my dad passed away and I will be pretty much out of touch for the duration.  But, I don't want to stop anyone from "going wild" with ideas.  Don't stop.  -- SteveYen<br>
<br>
<hr />
Where Shall I Put My JavaScript Templates?<br>
<br>
There is one more approach to placing JavaScript Templates.<br>
External scripts!<br>
Put link to external script to your (X)HTML like this<br>
<pre><code>    &lt;script type = "text/javascript" src = "templates/someFormTemplate.js"&gt;&lt;/script&gt;<br>
</code></pre>
where someFormTemplate.js looks like<br>
<pre><code>var cart_jst = '\<br>
    Hello ${customer.first} ${customer.last}.&lt;br/&gt;\<br>
    Your shopping cart has ${products.length} item(s):\<br>
    &lt;table&gt;\<br>
     &lt;tr&gt;&lt;td&gt;Name&lt;/td&gt;&lt;td&gt;Description&lt;/td&gt;\<br>
         &lt;td&gt;Price&lt;/td&gt;&lt;td&gt;Quantity &amp; Alert&lt;/td&gt;&lt;/tr&gt;\<br>
     {for p in products}\<br>
         &lt;tr&gt;&lt;td&gt;${p.name|capitalize}&lt;/td&gt;&lt;td&gt;${p.desc}&lt;/td&gt;\<br>
             &lt;td&gt;$${p.price}&lt;/td&gt;&lt;td&gt;${p.quantity} : ${p.alert|default:""|capitalize}&lt;/td&gt;\<br>
             &lt;/tr&gt;\<br>
     {forelse}\<br>
         &lt;tr&gt;&lt;td colspan="4"&gt;No products in your cart.&lt;/tr&gt;\<br>
     {/for}\<br>
    &lt;/table&gt;\<br>
    {if customer.level == "gold"}\<br>
      We love you!  Please check out our Gold Customer specials!\<br>
    {else}\<br>
      Become a Gold Customer by buying more stuff here.\<br>
    {/if}\<br>
';<br>
</code></pre>
You only should be disciplined enough to user single and double quote marks consistently.<br>
<br>
Now in your code simply use variable used in above script.<br>
<pre><code>var myTemplateObj = TrimPath.parseTemplate(cart_jst);<br>
</code></pre>

The visible benefits of this method are:<br>
<ul><li>reducing size of (X)HTML pages<br>
</li><li>caching of templates on client side<br>
</li><li>using of javascript files for placing templates looks like more adequate than using hidden textarea, IMHO.</li></ul>

-- IgorPoteryaev<br>
<br>
Igor: I like your concept of modularizing the templates, and putting them in js files does give us all the benefits you listed (all of them important: reduction in size, caching!, textarea hack unprofessionalism) - but i'd like to find a way to do it without having to litter my templates with escape characters and burden myself with the quoting hassles (sometimes i want to use BOTH types of quotes), this is a step backwards - akin to using basic js functions to output html, (a big part of the reason) we use JST (is) b/c we don't want to deal with the problems this approach brings.<br>
<br>
With that said, I have no great solution to that.  One approach I'm pondering is perhaps having a file extension to indicate a particular file contains ONLY JST code (perhaps ".jst" ..?) and set up an Apache handler to rewrite it slightly, escaping pertinent characters (newlines/quotes, etc) and "assigning" the escaped template code to a variable with the same name as the file.<br>
<br>
So an example:<br>
<ul><li>I define a file on my server, mytemplate.jst, it contains:<br>
<pre><code>Hello ${customer.first} ${customer.last}.&lt;br/&gt;<br>
Your shopping cart has ${products.length} item(s)<br>
yadda yadda, more (simple and "pure"!) jst code...<br>
</code></pre>
</li><li>when this file gets served, apache (or something) rewrites it to be:<br>
<pre><code>var mytemplate = "Hello ${customer.first} ${customer.last}.&lt;br/&gt;\<br>
Your shopping cart has ${products.length} item(s)\<br>
yadda yadda, more (simple and \"pure\"!) jst code...";<br>
</code></pre>
</li><li>reference this template in an (x)html file like so:<br>
<pre><code>&lt;script src="mytemplate.jst"&gt;&lt;/script&gt;<br>
&lt;script&gt;var myTemplateObj = TrimPath.parseTemplate(mytemplate);&lt;/script&gt;<br>
</code></pre></li></ul>

this i think brings the best of both worlds together, performance & maintainability, at the cost of making the server work a bit harder and having to customize it a bit more.  what do you think?<br>
<br>
-- BrianBittman<br>
<br>
Brian,<br>
Your approach can be implemented using, for example, this script (or similar one) on deployment stage.<br>
<br>
<pre><code># ! / usr/ bin/ python<br>
<br>
from os import linesep<br>
js_linesep = '\\' + linesep<br>
<br>
def process(fileName):<br>
    """ <br>
        Converts Javascript Template text file (assumed .jst file extension) to<br>
        javascript file directly loadable by using &lt;script src = "mytemplate.js"&gt;&lt;/script&gt; syntax.<br>
<br>
        Currently this script do just this replacements in source file:<br>
        1 - escape all double quote symbols (") with backslash symbol (\)<br>
        2 - add javascript line continuation symbol (\) to all lines<br>
    """<br>
    print 'Processing %s.jst ...' % fileName<br>
    lines = open(fileName + '.jst').read().replace('"', '\\"').split(linesep)<br>
    open(fileName + '.js', 'w').write('var %s = "%s";' % (fileName, js_linesep.join(lines)))<br>
<br>
if __name__ == '__main__':<br>
    import sys, os<br>
    process(os.path.basename(sys.argv[1])[:-4])<br>
</code></pre>

IMHO, it is better to preprocess source javascript files to remove extra stuff (comments, whitespaces ...).<br>
We, for example, use Douglas Crockford' JavaScript Minifier (<a href='http://www.crockford.com/javascript/jsmin.html'>http://www.crockford.com/javascript/jsmin.html</a>) in<br>
our deployment script.<br>
<br>
HTH.<br>
<br>
-- IgorPoteryaev<br>
<br>
<hr />
Thank you Igor!<br>
And for the perl inclined among us, here's how I did it:<br>
<pre><code>    my $filename = 'some/path/to/yourfile.jst';  #the hypothetical<br>
<br>
    #preprocess the template<br>
    my $varname = $filename;<br>
    $varname = (split /\//, $varname)[-1];  #give me the file, no path info (MAKE A NOTE OF THIS! don't stomp on your template variable names)<br>
    $varname =~ s/[^a-zA-Z0-9_]/_/g;  #make a legit variable name (MAKE NOTE OF THIS)<br>
    <br>
    open JS, "&lt;$filename";<br>
    my @js_lines = map { <br>
        chomp;<br>
        s/"/\\"/g;      #replace all " chars =&gt; \"<br>
        $_;<br>
    } &lt;JS&gt;;<br>
    close JS;<br>
    <br>
    print "Content-Type: application/x-javascript\n\n"; <br>
    print qq~var $varname = "~. join(qq~ \\\n~, @js_lines) . qq~";~;<br>
</code></pre>
and you can pick up your template in the variable named yourfile_jst (or whatever extension it may have had)<br>
-BrianBittman<br>
<br>
<hr />

Not sure if this is even a good idea but let's see what the people think, I like it so far...<br>
<br>
As I was authoring templates it dawned on me (or should I say, was forced upon me) that you cannot write any javascript inside the template and have it exist in the document - the reason for this is that unless the template output is written with a document.write() -- as is often the case -- call it won't get added to the document "script library".  To compensate for this I decided that I needed another way to include script in the template, so I added 2 more tags to the TPT tagset: "eval" and "minify".  (Minify was inspired by Crockfords' JSMin code, but does not do nearly as much as JSMin does, though it ought to - anybody..?)<br>
<br>
I butchered the {cdata} parsing block in the template.js file to make it work for more than just cdata, so eval & minify work just like cdata did (don't eval any "{" characters, and look for an customized ending block) but they do something different with the "inner block".  So if cdata takes the entire text inside it's beginning and ending block and outputs it verbatim, minify just trims and collapes all whitespace before printing it (and it should remove all comments too! not yet..) and eval, takes the inside and well, calls eval() on it, throwing an error if it fails.<br>
<br>
A tangent:<br>
While I was at it, the code that handled cdata tags is now much more scalebable to do more interesting things, and also for any of these "block tags" (ie, cdata, minify and eval, for now) you can omit the "marker" and it will search instead for {/block_name} - an example helps here.  For any block tag you can:<br>
<pre><code>{BLOCK}your text here{/BLOCK}<br>
</code></pre>
where "BLOCK" is replaced by any valid block tag (currently: cdata, minify or eval) - of course the old way of doing<br>
<pre><code>{BLOCK EOFMARKER}your text hereEOFMARKER<br>
</code></pre>
still works.<br>
<br>
<br>
How are these useful?  ok, minify might be used thusly:<br>
<pre><code>&lt;select onchange="{minify EOFMIN}<br>
     ...Do some complicated javascript...;<br>
     ...more js code here...;<br>
     this.enabled = false;<br>
EOFMIN"&gt;<br>
</code></pre>
basically allowing you to inline long functions into your elements, you could do this with html before too, but without newlines in your js code it becomes pretty unweildy...<br>
<br>
using the eval, why ever would you want this?  well i'll show you:<br>
<pre><code>&lt;select onchange="sel_onchange()"&gt;&lt;/select&gt;<br>
{eval} <br>
    sel_onchange = function() {<br>
     ...Do some complicated javascript...;<br>
     ...more js code here...;<br>
    }<br>
{/eval}<br>
</code></pre>
and now after this template is processed (ahem, each time this template is processed) a variable named sel_onchange is populated with that function - effectively allowing you to define functions in your template, which (unless you were outputting the templates with document.write) you could not do before.  a couple of caveats however, notice that i a) did not say "var sel_onchange" b/c that would then scope the variable inside the eval and it would never be accessible and b) did not say "function sel_onchange()" for the same reasons.  in mozilla anyway, you don't need to predeclare sel_onchange anywhere, ie may be different - can somebody let me know?<br>
<br>
so a couple of bad ideas... at the very least i've rewritten the cdata parsing block to be more expandable and allowed for the automatic {/cdata} block end markers.  umm, i guess i'll attach my template.js  file to this page at the bottom<br>
<br>
--BrianBittman<br>
<br>
This is actually a pretty <b>good</b> idea.  I've incorporated the cdata/eval/minify features into release 1.0.34 and also the loop indexVar_ct variable.  -- SteveYen<br>
<br>
Thanks Steve!  I'm appreciate your enthusiastic response. but i been playing with it, the eval is a bit more awkward than i had imagined, i think i just need to break my brain into using it. but besides there are 2 things i want to put forward as being required for the {eval} feature:<br>
<br>
1) this is so obvious, it's ridiculous this is an afterthought: a return value from an {eval} block should act like it's cdata, just put it into the output stream...<br>
<br>
2) and, i admit, i knew my "eval" processing was a kludge that had a fatal flaw, but what, i couldn't finger, then i got it: it does the js eval() call WHILE IT'S PARSING, so things don't work quite right under some situations.  what should happen is the code should just get inlined into the function being "written by the template" - so it would get eval'd as the output was written, not during parse time, where any variables IN the template that might change, haven't changed at all yet.  i think the solution would look something like:<br>
<pre><code>...snip: template.js, 1.0.34, line 192...<br>
} else if (blockType == 'eval') {<br>
    emitEval(blockText, funcText);                                <br>
}<br>
...end snip...<br>
</code></pre>
and, of course, we need to define that emitEval function later on (i had posted a poor implentation of this earlier, but now it's fixed), this is what the emitEval should look like:<br>
<pre><code>    var emitEval = function(text, funcText) {<br>
        <br>
        if (text == null ||<br>
            text.length &lt;= 0)<br>
            return;<br>
        <br>
        funcText.push(<br>
            '_OUT.write( (function() { ' + text + ' })() );'<br>
        );<br>
    }<br>
<br>
</code></pre>
nice and clean, in'it?  (SteveYen -- also note that i "discovered" the testing framework prior to posting this ;) i'll do that from now on...)<br>
<br>
so, this eval implementation should provide a solution to both of those problems i mentioned above.<br>
<br>
- if you use a "return" statement in your eval, that string will be printed (yay!), likewise, with no "return" statement, nothing is printed<br>
<br>
- "eval" code is executed as the output is written, not as the template is parsed<br>
<br>
and just for kicks, if you want to have fun with this, the first one's free:<br>
<pre><code>&lt;textarea id="myjst_jst" style="display:none"&gt;<br>
    see -- <br>
    {eval}<br>
       return something("yep.")<br>
    {/eval}<br>
&lt;/textarea&gt;<br>
&lt;script&gt;<br>
    alert(<br>
        TrimPath.parseDOMTemplate('myjst_jst').process({<br>
           'something': function(i) { return " you passed in: " + i; }<br>
        })<br>
    );<br>
&lt;/script&gt;<br>
</code></pre>

Oh and mailing list (mentioned below, but earlier), i second the motion.<br>
<br>
--BrianBittman<br>
<br>
Right-o.  Release 1.0.36 of JST has the {eval} block fix.  Note that I didn't use a separate emitEval() helper function, but your description really helped.<br>
<br>
-- SteveYen<br>
<br>
<hr />
You guys might need a mailing list soon... I got quick question. I am wanting to combine the TrimQuery with JST. I have the query part working like I want it, but I have a problem with the template. I invoke the template with a "processDOMTemplate("template", data)" where 'data' is the variable of the query result. My problem is I don't know how to access this result(s) in a for loop. The examples given assume a string with a named object of the results, such as Customer : <a href='.md'>id:1 </a>,etc (syntax may be incorrect but you get the idea). Well, the result from the query doesn't provide this 'object' as far as I know, so I can't access it with (for p in players).<br>
<br>
Can I use the resulting array from the query as the data or must the data be a string that then gets evaled? In case it helps, my use case is that I query up a table and build a JSON array in PHP. This then gets evaled and I use it as a table for then doing paging, without having to go back to the server. If I 'manually' do my template, via building up HTML, it works just fine, but I thought I would throw in the JST for the sake of trying it.<br>
<br>
Thanks for any help,<br>
-- Robert McIntosh<br>
-- robert.mcintosh@uug.com<br>
<br>
How about an extra level of indirection when you pass the data into the template engine?<br>
<pre><code>  var data = /* anArrayOfResults */<br>
  var output = TrimPath.processDOMTemplate("template", { data : data });<br>
<br>
  // which is the same as...<br>
  var data = /* anArrayOfResults */<br>
  var temp = new Object();<br>
  temp.data = data;<br>
  var output = TrimPath.processDOMTemplate("template", temp);<br>
</code></pre>
Then in your template the {for record in data} should work?  By the way, your use case sounds cool. -- SteveYen<br>
<br>
Ok, call me an amateur :-) I figured it was something like that but I just couldn't figure it out. I'll blame it on being a friday afternoon... That worked perfectly, thanks Steve. As soon as I get it into prod I'll send a link just in case you guys end up having a 'who's using it' page or something. Thanks again, Robert<br>
<br>
Here's a users page: TrimPathUsers<br>
<br>
<hr />
On the subject of 'where to put it', I had thought about having the template fragments on the server, and loading them via xmlHttpRequest. The idea is to load each template as you need it then cache it in the browser where you can then put it dynamically in a hidden text area. If you have a large number of templates, this would allow the page to load quickly then retrieve the templates as needed. Of course you could preload some and cache others or whatever you needed. By the same thought, you could load and parse then cache as well.<br>
<br>
- RobertMcIntosh<br>
<br>
<hr />
'''UPDATE: Please see the <a href='http://trimpath.com/forum/index.php'>TrimPath Forum</a> for the latest place to discuss stuff.'''<br>
<br>
We'll keep these community discussion wiki pages around, though, for reference.  -- SteveYen 2005/07/10<br>
<br>
<hr />
{ <a href='http://code.google.com/p/trimpath/wiki/JavaScriptTemplates'>JavaScript Templates (JST) home</a> | <a href='http://code.google.com/p/trimpath/wiki/JavaScriptTemplateAPI'>API</a> | <a href='http://code.google.com/p/trimpath/wiki/JavaScriptTemplateSyntax'>syntax</a> | <a href='http://code.google.com/p/trimpath/wiki/JavaScriptTemplateModifiers'>modifiers</a> | <a href='http://code.google.com/p/trimpath/downloads/list'>download</a> | <a href='http://code.google.com/p/trimpath/wiki/JavaScriptTemplateDiscussion'>community</a> }