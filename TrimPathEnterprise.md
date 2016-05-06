# Introduction #

Principle 1:  TrimPath
  * eschew server-side UI technologies
  * de-couple/separation of UI from server
  * demisauce pattern

Principle 2: Enterprise
  * Java
  * Jboss
  * Services,  rather than binding UI to services on server-side


# Details #

Related to [TrimPathCore](TrimPathCore.md)

Why focus on Spring if the idea is to separate UI from server?  Because we want to get paid,  and we want to focus on the right things on the server,  services,  not JSF or SEAM or other large investments that focus on binding UI with services.  We need portable UIs.  My UI should easily go from a loosely dropped in a portlet or even to a static page -- no serverside processing.

**Future Junction package notes:  jQuery is made use of extensively for glue utilities.  Any lightweight version of Junction should not rewrite the wheel.  There is some use of myLib for cookies,  even though myLib predicts the doom of jQuery.  Essentially package and use what has been proven to work so others can see those libs in the package.  Firebug and other firefox tools used extensively also,  don't try to replicate those.**

## Actual Track Record ##

1. Successful production deployment of front end status reporting ajax portlet for Massive Legal Dept Enterprise App.

2. Successful production deployment of another ajax portlet,  a policy viewer and exporter that governs the movement of Enterprise Lifeblood Records.


gol dern it mootools sucks.  more later...  do we really need to write all this incompatible crap that alters prototypes of native classes?  trimpath query does a little bit.  now I can't use sexyforms.