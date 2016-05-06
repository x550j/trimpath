EcmaScriptTemplates (EST) is an alternative template markup syntax and template processing engine that Junction supports.  Just use an '.est' suffix on your template files instead of '.jst', and Junction knows automatically to use the EST engine.

See: http://erik.eae.net/archives/2005/05/27/01.03.26/

In your Controller code, by the way, you don't mention either '.jst' or '.est'.  Junction figures out the right template processing engine at runtime.

For example....
```
BlogController = function() {}

BlogController.prototype.show = function(req, res) {
  res.blog = Blog.find('first', req['objId']);
  res.render('blog/show'); // We do not use 'blog/show.jst' or 'blog/show.est'
}                          // as Junction will automatically look for either one at runtime.
```

Although you're allowed to mix and match between, JST and EST files, with some templates being foo.jst and other templates being bar.est, we recommend that all new applications should just pick one template markup syntax and stick with it.

Between the two, we've found that EST is easier for folks to pick up and learn, because it's so similar to JSP and ERb (rhtml) syntax.