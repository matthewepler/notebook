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

NOTE: 'describe()' functions can be nested for logical groupings and/or fixtures (see below)

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
Starts at 'part5.'

Start:
__"/test/ship_test.js"__
```javascript
var expect = require('chai').expect;

describe('checkForShip', function() {
	// get the function we want to test from the file in which it was written
	var checkForShip = require('../game_logic/ship_methods').checkForShip;
	
	it('should correctly report no ship at a given players coordinate', function() {
		player = {
			ships: [
				{
					location: [[0, 0]]
				}
			]
		};
		
		expect(checkForShip(player, [9, 9])).to.be.false;
	});

	it('should correctly report a ship at a given players coordinate', function() {
		player = {
			ships: [
				{
					location: [[0, 0]]
				}
			]
		};	
		
		expect(checkForShip(player, [0, 0])).to.be.true;
	});
}
```

__"/game_methods/ship_methods.js"__
```javascript
function checkForShip (player, coordinates) {
	var shipPresent, ship;

	for (var i = 0; i < player.ships.length; i++) {
		ship = player.ships[i];

		shipPresent = ship.locations.filter(function (actualCoordinate) {
			return (actualCoordinate[0] === coordinates[0]) && (actualCoordinate[1] === cordinates[1]);
		}});
	}

	if (!shipPresent) {
		return false;
	} else {
		return true;
	}
}

module.exports.checkForShip = checkForShip;
```


## Making Tests Easier with Fixtures
Setup phase (provided by Mocha) helps us stay DRY. There are two places to use setup - before the entire series of tests and before each individual tests. 

write the mock dependencies (variables, objects, etc.) with the before() func. before the entire suite.
```javascript
var player;

describe('checkForShip', function() {
	before(function() {
		player = {
			ships: [
				{
					locations: [[0, 0]]
				}
			]
		}
	});

	it('should correctly report no ship at a given players coordinate', function() {
		// etc...
	});
});
```

In our code (see dir), we have different versions fo the 'player' object with increasing ccomplexity. You should use the most complex version of the object in the 'before' function. 

'checkForShip' is a pure function. Changing how its tests run do not affect other tests. In the case of a function that is not pure (see 'fire'), changes when refactorying may affect other parts of our code. 

**before() and beforeEach()**
```javascript
describe('fire', function () {
    var fire = require('../game_logic/ship_methods').fire;
    var player;
    
    beforeEach(function () {
        player = {
            ships: [
                {
                    locations: [[0, 0]],
                    damage: []
                }
            ]
        };    
    });

	it('should record damage on the given players ship at a given coordinate', function () {
        fire(player, [0, 0]);
        expect(player.ships[0].damage[0]).to.deep.equal([0, 0]);
    });
```

This resets the ship object to a nice clean state before testing. This makes the behavior of our function predictable between tests, even when the function has side effects.

These mock variables used to test in suites are called __fixtures__.


## Teardown 
The last phase of a test suite. Removes unwanted variables, stops processes, and deletes local files if they were created. 

Tests should never be clever. They should only report on our basic expectations. Example: do not pass state between test suites. 

Teardown is __not__ required for plain javascript objects.

**after() and afterEach()**
Put these in after the 'before()' but before the 'it()' statements.
```javascript
...
before()...
...
after(function() {
	console.log('entire test suite completed.');
});
afterEach(function() {
	console.log('one unit test complete');
});

it(...)
```


## Covering Edge Cases
What if we get something we don't expect? What if (for example) someone calls a function with incorrect or missing arguments?

Writing for edge cases shows other developers what you plan for your functions in terms of functionality and behavior. For example, when to throw an error. 

See docs in dir __3-covering-edge-cases__ for examples.

Of course, there are always tons of edge-cases. You don't have to write all of them. WRite ones that:
* will break your programs
* other developers might need to think about
* most likely user behaviors

 
## Code that's not easily testable should be refactored
For example, if you have a function that calls another function (tight-coupling).
```javascript
function computerFire(player) {
	var x = Math.floor(Math.random() * 9);
	var y = Math.floor(Math.random() * 9);
	var coordinates = [x, y];

	fire(player, coordinates); // only called within this computerFire func.
});
```

In the example above, you can pull out the first three lines into a separate function that returns 'coordinates.' Then fire can be called on its own and computerFire is unecessary.

See dir __"5-answer-writing-testable-code"__

IOW - if you have to write tests for existing code and it's hard to see an easy way to do it, you should think about refactoring your code so that it's more modular.


## Mocha Reporting 
`--reporter` flag changes the kind of reporter done. E.g. see only # passing, and full report for failures:
`mocha --reporter min`

`--reporter markdown` will print the same long test report but using MD format. You can then copy/paste into a README for a git repo.

[More available](https://mochajs.org/#reporters) in Mocha documentation. To make it your default, just add it to the 'test' section of your `package.json` file:
`mocha --reporter nyan`


## Outlining Test Suites
If you know what you need but don't want to get into the details yet.
```javascript
var expect = require('chai').expect;

describe('GAME INSTANCE FUNCTIONS', function() {
	describe('checkGameStatus', function() {
		it('should tell me when the game is over');
	});
});
```
The 'it()' function only has one argument here. The second (usually the function that describes our expectations) is left out. When the suite is run, this will show as 'pending.'

Useful as a reminder that tests still need to be written.

Blocks of tests can also be marked pending by appending an 'x' before the block:
```javascript
xdescribe('blah blah', function() {
	// everything in here is now pending
}
```

Generally it is recommended to leave out the function argument as opposed to the 'x' as it is more specific and does not require deleting anything.


## Watch - auto run your tests
Instead of running `npm test` every time you want to make sure your tests pass, you can use the watch flag to run the tests every time you make a change.

See files in __"4-next-steps"__

`mocha --watch ./test/game_test.js ./game_logic/game_instance.js`
Will run the tests found in the test file every time there is a change in the instance file. 

You can save a general 'watch' command in your `package.json` file:
```json
"scripts": {
	"test": "mocha",
	"test:watch": "mocha --watch ./test ./"
}
```
Now you can run `npm test:watch'. Don't forget the '.'

You can create other custom test methods in `package.json` to only watch certain things at on demand.


## Mocks and Stubs
Functions that fill in the gaps for our dependencies. There is some discrepency amongst developers about what 'mocks' and 'stubs' means. 

You can create empty functions as fixtures and use them to run tests. 

See dir __"4-next-steps/4-mocks-and-stubs"__ for examples. 

[Sinon.js](http://sinonjs.org/) is a library that helps with creation of mocks and stubs (as well as something called Spies)


## Testing Asynchronous Functions
Mocha allows us to describe a suite as asynchronous. 

Use the 'done' argument in the callback of an 'it()' function. Then call `done()` when we expect the asynchronous code to have completed. 

See __game_test.js__ in __"4-next-steps/5-testing-asynchronous-code/final"__

Usually you would write mocks/stubs for things like Ajax calls. 


## Resources
[Steven Anderson Blog: Writing Great Unit Tests](http://blog.stevensanderson.com/2009/08/24/writing-great-unit-tests-best-and-worst-practises/)
[list of Chai functions](http://chaijs.com/api/bdd/)
[Sinon.js](http://sinonjs.org/)
