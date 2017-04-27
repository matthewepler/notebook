#Links
**Repos**
* [code that goes with
vides](https://github.com/ReactjsProgram/React-Fundamentals)
* [curriculum you will build on your
  own](https://github.com/ReactjsProgram/react-fundamentals-curriculum)

# Intro to React Ecosystem Video
**Imperative vs. Declarative**
*Imperative* = How you will do something. E.g. looping over an array with a
for-loop doesn't tell us what you're doing, just that you're going to loop.
*Declarative* = What we want to do . E.g. reduce an array to return a specific
result, the equivalent of the for loop but declared in a way that explains
what you want to achieve.

React is mostly declarative. The components are, but the setState() is not.

**Composition**
Making larger components out of smaller components. This is the same as
writing functions that do one thing well, and then you use them to complete a
series of tasks. 

**Explicit Mutations**
The only way to change state is to explicity call this.setState

**React Router**
Mapping component rendering to URLs. 
* foo.com : Main -> Home
* foo.com/playerOne: Main -> PromptContainer
* etc.

**Axios**
Used to make HTTP requests. 

*How does it compare to fetch?* People are split on what to use, but they're
both request libraries, so they accomplish much of the same thing. See latest
documentation on each to determine what you *can't* do with each one.

# Webpack for React
mentions [blog post series on modules](https://medium.freecodecamp.com/javascript-modules-a-beginner-s-guide-783f7d7a5fcc#.d5cxrcy81)
*^ notes from this can be found in a separate file in the notebook*

# React Container Components vs Presentational (video)
You can pass props to Routes just like any other component.  
```
<Route path='playerOne' header='Player One' component={PromptContainer} />
```

Use <Link> component with ReactRouter.link. 
```
var ReactRouter = require('react-router')
var Link = ReactRouter.Link
```

Use `getInitialState()` instead of constructor? Could be because of the way he's instantiating the Class. Yeah - I think that's it. We should use ES6. 

contextTypes - allows you to pass items to your components without props. Don't use outside of Router type situations (recommended)

```
contexTypes: {
	router: React.PropTypes.object.isRequired
}
```

^ provides access to `this.context` which will contain a `router` object.

To navigate, then you can use:
```
this.context.router.push({
	pathname: '/battle',
	query: {
		playerOne: this.props.routeParams.playerOne,
		playerTwo: this.state.username
	}
})
```

## Containers vs Components
Containers should have business logic, Components should have UI elements.

Go to ~ 17:00 in video for explanation.

When refactoring (separating logic from UI), generally you should change the function names from `onSomething` to `handleSomething` when passing to a component.

## Prop Types
```
var Prompt = React.createClass({
	propTypes: {
		header: PropTypes.string.isRequired
	},
	...
})
```

## Functional Stateless Components
See ~23:00 in video

Has Proptypes, but no state, no methods, just UI...

Can replace Class with a simple function with `props` as a parameter. Then remove references to `this`. Proptypes become property of the func:

```
Prompt.Proptypes = {
	header: PropTypes.string.isRequired
}
```

Advantage = faster rendering, clearner code


# The 'this' keyword, 1-3
* Implicit Binding
* Explicit Binding
* new Binding
* window Binding

Finding out what `this` means is done by looking for where the function
is **invoked**. NOT when it was defined. 

**Implicit binding** - designated by the execution context
- look to the left of the dot (e.g. me.sayName(), i.e. the object in which it
  appears)


Explicit binding 
e.g. using .call(), .apply(), .bind()

.call and .apply invoke the function immediately. .bind() returns a new
function with the context attached.

'new' Binding - using the 'new' keyword

`window` binding - the global window object

# Axios, Promises and the Github API

The puke function for debugging.

```
var React = require('react');

function puke (object) {
  return <pre>{JSON.stringify(object, null, ' )}</pre>
}

function ConfirmBattle (props) {
  return props.isLoading === true
    ? <p> Loading! </p>
    : <div> {puke(props)} </div>
}

module.exports = ConfirmBattle
```
