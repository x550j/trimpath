# Junction #

The open source Junction framework is a conventions-over-configuration, synchronizing web MVC framework for JavaScript.

  * '''TrimPath Junction''' is a clone or port of the terrific [Ruby on Rails](http://www.rubyonrails.com/) web MVC framework into JavaScript.
  * '''TrimPath Junction''' is also sometimes referred to as TrimJunction, or as just Junction.

Show me some code: here are some code snippets comparing [Ruby on Rails versus JavaScript on Junction](http://code.google.com/p/trimpath/wiki/RubyRailsVsJavaScriptJunction).

Junction is your best bet if you want to build _single page applications_ using tried-and-true web MVC paradigms, while letting you leverage new technologies like Google Gears.

## More ##
  * **[documentation](http://trimpath.googlecode.com/svn/trunk/junction_docs/index.html)**
  * **[download](http://code.google.com/p/trimpath/downloads/list)**

## News ##
  * 2007/09/10 - Junction 1.1.18 and Next Action 0.94 released.
  * 2007/08/31 - Junction introduction article at O'Reilly onlamp.com.  Thanks to Jack Herrington, who did all the writing!
    * http://www.onlamp.com/pub/a/onlamp/2007/08/30/introducing-trimpath-junction.html
    * http://ajaxian.com/archives/trimpath-junction-a-walk-through
  * 2007/08/29 - Junction 1.1.16 and Next Action 0.92 released, with new modelInit() syntax for Model definitions, and bug fixes.
  * 2007/08/15 - Junction 1.1.14 released, with new TrimQuery improvements.  Also, Next Action 0.90 and Happy Birthday Alexander!
  * 2007/08/13 - Junction 1.1.12 and Next Action 0.88 released, with bug fixes and data export/import feature.
  * 2007/08/06 - Junction 1.1.10 released, which now runs on Linux.
  * 2007/08/04 - Junction 1.1.8 and Next Action 0.86 released, with big speed improvement.
  * 2007/07/30 - Junction 1.1.6 and Next Action 0.84 released.
  * 2007/07/12 - After two years, [Junction and Next Action finally get updated](http://trimpath.com/blog/?p=69).  Junction 1.1.2 and Next Action 0.80.
  * 2007/07/12 - New API docs online at http://trimpath.googlecode.com/svn/trunk/junction_docs/index.html
  * 2007/07/06 - [Google Gears is the missing link to Junction](http://trimpath.com/blog/?p=53)!
  * 2007/06/25 - Rails on JS forthcoming by Steve Yegge - http://www.iunknown.com/2007/06/steve-yegge-por.html
  * 2005/07/18 - New Release - 1.0.38
  * 2005/07/06 - A new demo app is available, called [Next Action](http://code.google.com/p/trimpath/wiki/NextAction)

## Vision ##

In a nutshell: copy Rails when possible, because it's a great web MVC framework.  Next, riff on Rails to allow for doing cool, non-Rails maneuvers that are only possible with JavaScript.

The result should be a web MVC framework that runs in either the client AND the server (Rhino).  While we're at it, explore and push the edges of occasionally connected web applications, providing easy synchronization between distributed code and data.

Long version:

With an eye towards [Don't Repeat Yourself](http://code.google.com/p/trimpath/wiki/DontRepeatYourself), spiced with a little Ajax, the grand vision of the TrimPath Junction project is to be able to write web application logic just once, in familiar languages like JavaScript.

A Junction-based web application should run, validate input, process data, generate output and do its thing on both the server AND the client.  We use the Rhino JavaScript runtime for the server and use your favorite, modern web browser for the client.

If you know Rails, then Junction will be very familiar stuff.  Maybe you can help the Junction project to grow up.

If you know JavaScript, then all the good things you've been hearing about Rails is now becoming available to you through the Junction project.  Except, you don't need to learn yet another programming language.

With Junction, you still need to know HTML/DHTML, DOM, CSS, and SQL.  You still need to know about HTTP, cookies, and sessions.  Junction doesn't hide these things from you and magically turn your web browser into the next Visual Basic IDE.  Junction believes in fealty to web standards.

But, you will probably need to learn about offline, occasionally connected databases and application design.  Junction deviates from its Rails roots in pushing on this feature area.  Think synchronization and poor (really destitute) man's Notes or Groove.  Junction aims to provide a web-standards-based reincarnation of Notes or Groove.  This is the key new area that Junction explores and aspires to push forward.

Junction, like Rails, also stays out of your way.  If you need to specify HTML down to the Nth degree, you can.  If you want to bring in more web GUI widgets (like from Dojo Toolkit or htmlArea or Flash or Prototype), you can.  Junction is a web Model-View-Controller (MVC) framework, not about hiding JavaScript or the DOM model.

## Model - View - Controller ##

Here's how the web MVC paradigm appears in Junction.

  * '''Model''' - in the Junction world, these are just a plain old JavaScript 'classes'.  Actually, that means these are just plain old JavaScript functions, and they follow the [Active Record pattern](http://www.martinfowler.com/eaaCatalog/activeRecord.html) of object-relational mapping.  Fancy queries are done using SQL, even when offline or disconnected from the server.  We use the TrimQuery project and Google Gears RDBMS to allow SQL to run in a (possibly disconnected) web browser data cache.
```
BlogPost = function() {}

modelInit('BlogPost');

BlogPost.hasMany('Comments);

BlogPost.findByShortName = function(shortName) {
    return BlogPost.find("first", { conditions: ["BlogPost.shortName = ?", shortName] });
}
```

  * '''View''' - these are [JavaScript Templates](http://code.google.com/p/trimpath/wiki/JavaScriptTemplates) (JST).  The JST engine is another project of the TrimPath effort.  JST templates should be familiar to anyone who knows other HTML templating markup syntax, like PHP, FreeMarker, Velocity, Smarty.
```
<div>
  Comments...
  <ul>
    {for comment in blogPost.getComments()}
      <li><span>${comment.author} says...</span>
          <div>${comment.body}</div></li>
    {/for}
  </ul>
</div>
```

  * Alternatively, if you like rhtml/ERb/JSP style syntax, Junction can also use EcmaScriptTemplates.  For example...
```
<div>
  Comments...
  <ul>
    <% for (var comments = blogPost.getComments(),
                i = 0; i < comments.length; i++) { %>
      <li><span><%= comments[i].author %> says...</span>
          <div><%= comments[i].body %></div></li>
    <% } %>
  </ul>
</div>
```

  * '''Controller''' - these are also just plain old JavaScript functions.  Notice that Junction supports scaffolding, just like with Rails...
```
BlogPostController = function() {}

scaffold(BlogPostController, 'BlogPost')
```

It's all hooked together through [convention, not configuration](http://www.onlamp.com/pub/a/onlamp/2005/01/20/rails.html).  That is, if you just follow the naming conventions, there are no configuration files.  The Junction framework favors reflection and late-bound dynamic object decorations and extensions.  The result is less code, no recompilation steps, and faster development.

## Links ##
  * [Junction Documentation](http://trimpath.googlecode.com/svn/trunk/junction_docs/index.html)
  * [Online Junction Demo](http://code.google.com/p/trimpath/wiki/TrimJunctionDemo)
  * [How Does Junction Work](http://code.google.com/p/trimpath/wiki/TrimJunctionHowItWorks)
  * [Follow A Request Through Junction](http://code.google.com/p/trimpath/wiki/TrimJunctionFollowARequest)
  * [Junction Controller](http://code.google.com/p/trimpath/wiki/TrimJunctionController)
  * [Quick Application Prototyping Using Junction](http://code.google.com/p/trimpath/wiki/PrototypingWithJunction)

  * [Spa -- Single Page Applications](http://code.google.com/p/trimpath/wiki/SinglePageApplications)
  * [Spade -- Single Page Application and Development Environment](http://code.google.com/p/trimpath/wiki/SinglePageApplicationAndDevelopmentEnvironment)

  * [Download Junction](http://code.google.com/p/trimpath/downloads/list)
    * TrimPath Components Used In Junction
      * [TrimPath JavaScript Templates](http://code.google.com/p/trimpath/wiki/JavaScriptTemplates)
      * [TrimPath TrimQuery](http://code.google.com/p/trimpath/wiki/TrimQuery)
      * [TrimPath Breakpoint](http://code.google.com/p/trimpath/wiki/TrimBreakpoint)
    * Other Requirements
      * [Prototype](http://prototype.conio.net)

  * External Resources
    * [Ruby on Rails](http://www.rubyonrails.com/)
      * Learning Ruby and Rails is a good thing, whether or not you dig Junction or not.  Rails and Ruby are very interesting technologies that show off the power of dynamic languages. Plus, since Junction suffers from a newborn project's lack of documentation, the best way to understand Junction right now is to know a little bit of its parent, Rails.
      * [Ruby on Rails versus JavaScript on Junction](http://code.google.com/p/trimpath/wiki/RubyRailsVsJavaScriptJunction) code comparison.
    * Solving The Back Button
      * [Dojo Toolkit](http://dojotoolkit.org)
      * [Back Button Article](http://www.contentwithstyle.co.uk/Articles/38/fixing-the-back-button-and-enabling-bookmarking-for-ajax-apps)
    * [Prototype](http://prototype.conio.net)
      * Junction uses the Prototype library from Sam Stephenson for Ajax and DOM animation effects.
    * [Groove Networks](http://groove.net)
    * [JSON](http://code.google.com/p/trimpath/wiki/JsonLibrary)


---

{ [Junction home](http://code.google.com/p/trimpath/wiki/TrimJunction) | [documentation](http://trimpath.googlecode.com/svn/trunk/junction_docs/index.html) | [download](http://code.google.com/p/trimpath/downloads/list) }