# TrimJunction Community #

{ [TrimJunction home](http://code.google.com/p/trimpath/wiki/TrimJunction) | [community](http://code.google.com/p/trimpath/wiki/TrimJunctionCommunity) }


---

'''UPDATE: Please see the [TrimPath Forum](http://trimpath.com/forum/index.php) for the latest place to discuss stuff.'''

We'll keep these community discussion wiki pages around, though, for reference.  -- SteveYen 2005/07/10


---

This is an open wiki page for you about TrimPath Junction usage, issues, rants, raves, and questions.  Please don't hesitate to add your JoJ info & feedback. -- SteveYen


This is definately interesting. Any thoughts on how you plan to implement off-line saving? Since the browser's "save as" doesn't save the in-memory dom state AFAIK. I suppose you might be able to do some kind of trick where you 'rebuild' the page dynamically via a doc.write() call, supplying all the edited data, but if I recall correctly doc.write() doesn't work after the page is done loading(?). It would be nice if you do it entirely in script allowing such things as a Ctrl-S for saving, but I guess that would take a signed script if it was possible.

- Robert McIntosh

Hi Robert, I'm slowing getting back into "normal" life after the loss of my father.

The approach I was taking is like you suggest.  When the user clicks a "save offline" button, I'd pop open a new window with all the edited data applied with a doc.write().  The user is also invited to do a File->Save As.

Also, for Mozilla browsers, some Mozilla extension or greasemonkey script could be added to remove any extra UI steps, so that Ctrl-S works.

- Steve Yen


---


''...after the loss of my father''[[BR](BR.md)]
Sorry to know that - My condolences are to you.

- BJessen


---


I love the idea of an MVC architecture in javascript. I've had thoughts about doing something similar recently. I've also had some experience with Ruby lately and I think it is an excellent framework to base this system on. I do have a few queries/ideas.

I'd like to develop a web app where the data and the view are loaded from a remote server. The idea is to have the app load bits of data and interface as it needs, without caching loads of stuff on the client side. Would Trim Junction work well in this environment?

If the aim of Trim Junction is to replicate the functionality of RoR, would it work well in unison with it? Like the scenario I've outlined above.

I intend to have a bit of a play with it. Hopefully I'll have some ideas to contribute after that. Is there a downloadable package?

- Luke Sutton

The best (only) place to look at Junction code right now is either as a "View Source" off of the demo page (TrimJunctionDemo) or in this wiki's integrated source code browser (http://trimpath.com/project/file/projects/trimpath/trunk/junction.js).  Junction right now is a simple single file of about 700 lines of code, but that does not count the JST and TrimQuery components.

The whole "delta" approach to data (using Ajax) is part of the Junction vision.  It's a fun technical challenge with many cases to work out, as with any distributed caching data system.  It gets even better when disconnected, offline operations are added.  But, at this point, it's still vision.  (So, I think now is a good time to contribute, before it gets too ossified.)

Right now, I've started writing a server-side to Junction, and I built it on Rails.  So, Junction is a Rails clone with a server side built on Rails.  Hmmmm.

-- SteveYen


---

Very interesting, Steve, I'll definitely be following your work. I'm sure lots of pepole would be interested in a "gmail-like" interface that could work offline/online (with reasonable limits of course). Any thoughts on going this far?

SteveYen -- yep, that's the goal - a framework that makes it easier for folks to build their own offline "gmail-like" web applications.  They might use it for apps to track things like orders, invoices, time-logs, maintenance records, address lists, etc.


---

This is one of the coolest things I've seen around - you are a real ecmascript expert! Exciting for me is the server side portion - your options seem unlimited! A year ago I'd of said - GO Rhino - but since learning ruby - and rails - I'm not so sure anymore. In any case I'll be looking at NextAction more. Great stuff Steve! PaulVumaska


---

'''UPDATE: Please see the [TrimPath Forum](http://trimpath.com/forum/index.php) for the latest place to discuss stuff.'''


---

We'll keep these community discussion wiki pages around, though, for reference.  -- SteveYen 2005/07/10


---

{ [TrimJunction home](http://code.google.com/p/trimpath/wiki/TrimJunction) | [community](http://code.google.com/p/trimpath/wiki/TrimJunctionCommunity) }