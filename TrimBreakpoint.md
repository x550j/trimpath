# TrimBreakpoint #
{ [TrimBreakpoint home](http://code.google.com/p/trimpath/wiki/TrimBreakpoint) -- [download](http://code.google.com/p/trimpath/downloads/list) | [demo](http://trimpath.com/demos/test1/trimpath/breakpoint_demo.html)}

'''TrimBreakpoint''' is a super lightweight debugging utility for JavaScript, to temporarily stop code execution so that you can inspect browser variables at runtime, including any variable in scope.  It's meant for when you're too lazy to pull out your [Venkman](http://www.svendtofte.com/code/learning_venkman/) or Visual Studio and when those alert()'s just aren't doing enough for you.

Note: TrimBreakpoint was written before [Firebug](http://www.getfirebug.com) sprang to wonderous life.  If you're on Firefox, use Firebug.  It has a great debugger/breakpoint feature, amongst many others.  If you're on IE without any other tools, on the other hand... shrug.

Here's a [demo for TrimBreakpoint](http://trimpath.com/demos/test1/trimpath/breakpoint_demo.html)

## Usage ##
First, include the trimpath/breakpoint.js script, like...
```
<html>
 <head>
  <script language="javascript" src="/javascripts/trimpath/breakpoint.js"></script>
  ...
 </head>
 ...
</html>
```

Then, in the middle of your code wherever you want to put a breakpoint, add a line of code like...
```
breakpoint(function(expr){return eval(expr);});
```
(Obviously, this is best when copy-&-pasted.)

If you end up setting a zillion breakpoints, you can also pass a message, to give your breakpoints a name, like...
```
breakpoint(function(expr){return eval(expr);}, "breakpoint in cal.validate()");
```

If you've set the breakpoint in a loop body, it's also useful to show the current loop variables, like...
```
breakpoint(function(expr){return eval(expr);}, 
           "convertList() currCounter is " + currCounter);
```

Then, at runtime, when the breakpoint line is reached, execution will stop and a prompt dialog will open where you can enter JavaScript expressions in order to...
  * Inspect variables and objects in the breakpoint's scope.  For example, "document.getElementById('foo').style.width"
  * Set and modify variable values, too.  For example, "currCounter = 0"
  * Additionally, you can also call alert() from the breakpoint prompt dialog, such as when you want to inspect something with a large output value.  For example, "alert(myHiddenTextarea.value)"

The breakpoint prompt dialog will repeatedly ask for more expressions until you click Cancel or enter nothing.  After you Cancel, normal program execution will resume.

To help save on repeated typing during debugging, you can also pass an optional initial expression string as the 3rd parameter to breakpoint(), like...
```
breakpoint(function(e){return eval(e);}, 
           "convertList() currCounter is " + currCounter,
           "resultList[resultList.length - 1]");
```

Think of TrimBreakpoint as the interactive version of the good old alert() trick.

One nice feature of TrimBreakpoint is that the breakpoints are persistent and settable right from your favorite text editor, because the breakpoint() calls are just right in your code.  That's nice for when you're hitting your browser's reload button a lot while cursing at that mysterious JavaScript bug.

TrimBreakpoint also behaves pretty much the same across different browsers (at least for IE 6.x and Firefox 1.0, which I tried it against).

You can pickup TrimBreakpoint from the [downloads page](http://code.google.com/p/trimpath/downloads/list).

Here's the [blog post about TrimBreakpoint](http://trimpath.com/blog/?p=24).


---

{ [TrimBreakpoint home](http://code.google.com/p/trimpath/wiki/TrimBreakpoint) -- [download](http://code.google.com/p/trimpath/downloads/list) | [demo](http://trimpath.com/demos/test1/trimpath/breakpoint_demo.html) }