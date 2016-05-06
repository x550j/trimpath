# TrimPath Coding Standards & Conventions #
  * We do not like TAB! Set TAB characters to 4 hard spaces. Set your editors appropriately.
  * Use relative paths only.  No hardcoded paths.
  * Use Java-style SmallTalkCase or smallTalkCase as appropriate.
  * XML/HTML should use lowercase as much as possible for tag elements & attributes.
  * Don't pollute the global namespace.
    * Only the TrimPath object variable is global.  Example. TrimPath.parseTemplate("hello ${firstName}, welcome");
    * Use anonymous closures to keep private namespaces.
    * Worst case, use a TrimPath prefix.
  * Design and code your JavaScript and CSS to be  [unobtrusive](http://www.onlinetools.org/articles/unobtrusivejavascript/).