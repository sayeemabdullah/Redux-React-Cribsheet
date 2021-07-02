# Redux-React-Cribsheet

> Redux is a JavaScript state management library. It can be used with libraries like React and Angular. But in this cribsheet, we will use it with [React](https://github.com/sayeemabdullah/React-Cribsheet#readme). This is an initiative to make small notes on React which may help me or anyone else in the future. You can also checkout my [React](https://github.com/sayeemabdullah/React-Cribsheet#readme) and [CSS](https://github.com/sayeemabdullah/CSS-Cribsheet) Cribsheets. 


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
So the basic structure of the `AppleContainer` will be something like the following: 

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

## Connect without Hooks

After connecting in the `AppleContainer` the file will look something like this:

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
So we can see we have used `mapStateToProps` and `mapDispatchToProps`. Firstly `mapStateToProps` is used when we want to access the **redux state** in our component. Here we get **redux state** as a parameter that can be used. If we want to dispatch any function we use `mapDispatchToProps`. And we connect both of them with our component using `connect`.

___

## Connect with Hooks

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

We will update our scenario one a little bit. We will add a input where we can put numbers of apple we want to buy and it will decrease accordingly. 

___

