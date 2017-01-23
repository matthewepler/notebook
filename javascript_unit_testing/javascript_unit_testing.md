# Javascript Unit Testing
[Treehouse](https://teamtreehouse.com/library/javascript-unit-testing)
Follows a BDD approach for Unit Testing
Uses [Mocha](https://mochajs.org/) and [Chai](http://chaijs.com/guide/installation/)


## Test Suites
Def: a specific group of related tests. NOTE: some people use 'test suite' to mean different things. 

Mocha runs the tests. Chai is an 'expectation library' that throws Errors when conditions aren't met.

One great reason for writing tests is that it can help explain what your code actually does.
 

## Types of Testing
**Unit Testing**
Writing tests that confirm an individual function or piece of code works. 

**Integration Testing**
Used when you're adding new code to existing code to make sure everything runs together without breaking. 

**End-to-End Testing**
Running a full application and that all details of deployment do not break what is already working. 


## Behavior-Driven Development
Not a type of test, but an approach to building software. 

Write tests first, then code.

**"Red Green Refactoring"**
* write the tests, even though they'll fail
* write the easiest code we can that passes the test
* go back and improve the passing code for readibility and performance.


## Running tests with Chai 
Chai is an assertion library that checks values with the expect() function.

You can run tests within the same file as your code with just Chai.
**Running Tests with Chai**
Install Chai:
`npm install chai --save-dev`
Create the JS file for test code. E.g:
`touch textUtilities.js`

Include methods from Chai:
```javascript
var expect = require('chai').expect
```

Then add some expect methods. E.g:
```javascript
expect(true).to.be.true;
```

You can run the code and tests (in this case all we have is a test so far) using plain-ole node:
`node titleUtilities.js`

Now make up an expect function for something you want to build:
```javascript
expect(titleCase('the great mouse detective')).to.be.a('string');
```

Run the code and you should get RED results (i.e. errors). 

In this case, the titleCase func doesn't even exist yet. We are just outlinging what we want to see happen. So running this (no Mocha so far so you can just use `node textUtitlities.js`) will throw an error. 
Now write the function:
```javascript
function titleCase(title) {
	return title;
}
```

Run the code and you should see GREEN results (i.e. no errors)

Now write a new expectation for the next bit of functionality you want to see. In this example, we want the function to return the given string with a capital letter for each word (hence the name titleCase).
```javascript
expect(titleCase('the great mouse detective').to.equal('The Great Mouse Detective'));
```

Run it and you'll see red (get errors) because the function doesn't return title-cased strings yet. So add that and run it again until you get green. 

And so on and so on...

**However**
Jumping to the expected result like that is not recommended. Instead, it's best to write tests that build up to the final result. 

For this example, you might write more granular tests to:
* return a single letter to uppercase if passed a single letter. 
* Then another expectation that turns the first letter of a word to uppercase. 
* Then another that splits the string at the spaces and iterates over each result.

Then look at the assumptions you're making about your expectations. In our case, we have no way to deal with a string that has numbers, or is in all caps for example. 


## Setting up Mocha + Chai
Mocha is a testing framework. Chai is an assertion library

Mocha helps the process of running tests. Get better, more helpful output. Separate your tests from your code.
 
Install Mocha + Chai:
`npm install --save-dev mocha chai`

Mocha and Chai work well with npm. Make sure you've started npm within your project.
`npm init`

Inside the package.json, there is a "script" section that has a default "test" command. If you installed mocha, it will be set to 'mocha.' If not, put it in.

Create a test folder at the same level as the `package.json` file. **It should be called 'test,** not tests or anything else. 

All test files should be put in the 'test' directory. To run them, all you have to do now is use npm:
`npm test`


## Setting up a Test Suite
This is an example of how a test suite is setup. The describe() function should describe what a group of tests is for. It is recommended to have at least one 'sanity check' test to make sure the basics of everything are working. 

```javascript
var expect = require('chai').expect;

// Test Suite
describe('Mocha', function() {
	// Test spec (unit test)
	it('should run our tests using npm', function() {
		expect(true).to.be.ok; // Chai assertion method for testing truthiness.
	});
});
```

## Writing a Test Suite
A suite for testing a Battleship game.
See files in this directory. NOTE: use `npm init` to install dependencies.

## Resources
[Steven Anderson Blog: Writing Great Unit Tests](http://blog.stevensanderson.com/2009/08/24/writing-great-unit-tests-best-and-worst-practises/)
