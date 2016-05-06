# Introduction #

Enterprise tested Query and JST modules.  Plus some extra utilities.

A few people have done memory allocation experiments in browsers finding that there is at least 20 meg at your disposal for javascript.  That's a lot of room to play with regular Trimpath Query before you would need a storage solution (gears).

# Details #

TrimPath Core was started from [issue 31](https://code.google.com/p/trimpath/issues/detail?id=31):
http://code.google.com/p/trimpath/issues/detail?id=31#c0

You can find dev and minified uploads there.

Notes:
possible avenues for improvement (templating costs most to do)
- incorporate code from higher performing JST implementation that was circulating in forums
- div pools to reuse somehow for templating
- try some stress tests,  possible to free up allocated divs when replacing div,  or is it automatic,  or would it be a leak if you do a lot of DOM templating?

To-do:
make trimpath core upload
make ajax helper upload
make branch for core and helper (to support various export formats and uses,  ongoing).

Main tool of [TrimPathEnterprise](TrimPathEnterprise.md),  enterprise applications detailed there.