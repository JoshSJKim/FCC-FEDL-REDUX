# FCC-FEDL-REDUX

Intro to Redux - "predictable state container for JavaScript apps"

## Create a Redux Store

- Redux is a state management framework that can be used with a number of different web technologies, including React.

- In Redux, there is a single state object that's responsible for the entire state of an application.
- For example, if there is a React app with ten components, each having its own local state, the entire state of the app would be defined by a single state object housed in the Redux `store`.
- This means that any time any piece of an app wants to update state, it must be done through the Redux `store`.
- The unidirectional data flow makes it easier to track state management in the app.

```jsx
const reducer = (state = 5) => {
    return state;
}

const store = Redux.createStore(reducer);
```

## Get State from the Redux Store

- Retrieve current `state` held in the Redux store object with the `getState` method.

- The code from the previous exercise can be written more concisely

```jsx
const store = Redux.createStore(        // create Redux store
    (state = 5) => state
);

const currentState = store.getState();  // get state from created store
```

## Define a Redux Action

- Since Redux is a state management framework, updating state is one of its core tasks.
- In Redux, all state updates are triggered by dispatching actions.
- An action is simply a JS object that contains information about an action event that occurred.
- The Redux store receives these action objects, then updates the states accordingly.
- Redux action also carries some data when necessary.
- The data is optional, but all actions must carry a `type` property that specifies the type of action that occurred.

```jsx
const action = {
    type: "LOGIN"
};
```

## Define an Action Creator

- After creating an action, the next step is to send it to the Redux store to update the state by defining action creators.
- Action creator is simply a JS function that returns an action.

```jsx
const action = {
    type: 'LOGIN'
};

function actionCreator() {
    return action;
};
```

## Dispatch an Action Event

- `dispatch` method is used to dispatch actions to the Redux store.
- Calling `store.dispatch()` and passing the value returned from an action creator sends an action back to the store.

```jsx
const store = Redux.createStore(
    (state = {login: false}) = > state
);

const loginAction = () => {
    return {
        type: 'LOGIN'
    }
};

store.dispatch(loginAction());
```

## Handle an Action in the Store

- After an action is created and dispatched, the Redux store needs to know how to respond to that action.
- This is done with the `reducer` function, which is responsible for the state modifications that take place in response to actions.
- A `reducer` takes `state` and `action` as arguments, and it always returns a new `state`.
- It is important to note that this is the ONLY role of the `reducer`.

- Another key principle in Redux is that `state` is read-only.
- In other words, the `reducer` function must ALWAYS return a new copy of `state` and never modify state directly.
- Note that Redux does not enforce state immutability.
- The programmer is responsible for enforcing it in the reducer function code.

```jsx
const defaultState = {
    login: false
};

const reducer = ( state = defaultState, action) => {
    if (action.type === "LOGIN") {
        return {
          login: true
        };
    } else {
      return state
    }
};

const store = Redux.createStore(reducer);

const loginAction = () {
    return {
        type: 'LOGIN'
    }
};
```

## Use a Switch Statement to Handle Multiple Actions

- Redux store can be programmed to handle multiple action types.
- Use a switch statement to achieve this.

```jsx
const defaultState = {
    authenticated: false
};

const authReducer = (state = defaultState, action) => {
    switch (action.type) {
        case 'LOGIN':                   // do not include 'break;' in switch statements.
            return {
                authenticated: true
            };
        case 'LOGOUT':
            return {
                authenticated: false
            };
        default:                        // default case is mandatory
            return state;
    }
};

const store = Redux.createStore(authReducer);

const loginUser = () => {       // this is a Redux action object
    return {
        type: 'LOGIN'
    }
};

const logoutUser = () => {
    return {
        type: 'LOGOUT'
    }
};
```

- `defaultState` object is defined with a single property `authenticated` set to `false`
- The `authReducer` function is defined and it passes two parameters: `state` and `action`.
  - `state` is set to `defaultState` object by default, and the `action` parameter will contain the action object when dispatched to the store.
