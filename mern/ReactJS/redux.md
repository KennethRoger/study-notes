Redux

Three core concepts - Store, Action, Reducer

# Redux Store

- Holds application state
- Allow access to state via `getState()`
- Allow state to be updated via `dispatch(action)`
- Registers listeners via `subscribe(listener)`
- Handles unregistering of listeners via the function returned by `subscribe(listener)`

Three Principles -
1. "The state of your whole application is stored in an object tree within a single store"
That is maintaint our application state in a single object which would be managed by the Redux store

Cake Shop-
Assume we are tracking the number of cakes on the shell

{
  numberOfCakes: 10;
}

2. "The only way to change the state is to emit an action, an object describing what happened"
That is if you want to update the state of your app, you need to let Redux know about that with an action. You are not allowed to directly update the state object. Shopkeeper example.

Cake Shop -
Scan the QR code and place an order - CAKE_ORDERED
{
  type: 'CAKE_ORDERED'
}


3. "To specify how the state tree is transformed by acions, you write pure reducers"
Reducer - (prevState, action) => newState

Eg:-
```js
const redux = require("redux");
const createStore = redux.createStore;

// Action

const BUY_CAKE = "BUY_CAKE";

function buyCake() {
  return {
    type: BUY_CAKE,
    info: "First redux action",
  };
}

// Reducer - (prevState, action) => newState

const initialState = {
  numOfCakes: 10,
};

const reducer = (state = initialState, action) => {
  switch (action.type) {
    case BUY_CAKE:
      return {
        ...state,
        numOfCakes: state.numOfCakes - 1,
      };

    default:
      return state;
  }
};

// Creeating store

const store = createStore(reducer);
console.log("initialState", store.getState());
const unsubscribe = store.subscribe(() => console.log("Updated state", store.getState()));
store.dispatch(buyCake());
store.dispatch(buyCake());
store.dispatch(buyCake());
unsubscribe()
```

When using more than one reducer you use `redux.combineReducers` to combine the reducers

eg:

```js
const combineReducers = redux.combineReducers;

...
const rootReduceer = combineReducers({
    cake: cakeReducer,
    iceCream: iceCreamReducer
})

const store = createStore(reducer)
...

```

# Middleware (eg: redux logger, redux thunk)

- The suggested way to extend Redux with custom functionality
- Provides a third-party extension point between dispatching an action, and the moment it reaches the reducer
- Use middleware for logging, crash reporting, performing aynchronous tasks etc

```js
const redux = require('redux');
...
cosnt reduxLogger = require('redux-logger');
const applyMiddleware = redux.applyMiddleware;
const logger = reduxLogger.createLogger();

...
const store = createStore(rootReducer, applyMiddleware(logger));

// Now in store.subscribe you dont need the log statement
const unsubscribe = store.subscribe(() => {});
...

```

# Immer library
Immer. With this library handling state changes which have multiple nested state is easier. This package allows you to work with immutable state in a more convenient way.


# Async Actions

### why?

Maybe you need to do asynchronous API calls to fetch data from an endpoint and use that data in your applicaton.

```js
const redux = require("redux");
const createStore = redux.createStore;
const applyMiddleware = redux.applyMiddleware;
const thunkMiddleware = require("redux-thunk").default;
const axios = require("axios");

const initialState = {
  loading: false,
  users: [],
  error: "",
};

const FETCH_USER_REQUEST = "FETCH_USER_REQUEST";
const FETCH_USER_SUCCESS = "FETCH_USER_SUCCESS";
const FETCH_USER_FAILURE = "FETCH_USER_FAILURE";

const fetchUserRequest = () => {
  return {
    type: FETCH_USER_REQUEST,
  };
};

const fetchUserSuccess = (users) => {
  return {
    type: FETCH_USER_SUCCESS,
    payload: users,
  };
};

const fetchUserFailure = (error) => {
  return {
    type: FETCH_USER_FAILURE,
    payload: error,
    FETCH_USER_REQUEST,
  };
};

const reducer = (state = initialState, action) => {
  switch (action.type) {
    case FETCH_USER_REQUEST: {
      return {
        ...state,
        loading: true,
      };
    }

    case FETCH_USER_SUCCESS: {
      return {
        loading: false,
        users: action.payload,
        error: "",
      };
    }

    case FETCH_USER_FAILURE: {
      return {
        loading: false,
        users: [],
        error: action.payload,
      };
    }
  }
};

const fetchUsers = () => {
  return function (dispatch) {
    dispatch(fetchUserRequest());
    axios
      .get("https://jsonplaceholder.typicode.com/users")
      .then((res) => {
        const users = res.data.map((user) => user.id);
        dispatch(fetchUserSuccess(users));
      })
      .catch((err) => {
        dispatch(fetchUserFailure(err.message));
      });
  };
};

const store = createStore(reducer, applyMiddleware(thunkMiddleware));
store.subscribe(() => {
  console.log(store.getState());
});
store.dispatch(fetchUsers());

```

# Concerns of Redux

- Redux require too much boilerplate code
- A lot of other packages have to be installed with redux for specific functionality(redux-thunk, Immer, redux-devtool)

