# Popularity #

Or, How To Make A Successful Open Source Platform

I just wanted to record some ideas about the 'soft' aspects of growing a successful software platform.  This includes social dynamics, politics, marketing, evangelism, docs, people, humanization; and, not really much about coding.

There's no pat instruction manual, just more of a reading list / link dump.

Some themes...

## Simplicity ##

  * Anything written by [Adam Bosworth](http://www.adambosworth.net) is a good start.
    * Especially his messages on [KISS](http://www.adambosworth.net/archives/000031.html).
    * [The Talk On Messy, Simple, Flexible, Sloppy](http://www.adambosworth.net/archives/000031.html)
      * [Tyranny of the geeks](http://dotnetjunkies.com/WebLog/sriram/archive/2004/11/18/32707.aspx#FeedBack)
    * [Evolution in Action](http://www.adambosworth.net/archives/000028.html)

  * Anything by Clay Shirky is great food for thought.
    * The Everyman Web Developer, or even better -- folks who don't consider themselves developers, should be able to succeed with the platform. That demands simplicity.
    * [In Praise Of Evolvable Systems](http://www.shirky.com/writings/evolve.html)
      * Will a centrally planned, overdesigned crystalline castle of a platform ever succeed?
      * Shirky argues that the messy, community-evolved, ugly-but-works, [View-Sourcable](http://www.shirky.com/writings/view_source.html), simple systems are inevitably invincible.
    * [Situated Software](http://www.shirky.com/writings/situated_software.html) is what Web 2.0 might be about.

## Useful In 10 Minutes ##

  * [Why Hibernate Is Successful](http://www.hibernate.org/38.html) is a good list.  I highlight below what I consider the true differentiators.
    * rapid release schedule
    * regression tests
    * '''do one thing well'''
    * '''avoid over-design'''
      * Aiming for too much abstraction and flexibility at an early stage is a great way to waste time that could be better spent solving actual problems that your actual users are facing. Do the simplest thing that can possibly work. Don't try to solve problems that your users don't care about. And it DOES NOT matter if your implementation is inelegant, at least initially. What matters is delivering useful functionality in a timely manner.
    * a central vision
    * documentation
    * avoid standardsism
    * '''up and running in ten minutes or less!'''
    * developer responsiveness
    * easily update-able wiki pages

  * The [Ruby On Rails (RoR)](http://www.rubyonrails.com) framework makes a similar story.
    * This is a terrific server-side web application framework.  I just started getting into a month ago, but it's really nice.
    * The buzz is that it was not so much top-down spec'ed and designed, but more '''refactored and extracted''' from an existing web application ([Basecamp](http://www.basecamphq.com)).  You can really tell, because what emerged from that effort (or what remained as RoR) is just the stuff you need and use.  This approach produces a result that's provably useful, productive, and feels wonderfully right.
    * RoR is destined for quick and high adoption, with a popularity growth curve that other project leads are rightly jealous of.
    * The RoR project lead reprises nearly all of the same points of stewardship as that of Hibernate.  But, he added modern twists like blogs, screencasts, and workshops.  And, he leveraged the underlying language Ruby really well.
    * http://www.blueskyonmars.com/archives/2005/03/23/ruby_on_rails_wins_the_marketing_war.html

Other thoughts:
  * No recompilation.  Take the compile out of the edit-compile-run-debug cycle.  It's all about instant gratification, usually through interpreters.
    * Similar to PHP, RoR has no compile-redeploy step.  Contrast that to J2EE.
  * Partial results and fast progress.
    * Postels law(?): be forgiving in the input you accept.  Even if the user types 

&lt;Table&gt;

<tr>

<TD>

hi<br>
<br>
Unknown end tag for </TABLE><br>
<br>
, it works, as best it can.</li></ul></li></ul>

Dojo versus Prototype:<br>
<ul><li>If thrown against the ideas listed above, I'd have to give the nod to Prototype -- it's simpler, more approachable, more obvious, view-sourcable.</li></ul>

<hr />
Those are the key pointers.  The most important takeaway lesson for me: build a real app first.<br>
<br>
<hr />
Less important links below:<br>
<br>
Tim Bray writes of <a href='http://www.tbray.org/ongoing/When/200x/2004/01/03/TPM1'>technology success predictors</a>.<br>
<br>
Tim O'Reilly and Web 2.0<br>
<ul><li><a href='http://www.oreillynet.com/pub/wlg/3017'>Architecture of participation</a>