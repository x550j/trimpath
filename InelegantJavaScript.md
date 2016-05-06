# Inelegant JavaScript #

A collection of bad things about JavaScript, such as buggy language implementation gotchas and quirks, especially in browsers. -- SteveYen

  * [Where's my memory?](http://www.bazon.net/mishoo/articles.epl?art_id=824) delves into Internet Explorer's JavaScript implementation (also known as JScript), which '''leaks memory when using closures'''.
    * Actually, it's more about COM/DOM objects in a deadly circular embrace with JavaScript objects.
    * Also http://jgwebber.blogspot.com/2005/01/dhtml-leaks-like-sieve.html
  * Flash's version of JavaScript, aka ActionScript, is different.
    * Shockingly '''ActionScript does not support closures''' (in Flash Players prior to version 6).
      * Here's the [description and workaround](http://www.laszlosystems.com/~adam/blog/archives/000018.html).
    * '''ActionScript does not support eval()'''.
      * Or, eval() in ActionScript can only do variable lookups, not true expressions.
    * '''ActionScript does not support dynamic regular expressions using the RegExp object'''.
    * http://livedocs.macromedia.com/flash/mx2004/main_7_2/wwhelp/wwhimpl/common/html/wwhelp.htm?context=Flash_MX_2004&file=00000756.html#comments
    * These issues will make it very very hard to impossible to get TrimPath technology to work in Flash/ActionScript.
      * Maybe things have changed, though, in Flash 8 or beyond (Apollo/AIR)
  * Laszlo implementation of EcmaScript...
    * Documented differences can be found [here](http://www.laszlosystems.com/lps-2.2/docs/guide/ecmascript-and-lzx.html#ecmascript-and-lzx.differences).
    * Laszlo EcmaScript limitations might be same as ActionScript limitations (since Laszlo deploys to Flash runtime)?
  * Internet Explorer and Mozilla treat JavaScript '''objects and arrays''' shortcut syntax differently.
    * The below code with the extra trailing commas works in Mozilla JavaScript engines (Spider Monkey and Rhino).  But it causes an exception in Internet Explorer.  Too bad -- it actually made programming easier, not having to mentally treat the last item differently.
      * var x = { slot1: 123, slot2: 456, };
      * var y = [1, 2, 3, ](.md);
  * '''String.split()''' with regular expressions is different in IE and Mozilla/Firefox.  Type the following into your browser's URL location bar...
    * javascript:"a${b}c".split(/(\$\{[^\}]+\})/g)
      * Mozilla/Firefox returns "a,${b},c" -- I prefer this return style.
      * IE returns "a,c" -- this seems lossy to me, ignoring the capture parenthesis in the regular expression.
    * If you type in "javascript:"a${b}c".split(/\$\{[^\}]+\}/g)", you get "a,c" in both browsers.
    * You also can't trust the edge cases of the expression appearing at the start and end of the string or right next to each other.
    * The upshot is not to use the parenthesis and test everything.  Or, don't use regular expressions.
    * Here's a comparison table...
| URL code | Mozilla/Firefox | IE |
|:---------|:----------------|:---|
| javascript:"a${b}c".split(/(\$\{[^\}]+\})/g) | a,${b},c        | a,c |
| javascript:"a${b}".split(/\$\{[^\}]+\}/g) | a,              | a  |
| javascript:"${b}c".split(/\$\{[^\}]+\}/g) | ,c              | c  |
| javascript:"a${b}${c}d".split(/\$\{[^\}]+\}/g) | a,,d            | a,d |

  * '''eval() and function differences'''
| URL code | Mozilla/Firefox | IE |
|:---------|:----------------|:---|
| javascript:function(){return 111} | nothing!        | nothing! |
| javascript:(function(){return 111}) | function () { return 111; } | nothing! |
| javascript:eval("function(){return 111;}") | nothing!        | nothing! |
| javascript:eval("var s=function(){return 111};s") | function () { return 111; } | function(){return 111} |
  * This blog [entry from Eric Lippert](http://blogs.gotdotnet.com/EricLi/PermaLink.aspx/4939ad1e-b2d7-436e-a2dc-bd7665d207bf) who worked on JScript and VBScript at Microsoft explains why.

  * String charAt works in IE and Mozilla (str.charAt(index)).
    * '''Do not rely on str[index](index.md)''' array-based-syntax shortcut syntax, which might not work in IE.
  * Be wary of using undefined global variables in IE.
    * Another blog entry from Eric Lippert [explains why](http://blogs.gotdotnet.com/EricLi/PermaLink.aspx/d16a5b2a-b273-44ca-ad62-31284f03744c).

## DOM Related Quirks ##
  * innerHTML doesn't work in IE for TABLE elements, but works for Mozilla/Firefox.
  * Other resources
    * PPK has a [quirksmode section for JavaScript](http://www.quirksmode.org/js/intro.html) quirks across various browsers
    * I recommend O'Reilly's '''JavaScript and DHTML Cookbook''' highly.


---

See also: ElegantJavaScript