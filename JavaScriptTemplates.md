# JavaScript Templates #
{ [JavaScript Templates (JST) home](http://code.google.com/p/trimpath/wiki/JavaScriptTemplates) | [API](http://code.google.com/p/trimpath/wiki/JavaScriptTemplateAPI) | [syntax](http://code.google.com/p/trimpath/wiki/JavaScriptTemplateSyntax) | [modifiers](http://code.google.com/p/trimpath/wiki/JavaScriptTemplateModifiers) | [download](http://code.google.com/p/trimpath/downloads/list) | [community](http://code.google.com/p/trimpath/wiki/JavaScriptTemplateDiscussion) }

For web application developers, the '''JavaScript Templates''' engine from TrimPath is a lightweight [APL / GPL open-source](http://code.google.com/p/trimpath/wiki/TrimPathLicense) component that lets you have template-based programming (like PHP/ASP/JSP) while running in a web browser.
  * The '''JST''' engine is written entirely in standard JavaScript.
  * It supports a productive template markup syntax very much like [FreeMarker](http://freemarker.sourceforge.net), [Velocity](http://jakarta.apache.org/velocity), [Smarty](http://smarty.php.net).
  * JST is a readable alternative to manually coded massive String concatenations and major DOM/DHTML manipulations.

Build your own modern, brilliantly rich web applications (like GMail/OddPost/Bloglines) with JavaScript Templates.

More information:
  * '''10 Minute Introduction''' -- see below.
  * [JST API](http://code.google.com/p/trimpath/wiki/JavaScriptTemplateAPI)
  * [JST Markup Syntax](http://code.google.com/p/trimpath/wiki/JavaScriptTemplateSyntax)
  * [JST Standard Modifiers](http://code.google.com/p/trimpath/wiki/JavaScriptTemplateModifiers)
  * [JST Downloads](http://code.google.com/p/trimpath/downloads/list)
  * [JST Community Wiki](http://code.google.com/p/trimpath/wiki/JavaScriptTemplateDiscussion)
  * [JST Browser Compatibility](http://code.google.com/p/trimpath/wiki/JavaScriptTemplateRunsIn)
  * [JST Online Demo](http://trimpath.com/demos/test1/trimpath/template_demo.html)

NEWS:
  * 2005/07/18 - New release - 1.0.38
    * Fix to a IE macro bug and made error messages slightly better in IE.
  * 2005/05/28 - New release - 1.0.36
    * B. Bittman (BrianBittman) found and solved a bug with the new eval block feature.
  * 2005/05/19 - New release - 1.0.34
    * B. Bittman provided new cdata/eval/minify features and a loop indexVar\_ct variable
    * An anonymous donor caught and provided the solution for an elseif bug.
  * 2005/05/11 - New release - 1.0.32.
    * Ross Shaull found and provided the solution for for/forelse looping over objects.  So for/forelse now works on objects (hashes), not just arrays.
  * 2005/04/07 - New release - 1.0.30.
    * Igor Poteryaev found many issues with running IE 5.0.1/5.5, and kindly provided solutions, which were integrated into this release 1.0.30.
    * Also, 1.0.30 supports a new "cdata" markup syntax, which tells the JST engine to skip any processing for a section of text.  See the [JST Markup Syntax](http://code.google.com/p/trimpath/wiki/JavaScriptTemplateSyntax) page for more info about '{cdata EOF} ... EOF' markup
  * 2005/03/25 - New release - 1.0.26.
    * Since 1.0.16, several bugs have been found and solved by Don Brown (of [Apache Struts Flow](http://struts.apache.org/flow/)) and BJessen.
    * Also, added a defined() function and optional ${% expr %} syntax.
  * 2005/03/01 - New release - 1.0.16, which has the Apache copyright boilerplate and a minor bug fix around OR expression parsing.
  * 2005/02/28 - Looks like this page has been del.icio.us'ed / popular'ed.  Cool.  Happy Templating, everyone!  -- SteveYen

## 10 Minute Introduction To JST ##
First, in our HTML page, we load the TrimPath JST component into our web browser...
```
  <html>
    <head>
      <script language="javascript" src="trimpath/template.js"></script>
      ...
    </head>
    ...
  </html>
```
Next, create some structured JavaScript data -- just some Objects and Arrays...
```
  <script language="javascript">
    var data = {
        products : [ { name: "mac", desc: "computer",     
                       price: 1000, quantity: 100, alert:null },
                     { name: "ipod", desc: "music player", 
                       price:  200, quantity: 200, alert:"on sale now!" },
                     { name: "cinema display", desc: "screen",       
                       price:  800, quantity: 300, alert:"best deal!" } ],
        customer : { first: "John", last: "Public", level: "gold" }
    };
  </script>
```
Next, here is a sample JST template that could 'render' that data.  We've put our JST template into a hidden 

&lt;textarea&gt;

 in our HTML page...
```
  <textarea id="cart_jst" style="display:none;">
    Hello ${customer.first} ${customer.last}.<br/>
    Your shopping cart has ${products.length} item(s):
    <table>
     <tr><td>Name</td><td>Description</td>
         <td>Price</td><td>Quantity & Alert</td></tr>
     {for p in products}
         <tr><td>${p.name|capitalize}</td><td>${p.desc}</td>
             <td>$${p.price}</td><td>${p.quantity} : ${p.alert|default:""|capitalize}</td>
             </tr>
     {forelse}
         <tr><td colspan="4">No products in your cart.</tr>
     {/for}
    </table>
    {if customer.level == "gold"}
      We love you!  Please check out our Gold Customer specials!
    {else}
      Become a Gold Customer by buying more stuff here.
    {/if}
  </textarea>
```
Here's some code that shows ways to do template processing with the [API](http://code.google.com/p/trimpath/wiki/JavaScriptTemplateAPI)...
```
  <script language="javascript">
    // The one line processing call...
    var result = TrimPath.processDOMTemplate("cart_jst", data);
    // Voila!  That's it -- the result variable now holds 
    // the output of our first rendered JST.

    // Alternatively, you may also explicitly parse the template...
    var myTemplateObj = TrimPath.parseDOMTemplate("cart_jst");

    // Now, calls to myTemplateObj.process() won't have parsing costs...
    var result  = myTemplateObj.process(data);
    var result2 = myTemplateObj.process(differentData);

    // Setting an innerHTML with the result is a common last step...
    someOutputDiv.innerHTML = result;
    // You might also do a document.write() or something similar...
  </script>
```
And the value of the result variable would be...
```
    Hello John Public.<br/>
    Your shopping cart has 3 item(s):
    <table>
     <tr><td>Name</td><td>Description</td>
         <td>Price</td><td>Quantity & Alert</td></tr>
         <tr><td>MAC</td><td>computer</td>
             <td>$1000</td><td>100 : </td>
             </tr>
         <tr><td>IPOD</td><td>music player</td>
             <td>$200</td><td>200 : ON SALE NOW!</td>
             </tr>
         <tr><td>CINEMA DISPLAY</td><td>screen</td>
             <td>$800</td><td>300 : BEST DEAL!</td>
             </tr>
    </table>
      We love you!  Please check out our Gold Customer specials!
```
In addition to getting our templates from 

&lt;textarea&gt;

 elements that are part of our HTML page, we can also use templates from straight JavaScript Strings...
```
  <script language="javascript">
    var myStr = "Hello ${customer.first} ${customer.last}, Welcome back!";

    // Using the process() method is easy...
    result = myStr.process(data);

    // Or, why bother with the middle-man variable?
    result = "Hello ${customer.first} ${customer.last}, Welcome back!".process(data);

    // The result will be "Hello John Public, Welcome back!"
    // which is the same as...
    result = "Hello " + customer.first + " " + customer.last + ", Welcome back!";

    // We can also do a one-time parse, to save parsing costs...
    var myTemplateObj = TrimPath.parseTemplate(myStr);
    var result  = myTemplateObj.process(data);
    var result2 = myTemplateObj.process(differentData);
  </script>  
```
You probably want to see this in action, so take a look at the [JST demo page](http://trimpath.com/demos/test1/trimpath/template_demo.html).


---

## Where Shall I Put My JavaScript Templates? ##

In the above introduction, we put our JST template into a hidden 

&lt;textarea&gt;

 in our HTML page.
  * Of course, you can have several 

&lt;textarea&gt;

 elements on a single HTML page, each holding a different JST template.
    * The general syntax is like: 

&lt;textarea id="my\_template\_jst" style="display:none;"&gt;

 ... template body ... 

&lt;/textarea&gt;


    * Be careful about putting these JST 

&lt;textarea&gt;

's into a 

&lt;form&gt;

, unless you want them posted or sent to a server during a submit button click.
  * Why 

&lt;textarea&gt;

?
    * The 

&lt;textarea&gt;

 element has the nice property of not radically messing up the formatting of its innerHTML.
      * This stability of 

&lt;textarea&gt;

's innerHTML is important because JST syntax allows you to place control flow tags (like if/elseif/for) in all sorts of odd places, such as even -inside- HTML tags.  For example:
        * <option value="${country.name}" {if country.name == currCountry}selected{/if}>
      * Just as with many server-side template languages, the JST syntax is not true HTML/XHTML/XML.  Hence, the beauty of using 

&lt;textarea&gt;

 elements to hold our JST templates.
    * A technical note about 

&lt;textarea&gt;

:  browsers will convert the contents of a 

&lt;textarea&gt;

 with any body characters of < and > to &lt; and &gt;
      * So, TrimPath.parseDOMTemplate() and TrimPath.processDOMTemplate() automatically reconvert back to < and > so that you have a nice usable JST template.

Alternatively, you might keep **.jst files on your web server and load them into the browser using XMLHttpRequest or a hidden iframe.**

## Server-side JST Evaluation ##
We designed JST to run in a web browser AND also in any standalone JavaScript interpreter (such as Mozilla Rhino or Spider Monkey).  The core JST engine is meant to have no critical DOM/DHTML/browser dependencies.


---

{ [JavaScript Templates (JST) home](http://code.google.com/p/trimpath/wiki/JavaScriptTemplates) | [API](http://code.google.com/p/trimpath/wiki/JavaScriptTemplateAPI) | [syntax](http://code.google.com/p/trimpath/wiki/JavaScriptTemplateSyntax) | [modifiers](http://code.google.com/p/trimpath/wiki/JavaScriptTemplateModifiers) | [download](http://code.google.com/p/trimpath/downloads/list) | [community](http://code.google.com/p/trimpath/wiki/JavaScriptTemplateDiscussion) }