# Redux Toolkit

Redux example while studying redux toolkit

```js
const redux = require("redux");
const { createStore } = redux;
const { bindActionCreators } = redux;
const initialState = {
  value: 0,
  tracker: 0,
};

const INCREMENT_VALUE = "INCREMENT_VALUE";
const DECREMENT_VALUE = "DECREMENT_VALUE";
const RESET_VALUE = "RESET_VALUE";
const TRACKER = "TRACKER";
const MULTIPLY = "MULTIPLY";

function incrementValue() {
  return {
    type: INCREMENT_VALUE,
    payload: 1,
  };
}

function decrementValue() {
  return {
    type: DECREMENT_VALUE,
  };
}

function resetValue() {
  return {
    type: RESET_VALUE,
  };
}

function multiply(val = 1) {
  return {
    type: MULTIPLY,
    payload: val,
  };
}

function tracker() {
  return {
    type: TRACKER,
  };
}

const reducer = (state = initialState, action) => {
  switch (action.type) {
    case INCREMENT_VALUE:
      return { ...state, value: state.value + 1 };
    case DECREMENT_VALUE:
      return { ...state, value: state.value - 1 };
    case RESET_VALUE:
      return { ...state, value: 0 }
    case TRACKER:
      return { ...state, tracker: state.tracker + 1 };
    case MULTIPLY:
      return { ...state, value: state.value * action.payload };
    default:
      return state;
  }
};

const store = createStore(reducer);
console.log("Initial state", store.getState());

const unsubscribe = store.subscribe(() =>
  console.log("Updated State", store.getState())
);
store.dispatch(incrementValue());
store.dispatch(incrementValue());
store.dispatch(decrementValue());
store.dispatch(decrementValue());
store.dispatch(decrementValue());
store.dispatch(decrementValue());
store.dispatch(incrementValue());
store.dispatch(incrementValue());
store.dispatch(incrementValue());
store.dispatch(incrementValue());
store.dispatch(tracker());
store.dispatch(tracker());
store.dispatch(tracker());
store.dispatch(multiply());
store.dispatch(multiply());
store.dispatch(multiply(5));
store.dispatch(resetValue())

// Used in old react
// const actions = bindActionCreators({ incrementValue, multiply }, store.dispatch)
// actions.incrementValue()
// actions.incrementValue()
// actions.incrementValue()
// actions.incrementValue()
// actions.multiply(2)
// actions.multiply(3)
// actions.multiply(4)
// actions.multiply(5)

unsubscribe();
```

# Multiple Reducers

When the number of state increases and it becomes very complicated to handle all the states in a single reducer, 
we use a method from redux called `combineReducer`


Example with `combineReducer`:
```js
const redux = require("redux");
const { createStore, combineReducers } = redux;

// INITIAL STATE

// Cake
const initialCakeState = {
  numOfCakes: 15,
};

// IceCream
const initialIceCreamState = {
  numOfIceCreams: 30,
};

// ACTIONS

// Cake
const BUY_CAKE = "BUY_CAKE";
const RESTOCK_CAKE = "RESTOCK_CAKE";

const buyCake = (qty = 1) => {
  return {
    type: BUY_CAKE,
    payload: qty,
  };
};

const restockCake = (qty = 1) => {
  return {
    type: RESTOCK_CAKE,
    payload: qty,
  };
};

// IceCream
const BUY_ICE_CREAM = "BUY_ICE_CREAM";
const RESTOCK_ICE_CREAM = "RESTOCK_ICE_CREAM";

const buyIceCream = (qty = 1) => {
  return {
    type: BUY_ICE_CREAM,
    payload: qty,
  };
};

const restockIceCream = (qty = 1) => {
  return {
    type: RESTOCK_ICE_CREAM,
    payload: qty,
  };
};

// REDUCERS

// Cake
const cakeReducer = (state = initialCakeState, action) => {
  switch (action.type) {
    case BUY_CAKE:
      return { ...state, numOfCakes: state.numOfCakes - action.payload };
    case RESTOCK_CAKE:
      return { ...state, numOfCakes: state.numOfCakes + action.payload };
    default:
      return state;
  }
};

// IceCream
const iceCreamReducer = (state = initialIceCreamState, action) => {
  switch (action.type) {
    case BUY_ICE_CREAM:
      return {
        ...state,
        numOfIceCreams: state.numOfIceCreams - action.payload,
      };
    case RESTOCK_ICE_CREAM:
      return {
        ...state,
        numOfIceCreams: state.numOfIceCreams + action.payload,
      };
    default:
      return state;
  }
};

const rootReducers = combineReducers({
  cake: cakeReducer,
  iceCream: iceCreamReducer,
});
const store = createStore(rootReducers);
const { subscribe, getState, dispatch } = store;

console.log("Initital state", getState());

const unsubscribe = subscribe(() => console.log(getState()));

dispatch(buyCake());
dispatch(buyCake(4));
dispatch(buyCake(10));
dispatch(buyIceCream(3));
dispatch(restockCake(10));
dispatch(buyIceCream(5));
dispatch(buyIceCream(7));

unsubscribe();
```



