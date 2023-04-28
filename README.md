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
