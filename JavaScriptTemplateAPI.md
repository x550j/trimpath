# JavaScript Templates API #
{ [JavaScript Templates (JST) home](http://code.google.com/p/trimpath/wiki/JavaScriptTemplates) | [API](http://code.google.com/p/trimpath/wiki/JavaScriptTemplateAPI) | [syntax](http://code.google.com/p/trimpath/wiki/JavaScriptTemplateSyntax) | [modifiers](http://code.google.com/p/trimpath/wiki/JavaScriptTemplateModifiers) | [download](http://code.google.com/p/trimpath/wiki/TrimPathDownload) | [community](http://code.google.com/p/trimpath/wiki/JavaScriptTemplateDiscussion) }

## Using JavaScript Templates ##
First, in your HTML/JSP/PHP/ASP file, include the trimpath/template.js JavaScript file.
```
  <script language="javascript" src="trimpath/template.js"></script>
```

The trimpath/template.js file can live anywhere you want and may be renamed.  It has no dependencies to other files.  For example, you might move it to lib/trimpath/template.js or js/trimpath/template.js.  We suggest using a subdirectory named trimpath to help you remember what it's about and also give you a space to place more (future) TrimPath components.  Future TrimPath components might depend on living in the same directory as trimpath/template.js.

After including trimpath/template.js, a JavaScript variable named TrimPath will hold an Object ready for you to use.


---

## The TrimPath Object ##
The TrimPath Object is the global singleton object that is the access point for all TrimPath components.  Except for this TrimPath Object, we try to keep the global variable namespace clean for you.

The TrimPath Object will have the following JST-related methods...

### TrimPath.parseDOMTemplate ( elementId, optionalDocument ) ###
Retrieves the innerHTML of the document DOM element that has the given elementId.  The innerHTML is then parsed as a template.  This method returns a '''templateObject''', which is described later.  Throws an exception on any parsing error.  The method parameters are...
| '''elementId''' | Id of the DOM element whose innerHTML is used as the template. |
|:----------------|:---------------------------------------------------------------|
| '''optionalDocument''' | An optional DOM document, which is useful when working with multiple documents with iframes, framesets or multiple browser windows. [[BR](BR.md)] Defaults to document. |

The document DOM element is usually a hidden 

&lt;textarea&gt;

.  You can use a style attribute to hide a textarea.  For example:
  * 

&lt;textarea id="elementId" style="display:none;"&gt;

 template body 

&lt;/textarea&gt;



### TrimPath.processDOMTemplate ( elementId, contextObject, optionalFlags, optionalDocument ) ###
Helper function that calls TrimPath.parseDOMTemplate() and then the process() method on the returned '''templateObject'''.  The output of templateObject.process() is returned.  Throws an exception on any parsing error.  The method parameters are...
| '''elementId''' | Same as for TrimPath.parseDOMTemplate. |
|:----------------|:---------------------------------------|
| '''contextObject''' | See templateObject.process()           |
| '''optionalFlags''' | See templateObject.process()           |
| '''optionalDocument''' | Same as for TrimPath.parseDOMTemplate. |

### TrimPath.parseTemplate ( templateContentStr, optionalTemplateName ) ###
Parses a String as a template and returns a '''templateObject''' which is described later.  Throws an exception on any parsing error.  The method parameters are...
| '''templateContentStr''' | String that has JST markup. | For example: "Hello ${firstName} ${lastName}" |
|:-------------------------|:----------------------------|:----------------------------------------------|
| '''optionalTemplateName''' | An optional String of the template's name. [[BR](BR.md)] Used to help in debugging. |                                               |


---

## The templateObject ##
A templateObject is returned by TrimPath.parseTemplate() and TrimPath.parseDOMTemplate().  It represents a successfully parsed template.  A templateObject has one key method...

### templateObject.process ( contextObject, optionalFlags ) ###
The process() method merges the template with the '''contextObject'''.  The process() method may be called repeatedly, without any template parsing performance overhead, so the highest performance can be had by caching and reusing templateObjects.  The return value of this method is a String of the 'rendered' template.

The '''contextObject''' parameter must be an Object, which becomes part of the lookup "scope" of the template.  If a template refers to ${a}, then contextObject.a is accessed.  For ${a.b.c}, then contextObject.a.b.c is acccessed.

Note that the '''contextObject''' can contain any JavaScript object, including strings, numbers, date, objects and functions.  So, calling ${groupCalender(new Date())} would call contextObject.groupCalender(new Date()).  Of course, you would have to supply the groupCalender() function, which should return a string value.

The '''optionalFlags''' may be null, or an Object that with the following optional slots...
| throwExceptions | Defaults to false. [[BR](BR.md)] When true, the process() method will forward exceptions by re-throwing them. [[BR](BR.md)] When false, any exceptions will stop template processing.  The caught exception is converted to a String message, which is then appended onto the process()'s return value String, which has any partially processed results so far. |
|:----------------|:-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| keepWhitespace  | Defaults to false. [[BR](BR.md)] When true, all whitespace characters are emitted during template processing. [[BR](BR.md)] When false, various whitespace characters (newline, space, tab) that surround statement tags (such as {if}, {else}, {for}) are stripped out to make any output look "cleaner and prettier".                                          |


---

## The String.prototype.process() method ##

### String.prototype.process ( contextObject, optionalFlags ) ###

As a convenience, the String prototype is enhanced with a process() method.  It parses the String as a template and invokes process().  The arguments are the same as for templateObject.process().
```
   var result = "hello ${firstName}".process(data)
   // ...is equivalent to...
   var result = TrimPath.parseTemplate("hello ${firstName}").process(data);
```


---

## Adding Custom Modifiers ##
You may add your own custom modifiers by placing them into a '''_MODIFERS''' Object, which should then be placed into the '''contextObject''' that is passed to the templateObject.process() method.  Each custom modifier should be a function that takes minimally one String argument and returns a String.  For example...
```
  var myModifiers = {
    hello : function(str, greeting) { 
      if (greeting == null)
          greeting = "Hello";
      return greeting + ", " + str;
    },
    zeroSuffix : function(str, totalLength) {
      return (str + "000000000000000").substring(0, totalLength);
    }
  };
  var myData = {
    firstName : "John", 
    getCurrentPoints : function() { /* Do something here... */ return 12; }
  }

  myData._MODIFIERS = myModifiers;

  "${firstName}".process(myData) == "John"
  "${firstName|hello}".process(myData) == "Hello, John"
  "${firstName|hello:"Buenos Dias"}".process(myData) == "Buenos Dias, John"
  "${firstName|hello:"Buenos Dias"|capitalize}".process(myData) == "BUENOS DIAS, JOHN"

  "${getCurrentPoints()}".process(myData) == "12"
  "${getCurrentPoints()|zeroSuffix:4}".process(myData) == "1200"
```_


---

{ [JavaScript Templates (JST) home](http://code.google.com/p/trimpath/wiki/JavaScriptTemplates) | [API](http://code.google.com/p/trimpath/wiki/JavaScriptTemplateAPI) | [syntax](http://code.google.com/p/trimpath/wiki/JavaScriptTemplateSyntax) | [modifiers](http://code.google.com/p/trimpath/wiki/JavaScriptTemplateModifiers) | [download](http://code.google.com/p/trimpath/downloads/list) | [community](http://code.google.com/p/trimpath/wiki/JavaScriptTemplateDiscussion) }