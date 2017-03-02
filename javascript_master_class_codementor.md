# Major Links
[Class site with outline and links to videos](https://www.codementor.io/classes/4874865172/students)
[Live-Sessions Sign-On](https://www.codementor.io/classes/4874865172/students)
[Slack](https://codementor-classes.slack.com/messages/js-class/)
[Test boilerplate](https://github.com/joshdmiller/js-test-boilerplate)
[Class code](https://github.com/joshdmiller/js-live-class-code)
[Vim config](https://github.com/joshdmiller/vim)

# Class 1
Use Slack #js-class for in-class chat and questions
Use Slack #js-qna for questions between classes

Activities are like homework. You can submit a PR for him to see your solution and give feedback. 

Class code is on Github

Josh will post link to his Vim config file

cool site (?) [http://vimcasts.org](http://vimcasts.org)

NERDtree for file explorer in Vim.

JS Frameworks for comparison
* React
* Angular
* Vue
* Ember
* Feathers

uses `babel-register` to transpile ES6 code when *not* using webpack

cool comparison of forEach, .map, and .map with ES6 for refactoring/simplifying 

lost me at the end about writing a new test after refactorying. Check it out in the video. 

[Activities for Class #1](https://paper.dropbox.com/doc/JS-Live-Class-Day-1-Activities-B9eJTVFBR6yTAuwGLwrKy)

**Given-When-Then**
Given a set of circumstances, when something happens, then I expect something...

i.e. double(), given the double function, when given '4', we expect a returned value of 8. 

##Notes from Activity #1
[What every unit test needs](https://medium.com/javascript-scene/what-every-unit-test-needs-f6cd34d9836d#.phct5ffd5)
A test should:
* Be a design aid. Write tests before implementation
* Document features. Written as a clear descript of the feature being tested.
* Halt the delivery pipeline on failure and produce a good bug report when they fail.

**Good Bug Reports**
* what were you testing
* what should it do
* what was the output (actual behavior)
* what was the expected output (expected behavior)?

[Why I Use Tape Instead of Mocha and So Should You](https://medium.com/javascript-scene/why-i-use-tape-instead-of-mocha-so-should-you-6aa105d8eaf4#.kx94mbv7v)
If you have to mock a bunch of stuff to test your code, it's not modular enough. 

**Tight Coupling** is the opposite of modular.

If you spend a lot of time on mocks and stubs, that's a strong code-smell. You can probably dramatically simplify both your tests and your application by breaking you app into more modular chunks. A few simple mocks here and there are okay.

STick to equal, deepEqual, pass, and fail for your tests. 

See example of how to replace before/after/beforeEach/afterEach

Testing I/O dependent code will cause problems. Functional, or E2E tests are often better for situations when dealing with databases, network I/O, user I/O, etc. 

Healthy test suites will recognize that there are three major types of software tests that all play a role, and your test coverage will create a balance between them. [link to article](https://www.sitepoint.com/javascript-testing-unit-functional-integration/). Types = unit, integration, functional

Good examples of each, especially of functional tests via Nightwatch.js


#Class 2
Will be writing code 'the wrong way' this class and then going back fixing it piece-by-piece in following classes. 

Check to see if DOM has loaded before running JS:
```
if (document.readyState !== 'loading) {
  onReady(); // a function that you define elsewhere containing your app code
} else {
  document.addEventListener( 'DOMContentLoaded', onReady() );
}
```

**BEM - Block Element Modifier**
a naming convention in CSS. e.g. `.todo--complete`


[Activities for Class #2]()



