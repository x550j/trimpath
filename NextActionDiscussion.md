# Next Action Discussion #


---

'''UPDATE: Please see the [TrimPath Forum](http://trimpath.com/forum/index.php) for the latest place to discuss stuff.'''

We'll keep these community discussion wiki pages around, though, for reference.  -- SteveYen 2005/07/10


---

This is an open wiki page to discuss the [Next Action](http://code.google.com/p/trimpath/wiki/NextAction) tool.  Looking for your rants, feedback, questions, ideas, suggestions... -- SteveYen


---


I know this product is still beta but you've inspired a couple of us over in the mozilla IRC chanels.  And I'm going to try and describe what came up, and see what you guys can tell us about the possibilities.

Firefox lacks a really good schedualing app at the moment, you guys are building something along those lines.  We're interested to see if nextAction could be hosted on a local machine via a Firefox Extension (as the the frontend code apears to be mostly JS this at least should work) and secondly if nextAction could be poked to export the data being stored for the events into an xml file. The eventual idea would be that the user could install the extension, and have a robust, configurable and simple Calander like aplication built into firefox, that would provide dynamic warnings etc on upcoming events via RSS.

Then to top it off we could patch into the fireftp code (ftp client for firefox) and have the extension upload the file(s) to a server, people outside of the user then could subscribe to rss feeds from him (based on catagories, or the full feed) and recieve updates in their own install from our originating user.  This could be used to synchronize meetings, or organize projects, family apointments, etc.

Anyways, we think the possibilities are incredible, and the ideas that keep coming up tell us that we're on to something.

So ... We're all interested, 1) how does the nextAction backend work?, can it be poked to export to xml like this? Is what we're thinking possible from nextAction assuming that it can be (at least sort of) imported into a firefox extension?

wow,
Anders


---


Thanks for the kind feedback.  Helps to inspire the next jaunt into 3AM hacking.  Everything you say is technically possible.  (After all, the world's largest JS codebase is Mozilla/Firefox itself.)  NextAction was meant to be locally used, designed for offline access.  Throwing it into an extension (especially to get at UniversalBrowserKungFu security priviledges) is a great idea.

For question (1), please take a look at TrimJunction for more about NextAction's underlying framework and we can go from there.  I wholeheartedly admit its radically underdocumented.  So, more docs should be forthcoming soon, once I get used to JSDoc.

And, I think NextAction is definitely extendable to export to XML.  It seems [Danny Ayers has already done RDF/XML!!](http://dannyayers.com/archives/2005/07/08/nextaction-last-actions/).

-- SteveYen


---


Steve, I've been reading (and reading and reading) and I have a few questions that you can probably answer quickly.  First you mention bringing NextAction to the local user "NextAction was meant to be locally used, designed for offline access", but with the ties that TrimJunction has to SQL how does this work?

Well, I guess that's my most important question atm. I'll have more to post at a later date.

Thanks,
Anders

There's a SQL engine written in JavaScript.  See TrimQuery.  Records are brought from the server in JSON format.  So any SQL queries run completely in the browser.  See http://trimpath.com/blog/?p=36.  And, TrimJunction / TrimQuery does this in a way so that if you do 'File->Page Save As', it will all save correctly and will run locally correctly, without needing to talk to the server anymore.  If you make local, offline record updates/deleted/insertions, you can still do 'File->Page Save As' again and again to save the changes locally.  The thing with UniversalBrowserAccessDaFilesystem rights is that this can be made automatic, so that folks don't even need to do a 'File->Page Save As' anymore.

Things that pop to mind -- will it scale for your needs?  TrimJunction (the MVC framework) and TrimQuery (the SQL query engine) were envisioned for local caches of thousands of records and small databases (like for a personal todo-list manager).  A calendaring app probably has many many orders of magnitude more records over time than a personal todo-list manager.  That's why even Outlook needs to archive and compact old calendar entries.  When I coded all this up, I envisioned large databases staying on the servers and sync'ing up the client's (much smaller) local caches.

-- SteveYen


---


Hi,

NextAction looks useful. When I saved the page linked to on http://trimpath.com/project/wiki/NextAction, (i.e. http://trimpath.com/demos/nextaction_static1/nextaction.htm) I get a page with many existing actions, projects etc. I don't see a way to download a fresh copy or easily delete all of the actions, projects etc from the page I have. This is not very convenient. With so many ways of managing todo's etc out there it may be easy for many to overlook NextAction if there is no better way to start out. Perhaps you don't mind this, I don't know.

-- HLeeney

Hi, on the most recent versions of NextAction, just click on the 'about' page in the top-right corner of the page and look for the 'Delete All Records' button.  -- SteveYen


---

This looks like a technology with a lot of promise. I just wish I could make it work for me. I can use the NextAction page perfectly when I am using the version at trimpath. It seems to work as I would expect. When I save it, I can bring up the page as it appears at trimpath, but if I click on anything, I receive the message "Connection Failure Error - The connection was refused when attempting to contact localhost:4000." I am using the latest version of Firefox (1.0.4) and even tried it in safe mode. I am using other JS applications, like TiddlyWiki without any problems. Sorry to bother you with this trivia, but if I experienced this, my guess is that others will also. Thanks for your help and such an interesting piece of software.

Tom

Hi, I've heard of this twice now from folks.  I'm using Win2K.  Are you on Windows XP?  Just trying to figure out the pattern... -- SteveYen


---

Steve,

Thanks for answering. Yes, I am using Win XP Pro SP2

Tom


---

I'm also using Windows XP Pro SP2 and am having a different problem.  I can save your starter file to my hard drive.  I can open it then add and delete contexts, actions and projects and then I save my work.  However, when I reopen the file from the location on my hard drive all of your default data is back and my changes are lost.  I can't seem to figure out what I'm doing wrong.  Would appreciate any help as this looks like a very useful app.
Thanks,
-Michael


---


Regarding ""Connection Failure Error - The connection was refused when attempting to contact localhost:4000."
I had that too. In Windows, try saving a fresh copy and make sure you're saving as "type: web page (complete)"

For Michael, before starting to use your NextAction file, make sure all your files don't have the properties of "archive" or "read-only".  If they do, uncheck, open the nextaction.htm file, do a save, and check properties again.  You might have to do this more than once until it "sticks".

David


---


I experienced similar problems with "Connection Failure Error ..." when I was trying to get NextAction to work. It was because I was trying to save the page by right clicking the link and choosing 'save link as'. When I navigated to the page, went to the menu and clicked 'File' - 'Save Page As' ... It worked fine for me.

--HLeeney


---


I am sure you have good reasons for choosing a wiki page Steve. From the point of view of user problems etc I would think that a forum/board would be better for all. Already the above page is a little messy. If (more likely when) more people start using it, it will become difficult to follow the thread of a discussion. Questions and answers will get intermingled unless there are strict rules as to how to edit the page. This has already happened above with some answers to questions being put in as part of the original questions section and some answers being put in as new sections. Hope this is useful. It will become harder to find answers to questions that have already been asked. It is great work regardless! Thanks.

--HLeeney

Good idea.  The [Forum](http://trimpath.com/forum/index.php) is online now. -- SteveYen


---


Can I put the file on my webpage so I have remote access and can I view the page on my cell in sms? It would be outstanding if I could then I would be connected all the time to my GTD system without paper.

Ben

Hi, I saw you put up the same question in the [TrimPath Forum](http://trimpath.com/forum/index.php).  The short answer is yes, but you'll need to write your own server side code.  Or, you can wait for me to finish my server side stuff, adding lots of encouragement. -- SteveYen

Steve: I just posted basically the same question in the forums, then saw this, I have no problem writing server code, but I couldn't find the post you're referring to (Ben's post) - how does the client need to be modified to do this?
-brian


---

I create an toplevel context with sub contexts and another top level context. If I edit the first context to become a sub  context all my sub contexts for all top level contexts disapear.
Thanks this is a great app.
Gerhard

Thanks: yes, it's a bug.  sub-sub-Contexts aren't displayed correctly.  And, it might be awhile before there's a fix, handling nested trees in SQL.  See the related forum topic: http://trimpath.com/forum/viewtopic.php?pid=161 and http://trimpath.com/forum/viewtopic.php?id=18 -- SteveYen


---

'''UPDATE: Please see the [TrimPath Forum](http://trimpath.com/forum/index.php) for the latest place to discuss stuff.'''

We'll keep these community discussion wiki pages around, though, for reference.  -- SteveYen 2005/07/10


---
