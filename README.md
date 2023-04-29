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
        case ADD_NOTE:          
            return action.text;
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
