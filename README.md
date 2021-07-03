# Redux-React-Cribsheet

> Redux is a JavaScript state management library. It can be used with libraries like React and Angular. But in this cribsheet, we will use it with [React](https://github.com/sayeemabdullah/React-Cribsheet#readme). This is an initiative to make small notes on React which may help me or anyone else in the future. You can also check out my [React](https://github.com/sayeemabdullah/React-Cribsheet#readme) and [CSS](https://github.com/sayeemabdullah/CSS-Cribsheet) Cribsheets. 


## Three Building Parts

The three-building parts of Redux are `Store`, `Action` and `Reducer`. In brief, we can say that store holds the state of our application, action describes what happened and the reducer ties the store and the actions together.

___

## Getting Started
 

Before starting we should make sure that we have **React** and to create our app we have to first go to our desired folder and use the below command:

``` shell

npx create-react-app name-of-our-project

```

And now to start our project we will use the following command after going to our project directory:

``` shell

npm start

```

And after that we will use the command below to install `redux` and `react-redux` in our project:

```shell

npm install redux react-redux 

```

As now we are ready we can start our implementation.

___

## Folder Structure

Different people use different structures but I followed [Vishwas’s](https://github.com/gopinav) one which is something like below: 

![Screenshot 2021-07-01 at 1 28 24 PM](https://user-images.githubusercontent.com/31423599/124083984-527c4400-da70-11eb-99a0-648b4aa2c1d9.png)

So here we made a separate folder for all the redux files and inside the **redux folder** we made separate folders for each container where the container’s **Action**, **Reducer** and **Types** files are kept. As we go through the cribsheet, we will better grasp **redux** and the **folder structure**. 

___


## Scenario 1

Before starting implementation lets us think of a scenario, so we will make an application that will show **number of apples** and if we press a button the **number of apples will decrease**.
So the basic structure of the `AppleContainer` will be something like the following if we are using a **class component** : 

``` js

import React, { Component } from "react";

class AppleContainer extends Component {
  render() {
    return (
      <>
        <div>
          <h2>Number of Apples: </h2>
          <button>Buy Apple</button>
        </div>
      </>
    );
  }
}

export default AppleContainer;

```

And something like this if we are using a **functional component**:

``` js

import React from "react";

function AppleContainer(props) {
  return (
    <>
      <div>
        <h2>Number of Apples: </h2>
        <button>Buy Apple</button>
      </div>
    </>
  );
}

export default AppleContainer;

```
___

## Actions

An action is an object with a type property. So our `appleActions` file will be like the following:

``` js

import { BUY_APPLE } from "./appleTypes";

export const buyApple = () => {
  return {
    type: BUY_APPLE,
  };
};

```

But here we can see that we are importing `BUY_APPLE` from a file named `appleTypes` which is used as an action type. Inside the `appleTypes` we will find:

``` js

export const BUY_APPLE = "BUY_APPLE"

```

So we are exporting a const with the same value so that we don’t do any mistakes while typing or we can say we are using that as a constant. This `appleTypes` is just a part of the file structure. We can also not use it if we want.

Once it is done we will make `index.jsx` inside the redux folder from where we will export all action creators as shown below:

``` js

export { buyApple } from "./apple/appleActions";

```

___

## Reducers

Our Reducer file is `appleReducer` which will look something like this:

``` js

import { BUY_APPLE } from "./appleTypes";

const initialState = {
  numOfApples: 20,
};

const appleReducer = (state = initialState, action) => {
  switch (action.type) {
    case BUY_APPLE:
      return {
        ...state,
        numOfApples: state.numOfApples - 1,
      };
    default:
      return state;
  }
};

export default appleReducer;

```

So here first we initialize the `initialState` where our numbers of apples are 20. And then in our reducer, `appleReducer` will receive two parameters which are the **state** which is **initialState** in our case and **action** which we have already created. After that, we have created a switch where the action type will be passed and as we have only one action type at the moment so there is only one case `BUY_APPLE` which will decrement the `numOfApples` by 1 and another default that will return the state. 

But now before creating the `store` we will make another file `rootReducer` which will as follows:

``` js

import { combineReducers } from "redux";
import appleReducer from "./apple/appleReducer";

const rootReducer = combineReducers({
  apple: appleReducer,
});

export default rootReducer;

```
This is a small application but in real life, there will be many reducers and it will be hard for us to control inside the `store`. So in the `rootReducer`, we use a function of Redux called `combineReducers`. Here we can access multiple reducers inside a single reducer as we bind all the reducers into one. 

___

## Store

``` js

import { createStore } from "redux";
import rootReducer from "./rootReducer";

const store = createStore(rootReducer);

export default store;

```

So here we call a function of redux called `createStore` to create a store where we pass the `rootReducer` as a parameter.
___

## Provider

`Provider` is a component of `react-redux`. We will use `Provider` in the `App.js` file as follows:

``` js

import { Provider } from "react-redux";
import "./App.css";
import AppleContainer from "./components/AppleContainer";
import store from "./redux/store";

function App() {
  return (
    <Provider store={store}>
      <div className="App">
        <AppleContainer></AppleContainer>
      </div>
    </Provider>
  );
}

export default App;

```
The provider is used to provide the store to components and we specify the store with a props name `store`. And as shown above we used it in the `App` component so that everyone gets the store.

___

## Connect in Class Component

After connecting in the `AppleContainer` the file will look something like this:

``` js

import { connect } from "react-redux";
import { buyApple } from "../redux";

import React, { Component } from "react";


class AppleContainer extends Component {
  render() {
    return (
      <>
        <div>
          <h2>Number of Apples: {this.props.numOfApples}</h2>
          <button onClick={this.props.buyApple}>Buy Apple</button>
        </div>
      </>
    );
  }
}

const mapStateToProps = (state) => {
  console.log(state);
  return {
    numOfApples: state.apple.numOfApples,
  };
};

const mapDispatchToProps = (dispatch) => {
  return {
    buyApple: () => dispatch(buyApple()),
  };
};

export default connect(mapStateToProps, mapDispatchToProps)(AppleContainer);

```

So we can see we have used `mapStateToProps` and `mapDispatchToProps`. Firstly `mapStateToProps` is used when we want to access the **redux state** in our component. Here we get **redux state** as a parameter that can be used. If we want to dispatch any function we use `mapDispatchToProps`. And we connect both of them with our component using `connect`.

___

## Connect in Functional Component without Hooks


``` js

import React from "react";
import { connect } from "react-redux";
import { buyApple } from "../redux";

function AppleContainer(props) {
  return (
    <>
      <div>
        <h2>Number of Apples: {props.numOfApples}</h2>
        <button onClick={props.buyApple}>Buy Apple</button>
      </div>
    </>
  );
}

const mapStateToProps = (state) => {
  console.log(state);
  return {
    numOfApples: state.apple.numOfApples,
  };
};

const mapDispatchToProps = (dispatch) => {
  return {
    buyApple: () => dispatch(buyApple()),
  };
};

export default connect(mapStateToProps, mapDispatchToProps)(AppleContainer);

```

It is almost the same as the **class component** but in the functional component which is self-explanatory.

___

## Connect in Functional Component with Hooks

``` js

import React from "react";
import { useDispatch, useSelector } from "react-redux";
import { buyApple } from "../redux";

function AppleContainer(props) {
  const numberOfCakes = useSelector((state) => state.apple.numOfApples);
  const dispatch = useDispatch();
  return (
    <div>
      <h2>Number of Apples: {numberOfCakes} </h2>
      <button onClick={() => dispatch(buyApple())}>Buy Apple</button>
    </div>
  );
}

export default AppleContainer;

```
I always believe hooks made our life easier. The code above is one of the examples. We don’t need to write functions like `mapStateToProps` and `mapDispatchToProps` we can simply use `useSelector` and `useDispatch`.

___

## Scenario 2

We will update our scenario a little bit. We will add an input where we can put numbers of apples we want to buy and it will decrease accordingly. 

___

## Action Payload

Now as we want to implement scenario 2, we will first make changes in our `AppleContainer`. So now it will look something like this if we are using **class component**:

``` js

import React, { Component } from "react";
import { connect } from "react-redux";
import { buyApple } from "../redux";

class AppleContainer extends Component {
  constructor(props) {
    super(props);

    this.state = {
      number: 1,
    };

    this.handleChange = this.handleChange.bind(this);
  }

  handleChange(event) {
    this.setState({ number: event.target.value });
  }

  render() {
    return (
      <>
        <div>
          <h2>Number of Apples: {this.props.numOfApples}</h2>
          <input
            type="text"
            value={this.state.number}
            onChange={this.handleChange}
          ></input>
          <button onClick={() => this.props.buyApple(this.state.number)}>
            Buy {this.state.number} Apple(s)
          </button>
        </div>
      </>
    );
  }
}

const mapStateToProps = (state) => {
  console.log(state);
  return {
    numOfApples: state.apple.numOfApples,
  };
};

const mapDispatchToProps = (dispatch) => {
  return {
    buyApple: (number) => dispatch(buyApple(number)),
  };
};

export default connect(mapStateToProps, mapDispatchToProps)(AppleContainer);


```

The changes are pretty basic react which are self-explanatory. In `mapDispatchToProps` we have passed a parameter **number** which states the numbers of apples we want to dispatch and if we are using **functional component without using hooks** (except `useState` you know what I meant :speak_no_evil: ):

``` js

import React from "react";
import { useState } from "react";
import { connect } from "react-redux";
import { buyApple } from "../redux";

function AppleContainer(props) {
  const [number, setNumber] = useState(1);
  return (
    <>
      <div>
        <h2>Number of Apples: {props.numOfApples}</h2>
        <input
          type="text"
          value={number}
          onChange={(e) => setNumber(e.target.value)}
        ></input>
        <button onClick={() => props.buyApple(number)}>
          Buy {number} of Apple(s)
        </button>
      </div>
    </>
  );
}

const mapStateToProps = (state) => {
  console.log(state);
  return {
    numOfApples: state.apple.numOfApples,
  };
};

const mapDispatchToProps = (dispatch) => {
  return {
    buyApple: (number) => dispatch(buyApple(number)),
  };
};

export default connect(mapStateToProps, mapDispatchToProps)(AppleContainer);

```

And if we are using **hooks**, it’s easier as shown below:

``` js

import React, { useState } from "react";
import { useDispatch, useSelector } from "react-redux";
import { buyApple } from "../redux";

function AppleContainer(props) {
  const numberOfCakes = useSelector((state) => state.apple.numOfApples);
  const dispatch = useDispatch();
  const [number, setNumber] = useState(1);
  return (
    <div>
      <h2>Number of Apples: {numberOfCakes} </h2>
      <input
        type="text"
        value={number}
        onChange={(e) => setNumber(e.target.value)}
      ></input>
      <button onClick={() => dispatch(buyApple(number))}>Buy Apple</button>
    </div>
  );
}

export default AppleContainer;

```

Once it is done we will make changes to our `appleAction` file. So after the changes, the file will look something like this:

``` js

import { BUY_APPLE } from "./appleTypes";

export const buyApple = (number = 1) => {
  return {
    type: BUY_APPLE,
    payload: number,
  };
};


```

So we are passing a **default function parameter** named **number** and whose **default value** is 1. And `payload` where the number will be stored. We can name it anything but it’s better to name something relevant like payload.

And now the final step will be changing the `appleReducer` which will be like the following after the changes are done:

``` js

import { BUY_APPLE } from "./appleTypes";

const initialState = {
  numOfApples: 20,
};

const appleReducer = (state = initialState, action) => {
  switch (action.type) {
    case BUY_APPLE:
      return {
        ...state,
        numOfApples: state.numOfApples - action.payload,
      };
    default:
      return state;
  }
};

export default appleReducer;

```

So here we will subtract the `action.payload` in the place of 1. Now we can see that our application works fine and we have successfully implemented our **scenario 2**. 
___

## Scenario 3

We will request to fetch data of some users from a [JSONPlaceholder API](https://jsonplaceholder.typicode.com/users) and it will render as a list in our application if fetching is successful and will show an error message if fetching fails. 

___

## Async Actions

First, we will define our user types inside our `userTypes` as below:

``` js

export const FETCH_USERS_REQUEST = "FETCH_USERS_REQUEST";
export const FETCH_USERS_SUCCESS = "FETCH_USERS_SUCCESS";
export const FETCH_USERS_FAILURE = "FETCH_USERS_FAILURE";

```

Now in `userActions` we will make three action creators as shown below:

``` js

import { FETCH_USERS_REQUEST } from "./userTypes";
import { FETCH_USERS_SUCCESS } from "./userTypes";
import { FETCH_USERS_FAILURE } from "./userTypes";

export const fetchUsersRequest = () => {
  return {
    type: FETCH_USERS_REQUEST,
  };
};

const fetchUsersSuccess = (users) => {
  return {
    type: FETCH_USERS_SUCCESS,
    payload: users,
  };
};

const fetchUsersFailure = (error) => {
  return {
    type: FETCH_USERS_FAILURE,
    payload: error,
  };
};

```

Now in `userReducer` we will create the following reducer:

``` js

import { FETCH_USERS_REQUEST } from "./userTypes";
import { FETCH_USERS_SUCCESS } from "./userTypes";
import { FETCH_USERS_FAILURE } from "./userTypes";

const initialState = {
  loading: false,
  users: [],
  error: "", 
};

const reducer = (state = initialState, action) => {
  switch (action.type) {
    case FETCH_USERS_REQUEST:
      return {
        ...state,
        loading: true,
      };
    case FETCH_USERS_SUCCESS:
      return {
        loading: false,
        users: action.payload,
        error: "",
      };
    case FETCH_USERS_FAILURE:
      return {
        loading: false,
        users: [],
        error: action.payload,
      };
    default:
      return state;
  }
};

export default reducer;

``` 

In `index.js` inside the `redux` folder we will export all the actions:

``` js

export * from "./user/userActions";

```

And add our reducer in our `rootReducer` file:

``` js

import { combineReducers } from "redux";
import userReducer from "./user/userReducer";

const rootReducer = combineReducers({
  user: userReducer,
});

export default rootReducer;

```
___

## Redux Thunk Get Request

Before starting making our store we will install two libraries `axios` and [`redux-thunk`](https://www.npmjs.com/package/redux-thunk). To do so we will write the below command in our terminal:

``` shell

npm install axios redux-thunk

```

Once it is done we will make our store:

``` js

import { createStore, applyMiddleware } from "redux";
import rootReducer from "./rootReducer";
import thunk from "redux-thunk";

const store = createStore(rootReducer, applyMiddleware(thunk));

export default store;

```

As we have seen before now we will pass another parameter with `rootReducer`. So we will pass `applyMiddleware` where our middleware is `thunk`.

**So what does Redux Thunk do?**

> Redux Thunk middleware allows you to write action creators that return a function instead of an action. The thunk can be used to delay the dispatch of action or to dispatch only if a certain condition is met. The inner function receives the store methods dispatch and getState as parameters.

Now we will make the following changes in `userActions`:

``` js

import axios from "axios";
import { FETCH_USERS_REQUEST } from "./userTypes";
import { FETCH_USERS_SUCCESS } from "./userTypes";
import { FETCH_USERS_FAILURE } from "./userTypes";

export const fetchUsersRequest = () => {
  return {
    type: FETCH_USERS_REQUEST,
  };
};

const fetchUsersSuccess = (users) => {
  return {
    type: FETCH_USERS_SUCCESS,
    payload: users,
  };
};

const fetchUsersFailure = (error) => {
  return {
    type: FETCH_USERS_FAILURE,
    payload: error,
  };
};

export const fetchUsers = () => {
  return (dispatch) => {
    dispatch(fetchUsersRequest);
    axios
      .get("https://jsonplaceholder.typicode.com/users")
      .then((response) => {
        const users = response.data;
        dispatch(fetchUsersSuccess(users));
      })
      .catch((error) => {
        const errorMsg = error.message;
        dispatch(fetchUsersFailure(errorMsg));
      });
  };
};

```
`fetchUsers` is returning another function by getting help from our thunk middleware. We did **axios request** as we do in our react application. We used `dispatch(fetchUsersRequest)` to set loading to `true` as when the data is fetching it will show loading in the screen and `dispatch(fetchUsersSuccess(users))` to set our list of users in our fetchUsersSuccess’s payload as a list and  `dispatch(fetchUsersFailure(errorMsg))` to store the error message in fetchUsersFailure’s payload.

Now as everything is done we will create the `UserContainer.jsx`:

``` js

import React, { useEffect } from "react";
import { connect } from "react-redux";
import { fetchUsers } from "../redux";

function UserContainer({ fetchUsers, userData }) {
  useEffect(() => {
    fetchUsers();
  }, []);
  return userData.loading ? (
    <h3>Loading...</h3>
  ) : userData.error ? (
    <h3>{userData.error}</h3>
  ) : (
    <div>
      <h2>Users List</h2>
      <div>
        {userData &&
          userData.users &&
          userData.users.map((user) => <p>{user.name}</p>)}
      </div>
    </div>
  );
}

const mapStateToProps = (state) => {
  return {
    userData: state.user,
  };
};

const mapDispatchToProps = (dispatch) => {
  return {
    fetchUsers: () => dispatch(fetchUsers()),
  };
};

export default connect(mapStateToProps, mapDispatchToProps)(UserContainer);

```
I believe if we followed through the whole cribsheet and know react, the above code is pretty self-explanatory. 
___
