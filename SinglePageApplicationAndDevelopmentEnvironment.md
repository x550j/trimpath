# S''ingle'' [Page](Page.md) '''Applicati'''on and Development Environment (Spade) #

A "Single Page Application and Development Environment" (Spade) is a [Spa](http://code.google.com/p/trimpath/wiki/SinglePageApplications) application with an integrated development environment, all fitting onto a single web page.

More than just the classic View Source that we all know and love, with Spade techniques, you can also Manipulate Source or Edit Source right on the page.  You can tinker and improve the app, all without firing up Eclipse or Emacs.  No extra Ant or Apache needed.  The historic edit-compile-link-run-debug programming cycle loses one more small step: no more the Alt-Tab'ing between text editor and web browser.  Using a Spade, it's all now in the browser.

## Is Spade Possible? ##

Modern browsers like Firefox (and Internet Explorer, somewhat) appear to save any programmatic changes that were made to the DOM tree when you do a "File -> Save Page" command.  So, the question is whether there's a pathway or set of techniques that can utilize this feature or loophole to allow presentation, data, and code/behavior to be saved again and again, as a developer continues to improve his or her [Single Page Application](http://code.google.com/p/trimpath/wiki/SinglePageApplications).

## Is Spade Worthwhile? ##

Well, we'll see.  Throw it out there and see if it sticks.

Related: does Spade deserve YABA (Yet Another Bleeping Acronym)?  What, you mean like Ajax?

## Known Spade Applications ##
  * [Next Action](http://code.google.com/p/trimpath/wiki/NextAction) -- one of the first applications built using the [Junction MVC framework](http://code.google.com/p/trimpath/wiki/TrimJunction).  [Next Action](http://code.google.com/p/trimpath/wiki/NextAction) is a todo-list manager in the [Getting Things Done](http://code.google.com/p/trimpath/wiki/GettingThingsDone) style.
  * [EditThisPagePHP](http://editthispagephp.sourceforge.net/) -- at surface simple, a single page "edit this page" interface. However, also supports trackbacks, comments, diffs, RSS feeds for current version and diffs, one file installer, etc. Earlier versions also allowed you to edit the PHP in the script, but this is now disallowed for security reasons.
    * This seems to require a webserver with PHP.
  * See also: http://c2.com/cgi/wiki?WikiWithProgrammableContent
  * [tiddlywiki](http://www.tiddlywiki.com/) is a Spade-style wiki implemented in a single page, as is [GTD tidliwiki](http://shared.snapgrid.com/gtd_tiddlywiki.html).
  * (Heard of other Spade examples?  Please add them here.)


---



---


## Your Thoughts ##
  * Speaking of AJAX, this would be a great tool to mix with HTTPREQUEST (unless I'm just plain stupid, and this is HTTPREQUEST).
  * Not sure if AJAX will be compatible with Spade, since you will run into cross-domain restrictions placed on pretty much every AJAX implementation.  The problem is that the Spade page has to be saved to your local computer, whereas the AJAX server will normally be retrieved from the web.
    * Good point.  I'm shooting for now to have explicity user-driven synchronization working, which my experiments lead me to believe is workable. -- SteveYen
  * I have enjoyed this reading or than using my time getting [Air Force Ones](http://www.bayareakicks.com/air-force-ones.html) shoes online.

http://trimpath.com/project/attachment/wiki/SinglePageApplicationAndDevelopmentEnvironment/spades.jpg?format=raw

TESTTESTtest