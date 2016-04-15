<img src="https://aspblogs.blob.core.windows.net/media/dwahlin/Windows-Live-Writer/57c59b2be72b_127DE/image_8.png" width="140" align="left">
# Snipppets!
Node - React - ReactNative - Redux
<br />
<br />
<br />
<br />
  
## Redux snippets
#### Cleaning up the messy seamless-immutable functions in redux-log
```javascript
import createLogger from 'redux-logger'

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
const loggerMiddleware = createLogger({
  collapsed: true,
  stateTransformer
})
```

## Unit Testing
#### Mocking a function (startRanging) of  module(beaconsHandler) imported by another module(myModule) (...what a mess)
```javascript
import proxyquire from 'proxyquire'


// Test case: 
// - I need to check that myModule calls startRanging (which
//   is a function that myModule imports from beaconsHandler)
// - beaconsHandler may do a lot of things we don't need (like
//   calling dependecies that are not accessible during our tests)
// - ...let's mock startRanging:
const mockedStartRanging = () => null
const myModule = proxyquire('../../src/myModule', {
  '../services/beaconsHandler': {
    startRanging: mockedStartRanging,
    '@noCallThru': true // Block dependencies check
  }
})

// We can now check that the function is called without messy dependency issues.
// In e.g. with Mocha:
  it('checks for transmission support', () => {
    const actual = iterator.next().value
    const expected = call(mockedStartRanging)
    expect(actual).to.deep.equal(expected)
  })

```


## Setup configs
#### (React-Native) package.json example
```json
{
  "name": "React-Native app",
  "version": "0.0.1",
  "private": true,
  "scripts": {
    "start": "node node_modules/react-native/local-cli/cli.js start",
    "reset": "watchman watch-del-all && rm -rf node_modules/ && npm cache clean && npm prune && npm i",
    "generate-apk": "curl 'http://localhost:8081/index.android.bundle?platform=android' -dev=false -o 'android/app/src/main/assets/index.android.bundle' && android/gradlew assembleRelease -p android/",
    "lint": "eslint src",
    "test": "mocha --opts tests/mocha.opts tests/ --recursive",
    "lintest": "eslint src && mocha --opts tests/mocha.opts tests/ --recursive"
  },
  "dependencies": {
    "autobind-decorator": "^1.3.3",
    "ramda": "^0.21.0",
    "react": "^0.14.8",
    "react-native": "^0.23.1",
    "react-native-router-flux": "^3.2.13",
    "react-redux": "^4.4.4",
    "redux": "^3.4.0",
    "redux-logger": "^2.6.1",
    "seamless-immutable": "^5.1.1"
  },
  "devDependencies": {
    "babel-eslint": "^6.0.0",
    "babel-plugin-transform-decorators-legacy": "^1.3.4",
    "babel-preset-react-native": "^1.5.6",
    "chai": "^3.5.0",
    "chai-immutable": "^1.5.4",
    "deep-freeze": "0.0.1",
    "eslint": "^2.7.0",
    "eslint-config-standard": "^5.1.0",
    "eslint-config-standard-jsx": "^1.1.1",
    "eslint-plugin-import": "^1.4.0",
    "eslint-plugin-promise": "^1.1.0",
    "eslint-plugin-react": "^4.3.0",
    "eslint-plugin-standard": "^1.3.2",
    "fetch-mock": "^4.3.1",
    "mocha": "^2.4.5",
    "proxyquire": "^1.7.4"
  }
}
```

#### (React-Native) .eslintrc
```json
{
  "extends": ["standard", "standard-jsx"],
  "globals": {
    "fetch": true,
    "__DEV__": true
  },
  "parser": "babel-eslint",
  "plugins": ["import"],
  "rules": {
    "no-unused-vars": 1,
    "arrow-parens": 0,
    "import/default": 2,
    "import/named": 2,
    "import/namespace": 2,
    "import/no-unresolved": [2, { ignore: ['\.png$'] }],
    "import/export": 2,
    "import/no-duplicates": 2,
    "import/imports-first": 2,
    "react/jsx-boolean-value": 0
  },
  "settings": {
    "import/ignore": [
      "node_modules",
      ".(png|jpg)$",
      ".(scss|less|css)$",
      ".[^js(x)?]+$"
    ]
  }
}
```

#### (React-Native) .babelrc
```json
{
  "presets": ["react-native"],
  "plugins": ["transform-decorators-legacy"]
}
```
