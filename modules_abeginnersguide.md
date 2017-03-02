#Links
[part 1](FreeCodeCamp](https://medium.freecodecamp.com/javascript-modules-a-beginner-s-guide-783f7d7a5fcc#.d5cxrcy81)
[part
2](https://medium.freecodecamp.com/javascript-modules-part-2-module-bundling-5020383cf306#.wqi8y2tlq)
[ES6 - jsmodules.io](http://jsmodules.io/cjs.html)
[ES6 - modules](http://exploringjs.com/es6/ch_modules.html)
[Module Pattern In-Depth](http://www.adequatelygood.com/JavaScript-Module-Pattern-In-Depth.html)
[The Module Pattern](https://carldanley.com/js-module-pattern)

**Good authors divide their books into chapters and sections; good programmers
divide their programs into modules.**

Like a book chapter, modules are just clusters of words. Good modules are
highly self-contained with distinct functionality, allowing them to be
shuffled, removed, or added as necessary, without disrupting the system as a
whole. 

**Common JS**
the one that uses `require` and `module.exports`

synchronously loads code in the order it is declared. This is good for the
server, but not always good for the browser because while a module is loading,
it blocks anything else from running until it's done. 

**AMD**
the one that used `define`. Asynchronous Module Definition. 

Good for browser-first approach. Supports more than just objects as modules. 

Not compatible with io, filesystem, and other server-oriented features. More
verbose. 

**UMD**
Universal Module Definition - used to support both AMD and CommonJS features. 

**NativeJS**
Included in ES6. 

