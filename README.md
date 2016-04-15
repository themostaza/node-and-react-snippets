# node-and-react-snippets
...it doesn't need description I guess

#### Cleaning up the messy seamless-immutable functions in redux-log
```javascript
// Seamless-Immutable logger cleanup
const stateTransformer = (state) => {
  if (typeof state === 'object' && state !== null && Object.keys(state).length) {
    let newState = {}
    for (var i of Object.keys(state)) {
      if (state[i].asMutable) newState[i] = state[i].asMutable({ deep: true })
      else newState[i] = state[i]
    }
    return newState
  } else {
    return state
  }
}
​
// Create the logger
const logger = createLogger({
  collapsed: true,
  stateTransformer
})
```
