# TrimPath License Discussion #

Is there something better than GPL?  We're not sure yet.  We haven't looked deeply.  We'd rather code. What do you think? -- SteveYen

I feel that LGPL will promote the use of your software much more than GPL -- TariqueSani

I'm now thinking of making JST into a Apache license.  Since I'm the original writer and I'm not a lawyer, I figure I can do this.  But is it a dual license our just a relicense?  -- SteveYen

I found a great explanation of all popular licenses by a lawyer, and he said that whoever is owner can change a license anytime to whatever. So theoretically Linus can change Linux license anytime. So I guess in your case that would be relicensing if you decide to go that way. - Milan


---


LGPL is the way to go!

I suspect the LGPL would best suit you and everyone for the 'components' rather than the GPL. This is because the GPL is too 'viral' to use with javascript libraries: from http://www.developeriq.com/articles/view_article.php?id=162  ''GPLed libraries "contaminate" only software to which they will actively be linked at runtime, not software with which they are merely distributed.''  Javascript is 'linked' in the browser with javascript and html, effectively meaning that your library cannot be used for any project that is not itself GPL.

The GPL makes sense for an application such as Next Action - if anyone makes changes to the application they have to publish those changes, and their changes must be under the GPL.

The Apache licence is possibly not what you want: I think it allows anyone to use your source code without giving back any enhancements that they may make to it (e.g. Microsoft used the BSD TCP/IP protocol stack code (BSD licence, similar to Apache) and didn't give back any improvements that they made.)

You can release new versions under a new licence, but once other people have submitted changes without assigning the copyright to you, they may have to be consulted also (or their changes removed). So Linus can change the licence for source code he has written, but cannot make that decision for Linux, because all the rest of the source code is copyrighted by thousands of different individuals. Basically you need to choose your licence early on, because effectively you may not be able to change it later if there are large contributions from others.
Of course all the above is based on what I have read, I am not a lawyer, so could be wrong.

-- Morris


---
