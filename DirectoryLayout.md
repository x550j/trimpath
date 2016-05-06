# Directory Layout #

```
/trimpath            -- svn location http://trimpath.com/svn/repo1/projects/trimpath/trunk
  query.js
  query_test.js
  query_test.html
  query_demo.html
  template.js        -- core JavaScript Template (JST) engine.  Has no other dependencies.
  template_test.js   -- unit test that depends on ../jsunit testing framework.  No DOM dependencies in here.
  template_test.html -- includes template_test.js.  Any unit tests that need DOM/DHTML/browser objects go here.
  template_demo.html

/jsunit              -- svn location http://trimpath.com/svn/repo1/projects/jsunit/trunk
                        comes from jsunit.net, unit testing framework for JavaScript in browsers,
                        where the trimpath unit tests (like template_test.html) expects 
                        it to live in ../jsunit
                        
```

The toplevel directories are relative.
  * For example, a user might install the /trimpath directory under http://www.somewhere.com/lib/trimpath or under http://www.somewhere.com/trimpath or under http://www.somewhere.com/lib/js/trimpath, and it should still work.
  * Also, the unit tests (such as template\_test.html) expect the /jsunit directory to be a sibling of /trimpath directory.

The /jsunit directory is a not meant to be enhanced.  If we can't resist and do some enhancement, any bug fixes or improvements to /jsunit must be contributed back to the main project at jsunit.net.