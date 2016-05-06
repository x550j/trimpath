# Ruby Versus JavaScript #

The JavaScript language and Ruby are very similar.
  * You might say JavaScript is very much like Ruby Lite, but with the syntax of Java.
  * Or, you might say Ruby is very much like JavaScript 3.0, but without all the punctuation.

Here's some of what JavaScript can't do, compared to Ruby...
  * Natural looking attribute getters and setters, like for @comment.post = @post
  * Magic method\_missing, to allow for dynamically, on-demand-generated findByFooAndBlah() runtime methods.
  * Concise code blocks.  Instead, you got to pass around closures.
    * See ruby.js (google for it), which attempts to make JavaScript behave like Ruby.
  * Embedded text
  * ...and probably much much more

On the other hand, JavaScript has a few things going for it, too.
  * It's widely deployed, in every browser, making eval() available to the world.
  * JavaScript is inherently [View Sourcable](http://www.shirky.com/OpenSource/view_source.html).
  * It's runnable on a server, too -- with the Rhino Java implementation of JavaScript.
    * Sadly, though, not at TextDrive, which doesn't support Rhino/Java.
  * It's got more bookshelf real estate right now than Ruby.
    * So it's easy for you to learn JavaScript in 21 days!
  * More folks have (blindly) copy-&-pasted snippets of JavaScript than snippets of Ruby.
  * Brendan beats Matz > 9 to 1 in Google Smackdown.
    * But, some folks would say, like with comparing lines of code measurements, that it does not matter much.
  * And, most importantly, this is the secret marketing weapon: it's got ''Java'' as part of its name.

Although I've only picked up Ruby for 2 months now and I'd rather now program in Ruby if I could, having JavaScript be the world most widely deployed scripting language is not a terrible situation.  After all, it has eval(), closures, and prototypes.  It could have been worse (and we all know worse is better).  -- SteveYen