- Inside the `authReducer` function is a `switch` statement that checks the value of `action.type`.
  - If `action.type` is `LOGIN`, the function returns a new object with `authenticated` set to `true`.
  - If `action.type` is `LOGOUT`, the function returns a new object with `authenticated` set to `false`.
  - If `action.type` is not recognized, it returns the current state object by default.
- `store` is created using `Redux.createStore` function, which passes `authReducer` function as its argument.
  - This sets up the store with the initial state object `(state = defaultState)`
- `loginUser` and `logoutUser` functions are defined, each returning an object with a `type` property set to `LOGIN` and `LOGOUT`, respectively.
  - These functions are used to dispatch actions to the store
    - `store.dispatch(loginUser());`
      - When it is called, it returns an action object with a `type` of `LOGIN`
    - `store.dispatch(logoutUser());`
      - When called, it returns an action object with a `type` of `LOGOUT`
- Each time an action is dispatched to the store, the `authReducer` function is called with the current state and the action object as arguments.
  - The `switch` statement in the `authReducer` function checks the `type` property of the action object and returns a new state object accordingly
- `store.getState` method can be used to get the current state of the store.

- It is important to include a default case in the switch statement because all reducers in the app will run whenever an action is dispatched.
- It means that it will run through the reducer even if the action isn't related to the reducer.
- In such cases, it is important that the reducer returns and maintains the current state.

## Use const for Action Types

- A common practice when working with Redux is to assign action types as read-only constant, then reference these constants wherever they are used.

```jsx
const LOGIN = 'LOGIN';
const LOGOUT = 'LOGOUT';

const defaultState = {
    authenticate: false
};

const authReducer = ( state = defaultState, action ) => {
    switch (action.type) {
        case LOGIN:
            return {
                authenticated: true;
            };
        case LOGOUT:
            return {
                authenticated: false;
            };
        default: 
            return state;
    }
};

const store = Redux.createStore(authReducer);

const loginUser = () => {
    return {
        type: LOGIN
    }
};

const logoutUser = () => {
    return {
        type: LOGOUT
    }
};
```

- Note: It's generally a convention to write constants in all uppercase. This is standard practice in Redux as well.

## Register a Store Listener

- Aside from `store.dispatch()` method to access the Redux store objects is `store.subscribe()`.
- This allows you to subscribe listener functions to the store, which are called whenever an action is dispatched against the store.
- One simple use for this method is to subscribe a function to your store that simply logs a message every time an action is received and the store is updated.

```jsx
const ADD = 'ADD';

const reducer = (state = 0, action) => {
    switch(action.type) {
        case ADD:
            return state + 1;
        default: 
            return state;
    }
};

const store = Redux.createStore(reducer);

let count = 0;

const increment = () => {
    return count++;
}

store.subscribe(increment);

store.dispatch({type: ADD});
console.log(count);             // 1
store.dispatch({type: ADD});
console.log(count);             // 2
store.dispatch({type: ADD});
console.log(count);             // 3
```

- Note that the count variable, increment function, and store.subscribe method are not directly related to the logic of the rest of the code.
- It simply 'listens in' to keep track of how many times an ADD action type has been dispatched to the store.
- This just demonstrates a simple way of how subscriber function can be used to listen to state changes in a Redux store.

## Combine Multiple Reducer

- When the state of the app grows more complex, it may be tempting to divide state into multiple pieces.
- Remember that in Redux, all app state is held in a single state object in the store.
- Therefore, Redux provides reducer composition as a solution for a complex state model.
- Define multiple reducers to handle different pieces of the application state, then compose these reducers together using `combineReducers()` method into a `root reducer`.
- This root reducer is then passed into the Redux `createStore()` method.

- `combineReducers()` method accepts an object as an argument in which you define properties which associate keys to specific reducer functions.
- The names given to the keys will be used by Redux as the name for the associated piece of state.

- Typically, it is good practice to create a reducer for each piece of application state when they are distinct or unique in some way.

