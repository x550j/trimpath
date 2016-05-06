# Follow A Request Through Junction #

{ [Junction home](http://code.google.com/p/trimpath/wiki/TrimJunction) | [documentation](http://trimpath.googlecode.com/svn/trunk/junction_docs/index.html) }

For web application developers, let's follow a request through the [Junction](http://code.google.com/p/trimpath/wiki/TrimJunction) Model-View-Controller framework.  That is, what happens when a user clicks on a hyperlink in a Junction application...

If you've seen a .jst ([JST](http://code.google.com/p/trimpath/wiki/JavaScriptTemplates)) file in a Junction app, you've probably see lots of calls to the linkToLocal() function.  For example...
```
<div>
Click ${linkToLocal('here', 'blog', 'index')} to see your blog entries.
</div>
```

When the JST is processed, it will be converted into HTML.  We'll talk more about template processing later, such as why and when it occurs in the Junction framework.  But, for now, during template processing, the JST template engine will invoke the linkToLocal() helper function.  The linkToLocal() function, which is part of the Junction API, will return an 

&lt;A&gt;

 hyperlink/anchor tag, so the resulting HTML will look like...
```
<div>
Click <a href="?controllerName=blog&actionName=index" 
         onclick="return TrimPath.junctionClient.get('blog', 'index')">here</a> 
to see your blog entries.
</div>
```

So, when the user clicks on the link, because of the onclick event handler, the TrimPath.junctionClient.get() function is called, with a controllerName parameter value of 'blog' and an actionName parameter value of 'index'.

ASIDE: The function is named TrimPath.junctionClient.get() to remind you that it should mimic a HTTP GET request.  There's another function called TrimPath.junctionClient.post() which, unsurprisingly, should mimic a HTTP POST request.  We say 'mimic' because all processing is happening locally in-browser, where no requests are actually made to the web server.

## The Controller ##

The TrimPath.junctionClient.get() function will next look for a global constructor function named BlogController.  This BlogController constructor function is supplied by you, the app developer.  Junction uses naming conventions to convert from the 'blog' string parameter value to BlogController.  If the controllerName parameter, for example, instead had the value of 'invoiceStatistics', then Junction would use the InvoiceStatisticsController global constructor function.

Here's what your BlogController constructor function might look like...
```
BlogController = function() {
    this.index = function(req, res) {
        res.blogs = Blog.findActive('all');
    }

    this.show = function(req, res) {
        res.blog = Blog.findActive(req['objId']);
    }

    this.ping = function(req, res) {
        res.renderText("hello: " + new Date());
    }
}

BlogController.layoutName = 'main';
```

In the above simplified example, the BlogController has three actions defined: 'index', 'show' and 'ping'.

Junction uses the actionName parameter to determine the action method which it will call.  In our example, the actionName was 'index', so Junction will create a new instance of BlogController and call that instance's 'index' function, with request and result objects as parameters.

ASIDE: Each invocation of TrimPath.junctionClient.get/post() will create a brand new controller object.  These controller instances are not reused between requests.  The reason why is we want to mimic server-side behavior, so that your BlogController code will behave the same, whether it runs inside a browser or on a server.

At this point, our BlogController.index() function now has the first crack at handling the request.

The req (or request) object holds parameters, such as req['blogTitle'], req['invoiceId'], req['productName'], req['objId'].  In this example, there aren't actually any request parameters.

The res (or response/result) object holds a bunch of API helper functions and is used during result generation.  The res object also becomes a holder or context where you can place objects that are used during template processing.

In the above example, you can see we're adding a blogs Array object to the res object, with "res.blogs = Blog.findActive('all')".  In any templates, we can then refer to the blogs Array object.

## The Model ##

What's going on with the Blog.findActive('all') call?  It's a function that needs to return an Array (possibly empty) of Blog instances. How was Blog.findActive() written?

Here's one example definition of the Blog model...
```
Blog = function() {}

modelInit('Blog');

Blog.belongsTo('Category');
Blog.belongsTo('User');

Blog.hasMany('Comments');
Blog.validatesPresenceOf('title');
```

The call to modelInit('Blog') tells the Junction framework to automatically decorate the Blog function with "class" functions like find(), findActive(), findBySql(), newInstance(), and many more.  So, there's very little work for you to do.

The automatically generated Blog.findActive() function will do a SQL query against the Blog table.  For example, if you call Blog.findActive('all'), the SQL query would look like `SELECT Blog.* from Blog WHERE Blog.active = 1`.  The method also converts the resultset into an Array of Blog instances.

The SQL query is next executed on an RDBMS.  But, which RDBMS?  The answer depends on where you're running your application.  If your application is running in a server-side web application server, the SQL query is executed against a server-side RDBMS.

If your application is running in a web browser client, and you have Google Gears installed, the SQL query is executed against the browser-local Google Gears RDBMS (sqlite).  If Gears isn't available, Junction automatically executes the SQL query against an in-memory, browser-local RDBMS provided by the TrimQuery engine.  When running in the browser, no requests are made to a remote database server to service a query.

ASIDE: Something else going on in the above Blog model definition is the that we're declaring information that Junction can use to automatically generate even more helper functions.  Although we won't explore these now, here's what the declarations are saying:
  * A Blog entry might belong to a Category and to a User.
  * A Blog entry might have many Comment records.
  * And, a Blog entry must have a non-empty title field.

## The View ##

After Blog.findActive('all') returns, we're back in our BlogController.index() function...

In our BlogController.index() function, because we have not explicitly called res.render(), res.redirectTo(), or other functions like those, the Junction framework will now automatically look for a template file to render, such as "/app/views/blog/index.jst" or "/app/views/blog/index.est".

ASIDE: As a counter example, the BlogController.ping() action function explicitly calls res.renderText().  So, during requests to BlogController.ping(), the Junction framework will know NOT to bother looking for and processing a template file.

Imagine that "/app/views/blog/index.jst" exists.  Here's what that template might look like...
```
<div>
  <h2>Your blogs:</h2>
  <h3>You have ${blogs.length} blog entries.</h3>
  <ul>
    {for blog in blogs}
      <li>${linkToLocal(blog.name, 'blog', 'show', blog.id)}</li>
    {/for}
  </ul>
</div>
```

As you can see, the template is referring directly to a blogs Array object, which holds the results of the Blog.findActive('all') call.  The template will show the number of blog entries and hyperlinks to all the blog entries in a nice list.

Notice, however, that the index.jst template is just a fragment of HTML.  Where are the open and close 

&lt;html&gt;



&lt;body&gt;

...

&lt;/body&gt;



&lt;/html&gt;

 tags?  The answer is with a feature called 'layouts'.

Because the BlogController has a layoutName that is set to 'main', the Junction framework will next load and render a template named '/app/views/layouts/main.(jst|est)'.  (Otherwise, Junction will just use a default layoutName of 'default'.)  All layouts are stored in the /app/views/layouts' directory.  The layout template provides the surrounding 

&lt;html&gt;



&lt;body&gt;

 tags and other common UI headers and footers.  In short, a developer using Junction can control the response output, then, in detail all the way from the 

&lt;html&gt;

 start tag to the 

&lt;/html&gt;

 close tag.

The final result of layout rendering is now displayed by the browser.

Finally, you may have also noticed, in order to generate the hyperlinks, the template uses the linkToLocal() function.  This brings us back to the start and completes this article.


---

{ [Junction home](http://code.google.com/p/trimpath/wiki/TrimJunction) | [documentation](http://trimpath.googlecode.com/svn/trunk/junction_docs/index.html) }