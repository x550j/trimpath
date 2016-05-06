# JavaScript Template Standard Modifiers #
{ [JavaScript Templates (JST) home](http://code.google.com/p/trimpath/wiki/JavaScriptTemplates) | [API](http://code.google.com/p/trimpath/wiki/JavaScriptTemplateAPI) | [syntax](http://code.google.com/p/trimpath/wiki/JavaScriptTemplateSyntax) | [modifiers](http://code.google.com/p/trimpath/wiki/JavaScriptTemplateModifiers) | [download](http://code.google.com/p/trimpath/downloads/list) | [community](http://code.google.com/p/trimpath/wiki/JavaScriptTemplateDiscussion) }

The JavaScript Template engine includes several standard modifiers, which are used to modify the output of expressions.  For example, the capitalize modifer is used like: ${customer.countryName|capitalize}

Also, note that modifiers MUST BE WRITTEN as
```
${whatever_expression|modifier}   //OK
${whatever_expression |modifier}  //OK
```
and that added whitespace WILL NOT WORK, such as
```
${whatever_expresion | modifier} //BROKEN
${whatever_expresion| modifier}  //BROKEN 
```
...that bar has got to be right up against the modifier name

See the [syntax](http://code.google.com/p/trimpath/wiki/JavaScriptTemplateSyntax) page for how to use modifiers and the [API](http://code.google.com/p/trimpath/wiki/JavaScriptTemplateAPI) page for how to add your own custom modifiers.

  * '''capitalize'''
    * This modifier capitalizes all the letters in the string, using String.prototype.toUpperCase()
    * Example: ${customer.emailAddr + " " + customer.countryName|capitalize} might produce something like "BILLG@MICROSOFT.COM USA"
  * '''default:valueWhenNull'''
    * This modifier tests the expression for null.  If null, the default modifier returns the valueWhenNull argument.
    * Examples:
      * ${customer.aliasName|default:"anonymous"}
      * ${customer.aliasName|default:customer.emailAddress)}
    * Note, if your expression is a top level variable, JavaScript makes a distinction between null and undefined values.  If your expression is a top level variable reference and is undefined (instead of null), you'll get an exception, even if you try to protect the reference by using the '''default''' modifier.
      * For example if systemAdminMessage is null, the following will work fine.  But, if systemAdminMessage is undefined, the following will throw an exception.
        * <div> ${systemAdminMessage|default:""} </div>
      * To prevent the error, you can use the defined(str) helper function, in a protective if statement.  For example...
        * {if defined('systemAdminMessage')} <div> ${systemAdminMessage|default:""} </div> {/if}
  * '''eat'''
    * The eat modifier always returns the empty string or "".
    * The eat modifier is useful if you want to run some JavaScript code, but do not want to output anything.
    * Example: ${shoppingCart.calculateTaxes(shippingAddress)|eat}
  * '''escape'''
    * Converts the characters of & to &amp;, < to &lt;, and > to &gt;.
    * The escape modifier is useful to prevent Cross-Site Scripting attacks, preventing evildoers from injecting their own HTML or JavaScript into your pages.
    * Example: ${blogEntry.text|escape}
  * '''h'''
    * The h modifier is the same as the escape modifier.
    * Example: ${blogEntry.text|h}

---


Brian Bittman, who is a heavy user of perl's Template Toolikit (TT) http://www.template-toolkit.org, went ahead and implemented all, er most of, the standard filters that TT provides in it's library.  See this page (TT calls JST's modifiers, "filters"): http://www.template-toolkit.org/docs/plain/Manual/Filters.html

still left to implement are a parallel of the eval/evaltt and perl/evalperl functions - the eval/evaltt modifiers would parse the string as a JST template and the perl/evalperl functions would parse the string as javascript (that seems a bit unnesscessary though).  Also the uri and html\_entity modifiers were not implemented due to time contraints, please feel free to do so. 'null' was not implemented because 'eat' already exists and 'null' is a javascript reserved word.  stderr was replaced with the 'alert' modifier, which will display the string in an alert dialog

Yes, some of these modifiers overlap with existing JST provided modifiers.  So, now there's More Than One Way To Do It.


---


I think this seems like a sensible way of grouping your modifiers into several "libraries": defining several hashes of them and then merging them all into one giant hash like so:
```
   var Modifiers = hashmerge(GroupOfModifiers1, AnotherModifierLib, EvenMoreModifier_hash);
   var t = TrimPath.parseTemplate(someTemplateCode);
   var out = t.process({ _MODIFIERS: Modifiers });
```
though I feel like there should be some built-in facility for this, perhaps if it were to behave thusly:
```
   var t = TrimPath.parseTemplate(someTemplateCode);
   var out = t.process({ _MODIFIERS: [ GroupOfModifiers1, AnotherModifierLib, EvenMoreModifier_hash ] });
```
thoughts?

Anyhow, though it's pretty straightforward, here's my hashmerge() code:
```
   function hashmerge() {
//note that "keys" that exist in more than one hash will
//contain the value of that in the hash closest to the end of the argument list
        var ret = arguments[0];        
        for(var i=1; i<arguments.length; i++) {
            var h = arguments[i];            
            for(k in h) {
                ret[k] = h[k];
            }
        }
        return ret;
    }
```


---

Do you want more modifiers?  Please make [suggestions](http://code.google.com/p/trimpath/wiki/JavaScriptTemplateDiscussion).  Or, perhaps you'd like to [contribute](http://code.google.com/p/trimpath/wiki/HowToContribute) your own custom modifiers?


---

{ [JavaScript Templates (JST) home](http://code.google.com/p/trimpath/wiki/JavaScriptTemplates) | [API](http://code.google.com/p/trimpath/wiki/JavaScriptTemplateAPI) | [syntax](http://code.google.com/p/trimpath/wiki/JavaScriptTemplateSyntax) | [modifiers](http://code.google.com/p/trimpath/wiki/JavaScriptTemplateModifiers) | [download](http://code.google.com/p/trimpath/downloads/list) | [community](http://code.google.com/p/trimpath/wiki/JavaScriptTemplateDiscussion) }