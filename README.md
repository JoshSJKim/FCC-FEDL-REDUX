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