```jsx
const INCREMENT = 'INCREMENT';
const DECREMENT = 'DECREMENT';

const counterReducer = (state = 0, action) => {
    switch(action.type) {
        case INCREMENT:
            return state + 1;
        case DECREMENT:
            return state - 1;
        default:
            return state;
    }
};

const LOGIN = 'LOGIN';
const LOGOUT = 'LOGOUT';

const authReducer = ( state = {authenticated: false}, action) => {
    switch(action.type) {
        case LOGIN:
            return {
                authenticated: true
            };
        case LOGOUT: 
            return {
                authenticated: false
            };
        default:
            return state;
    }
};

const rootReducer = Redux.combineReducers({
    auth: authReducer,
    count: counterReducer
});

const store = Redux.createStore(rootReducer);
```

## Send Action Data to the Store

- Exercises up to this point only dealt with actions that contain the `type` information.
- Specific data can be sent along with the actions.
- This is very common because actions usually originate from some user interaction and tend to carry some data.
- The Redux store often needs to know this data.

```jsx
const ADD_NOTE = 'ADD_NOTE';

const notesReducer = (state = 'initial state', action) => {
    switch(action.type) {
        case ADD_NOTE:          // When action is dispatched (type: ADD_NOTE)
            return action.text; // it returns the action.text
        default:
            return state;
    }
};

const addNoteText = (note) => {
    return {
        type: ADD_NOTE,
        text: note
    }
};

const store = Redux.createStore(notesReducer);

console.log(store.getState());          // Initial state
store.dispatch(addNoteText('Hello!'));  
console.log(store.getState());          // Hello!
```

## Use Middleware to Handle Asynchronous Actions

- Asynchronous actions are an unavoidable part of web development.
- At some point, it is necessary to call asynchronous endpoints in the Redux app.
- Redux provides middleware designed specifically to handle asynchronous actions called `Redux Thunk`.

- To include Redux Thunk, pass it as an argument to `Redux.applyMiddleware()`
- This statement is then provided as a second optional parameter to the `createStore()` function.
- Then, to create an asynchronous action, return a function in the action creator that takes `dispatch` as an argument.
- Within this function, you can dispatch actions and perform asynchronous requests.

```jsx
const REQUESTING_DATA = 'REQUESTING_DATA';
const RECEIVED_DATA = 'RECEIVED_DATA';

const requestingData = () => {
    return {
        type: REQUESTING_DATA
    }
}
const receivedData = (data) => {
    return {
        type: RECEIVED_DATA, users: data.users
    }
}

const handleAsync = () => {
    return function(dispatch) {
        store.dispatch(requestingData());
        setTimeout(function() {
            let data = {
                users: ['Jeff', 'William', 'Alice']
            }
        store.dispatch(receivedData(data))
        }, 2500);
    }
};

const asyncDataReducer = (state = defaultState, action) => {
    switch(action.type) {
        case REQUESTING_DATA:
            return {
                fetching: true,
                users: []
            }
        case RECEIVED_DATA:
            return {
                fetching: false,
                users: action.users
            }
        default: 
            return state;
    }
};

const store = Redux.createStore(
    asyncDataReducer, Redux.applyMiddleware(ReduxThunk.default)
);
```

## Write a Counter With Redux

```jsx
const INCREMENT = 'INCREMENT';
const DECREMENT = 'DECREMENT';

const counterReducer = (state = 0, action) => {
    switch(action.type) {
        case INCREMENT:
            return state + 1
        case DECREMENT:
            return state - 1
        default:
            return state;
    }
};

const incAction = () => {
    return {
        type: INCREMENT
    }
};

const decAction = () => {
    return {
        type: DECREMENT
    }
};

const store = Redux.createStore(counterReducer);

store.dispatch(incAction());
store.dispatch(incAction());    
console.log(store.getState());  // 2
store.dispatch(decAction());
console.log(store.getState());  // 1
```

## Never Mutate State

- Immutable state means that the state must never be directly modified.
- Instead, return a new copy of state.

- If a snapshot of the state of a Redux app over time, it would look something like `state 1`, `state 2`, `state 3`, `state 4`, etc.
- Each state may be similar to the last, but each is a distinct piece of data.
- This immutability is what makes `time-travel debugging` possible.

