# Express JS
[Express Basics - Treehouse](https://teamtreehouse.com/library/express-basics)

### Tools
**nodemon**
* runs dev server
* is a wrapper around the 'node' command and package
`node <path>`
example:
`node src/app.js`

**node inspector**
* allows debugging in browser devtools
* support is built in as of v6.3.0
`node --inspect <path>`
example:
`node --inspect index.js`
You will see a URL starting with `chrome-devtools://...`. Copy/past this into Chrome to use. 

**breakpoints**
When debugging in the browser, adding breakpoints gives access to variables inside the console. This way you can see what you have and then test more calls on those objects/values in the console before writing it into the code. 

To run inspector  with a breakpoint set before the first line of code:
`node --inspect --debug-brk <path>`
Then use the play button in the browser to step through the code and advance to the next break point

### Middleware
Stuff you do between receiving the request and sending a response. 

### Rendering
You can include an object to pass to the rendering template using the .render() method.
`res.render('index', {'posts', posts});`
or use the 'dot' syntax:
`res.locals.posts = posts;` <- same as above. 

### Patterns
**Converting JSON to an array**
```javascript
// assuming a JSON object 'posts'

Object.keys(posts).map(function(value) {
	return posts[value];
});
```
