# Redux Notes
Contents:  
1. [Wes Bos](#Wes\ Bos)  
2. [React Training](#React\ Training)


## 1. Wes Bos

[Videos](https://learnredux.com/account/access/586c180973e59699e1bab27f)

[Starter Files](https://github.com/wesbos/Learn-Redux-Starter-Files)

### Actions
Something that happens in your application (a click, a load, submit, etc.). Like JS events.

It's an object that indicates what needs to change.

Send as little information as you can get away with when dispatching an action

Defined by action creators. 

### Reducers
Does the work of the actions. Like event handlers. It edits state (which is stored in a store).

Like many handler functions in regular react, it takes in the info about what happened and makes a copy of state, then returns a new version of state after acting on the info received.

Create a reducer for every piece of state. 

You can only have (and need to have) one root reducer.

Triggered by Actions.

Every time an Action is dispatched, ALL REDUCERS ARE RUN. Whether or not it's run is up to each reducer.

## 2. React Training