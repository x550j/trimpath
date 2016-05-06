# TrimJunction Controller #

{ [Junction home](http://code.google.com/p/trimpath/wiki/TrimJunction) | [documentation](http://trimpath.googlecode.com/svn/trunk/junction_docs/index.html) }

Junction Controllers are made up of one or more actions that performs its purpose
and then either renders a template or redirects to another action.
An action is defined as an instance method on the Controller.
A sample controller could look like this:
```
GuestBookController = function() {
    this.index = function(req, res) {
        res.entries = Entry.findAll();
    }

    this.sign = function(req, res) { 
        Entry.newInstance(req['entry']);
        res.redirectToAction('index');
    }
}

GuestBookController.layoutName = 'main';
```

Alternatively, if you want to use a prototype based programming idiom...
```
GuestBookController = function() {}

GuestBookController.prototype.index = function(req, res) {
    res.entries = Entry.findAll();
}

GuestBookController.prototype.sign = function(req, res) { 
    Entry.newInstance(req['entry']);
    res.redirectToAction('index');
}

GuestBookController.layoutName = 'main';
```

All actions assume that you want to render a template matching the name
of the action at the end of the performance unless you tell it otherwise.
The index action complies with this assumption, so after populating the res.entries
instance variable, the GuestBookController will render the "/app/views/guestbook/index.(jst|est)" template.

Unlike index, the sign action isn’t interested in rendering a template.
So after performing its main purpose (creating a new entry in the guest book),
it sheds the rendering assumption and initiates a redirect instead.
This redirect will take the user to the index action.

The index and sign methods represent the two basic action archetypes used
in Junction Controllers. Get-and-show and do-and-redirect.
Most actions are variations of these themes.

## Requests ##
Requests are processed by the Junction Controller framework by
using the actionName parameter to the TrimPath.junctionClient.get/post() call.
This value should hold the name of the action to be performed.
Once the action has been identified, the remaining request parameters,
the session (if one is available), and the full request with all the
http headers (if available) are made available to the action through
the request (or req) object. Then the action function is performed.

The full request object is available with the req (or request) parameter.
Request parameters can be accessed through a property lookup, like this:
```
  this.hello = function(req, res) {
    var incomingMessage = req["msg"];
    res.renderText("Hello stranger who said: " + msg);
  }
```

All request parameters whether they come from a GET or POST request, or from the URL,
are available through the req (or request) hash.

It’s also possible to construct a multi-level, nested request parameter hashes by specifying keys using brackets, such as:

```
  <input type="text" name="post[name]" value="david">
  <input type="text" name="post[address]" value="hyacintvej">
```

A request stemming from a form holding these inputs will include { post: { name: "david", address: "hyacintvej" } }.   If the address input had been named "post[address](address.md)[street](street.md)", the params would have included { post: { address: { street: "hyacintvej" } } }.
There’s no limit to the depth of the nesting.

## Responses ##

Each action results in a response.  The actual response text is generated through the use of renders and redirects.

## Renders ##

Junction Controller sends content to the user by using one of five rendering methods.
The most versatile and common is the rendering of a template, such as an [JST](http://code.google.com/p/trimpath/wiki/JavaScriptTemplates) or [EST](http://code.google.com/p/trimpath/wiki/EcmaScriptTemplates) template.
The controller passes objects to the templates by assigning properties in the res (or result) object:
```
  this.show = function(req, res) {
    res.post = Post.find(req['objId']);
  }
```

The properties set on the res hash are automatically available to the template:
```
  <h2>Title: ${post.title}</h2>
```

You don’t have to rely on the automated rendering.
Especially actions that could result in the rendering of different templates
will use the manual rendering methods:
```
  this.search = function(req, res) {
    res.results = Search.find(req['query']);
    if (res.results.length == 0)
      res.renderAction("noResults");
    else if (res.results.length == 1)
      res.renderAction("show");
    else
      res.renderAction("showMany");
  }
```

Read more about writing [JavaScript Templates](http://code.google.com/p/trimpath/wiki/JavaScriptTemplates) or EcmaScriptTemplates.

## Redirects ##

Redirecting is what actions that update the model do when they’re done.
The savePost method shouldn’t be responsible for also showing the post
once it’s saved — that’s the job for showPost. So once savePost has
completed its business, it’ll redirect to showPost.

## Calling multiple redirects or renders ##

An action should conclude by a single render or redirect.
Attempting to try to do either again will result in the last redirect taking precedence,
and then the last render taking next precedence.
```
  this.function = doSomething(req, res) {
    res.redirectToAction("nothere");   // This one is second place priority.
    res.redirectToAction("elsewhere"); // This one wins.
    res.render("overthere");           // This one is last place priority.
    res.render("overthere");           // This one is third place priority.
  }
```

If you need to redirect on the condition of something, then be sure to use a "return" statement to halt execution.
```
  this.function = doSomething(req, res) {
    if (req['monkeys'] == null) {
      res.redirectToAction("elsewhere");
      return;
    }
    res.render("overthere"); // Won't be called unless monkeys is null
  }
```


---

'''Acknowledgements''': this page was ported from the ActionController::Base API documentation from the [Ruby on Rails](http://api.rubyonrails.com/) framework.


---

{ [Junction home](http://code.google.com/p/trimpath/wiki/TrimJunction) | [documentation](http://trimpath.googlecode.com/svn/trunk/junction_docs/index.html) }