- Remember, React does not actively enforce state immutability in its store or reducers; it is the responsibility of the programmer.
- JavaScript (ES6 in particular) provides several useful tools that can be used to enforce state immutability.
- Strings and numbers are primitive values and are immutable by nature.
- But arrays and objects are mutable.
- The states handled in Redux and React will probably consist of arrays and objects since they are useful data structures for representing many types of information.

```jsx
const ADD_TO_DO = 'ADD_TO_DO';

const todos = [
    'Go to the store',
    'Clean the house',
    'Cook dinner', 
    'Learn to code'
];

const immutableReducer = (state = todos, action) => {
    switch(action.type) {
        case ADD_TO_DO:     // Remember not to modify the original state
            return todos.concat(action.todo) // .push() will modify the original, but .concat() will return a new array.
        default:
            return state;
    }
};

const addToDo = (todo) => {
    return {
        type: ADD_TO_DO,
        todo
    }
};

const store = Redux.createStore(immutableReducer);
```

## Use the Spread Operator on Arrays

- One solution from ES6 to help enforce state immutability in Redux is the spread operator `...`.
- It has a variety of applications, one of which is to produce a new array from an existing array.

```jsx
// alternate solution for previous exercise using spread operator

const immutableReducer = (state = todos, action) => {
    switch(action.type) {
        case ADD_TO_DO:
            let newArr = [];                            // declare a new variable newArr with an empty array
            return newArr = [...todos, action.todo];    // use the spread operator to spread the elements of 'todos' in the new array
    }                                                   // and include the value of action.todo at the end of the clone array.
};
```

- The spread syntax can be used multiple times in array composition in this manner.
- But it's important to keep in mind that it only makes a shallow copy of the array.
- In other words, it only provides immutable array operations for one-dimensional arrays.

```jsx
const immutableReducer = ( state = ['Do not mutate state!'], action) => {
    switch(action.type) {
        case 'ADD_TO_DO':
            let newArr = [];
            return newArr = [...state, action.todo];
        default:
            return state;
    }
};

const addToDo = (todo) => {
    return {
        type: 'ADD_TO_DO',
        todo
    }
};

const store = Redux.createStore(immutableReducer);
```

## Remove and Item from an Array

- Practice removing items from an array.
- Remember not to modify the state

```jsx
const immutableReducer = (state = [0, 1, 2, 3, 4, 5], action) => {
    switch(action.type) {
        case 'REMOVE_ITEM':
            let removedArr = state.filter((elem) => elem !== state[action.index]);
            return removedArr
        default:
            return state;
    }
};

const removeItem = (index) => {
    return {
        type: 'REMOVE_ITEM',
        index
    }
}

const store = Redux.createStore(immutableReducer);

store.dispatch(removeItem(2));
console.log(store.getState()) // [0, 1, 3, 4, 5]
```

## Copy an Object with Object.assign

- When working with objects, `Object.assign()` utility is a useful tool to enforce state immutability.
- It takes a target object and source objects and maps properties from the source objects to the target object.
- Any matching properties are overwritten by properties in the source objects.
- This behavior is commonly used to make shallow copies of objects by passing an empty object as the first argument followed by the object(s) to be copied.

`const newObject = Object.assign({}, obj1, obj2);`

- This will create a new object `newObject`.
- It contains the properties that exist in obj1 and obj2.

```jsx
const defaultState = {
    user: 'CamperBot',
    status: 'offline',
    friends: '732,982',
    community: 'freeCodeCamp'
};

const immutableReducer = (state = defaultState, action) => {
    switch(action.type) {
        case 'ONLINE': 
            return Object.assign({}, state, {status: 'online'});
        default:
            return state;
    }
};

const wakeUp = () => {
    return {
        type: 'ONLINE'
    }
};

const store = Redux.createStore(immutableReducer);

store.dispatch(wakeUp());
console.log(store.getState());
// { user: 'CamperBot',
//   status: 'online',
//   friends: '732,982',
//   community: 'freeCodeCamp' }
```

- Overall, this example demonstrates how to use `Object.assign()` utility to create new state objects that are updated in an immutable way.
- By returning a new object rather than modifying the old one, the reducer maintains a consistent state and avoids introducing bugs caused by shared state.
