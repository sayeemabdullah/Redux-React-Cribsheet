# Redux-React-Cribsheet

> Redux is a JavaScript state management library. It can be used with libraries like React and Angular. But in this cribsheet, we will use it with [React](https://github.com/sayeemabdullah/React-Cribsheet#readme).


## Three Building Parts

The three-building parts of Redux are **Store**, **Action** and **Reducer**. In brief, we can say that store holds the state of our application, action describes what happened and the reducer ties the store and the actions together.

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

Different people use different structures but I follow [Vishwas’s](https://github.com/gopinav) one which is something like below: 

![Screenshot 2021-07-01 at 1 28 24 PM](https://user-images.githubusercontent.com/31423599/124083984-527c4400-da70-11eb-99a0-648b4aa2c1d9.png)

So here he made a separate folder for all the redux files and inside the **redux folder** he made separate folders for each container where the container’s **Action**, **Reducer** and **Types** files are kept. As we go through the cribsheet, we will better grasp **redux** and the **folder structure**. 

___


## Scenario

Before starting implementation lets us think of a scenario, so we will make an application that will show **numbers of apple** and if we press a button the **numbers of apple will decrease**.
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

But here we can see that we are importing `BUY_APPLE` from a file named `appleTypes`. Inside the `appleTypes` we will find:

``` js

export const BUY_APPLE = "BUY_APPLE"

```

So we are exporting a const with the same value so that we don’t do any mistakes while typing the `type` parameter. This `appleTypes` is just a part of the file structure. We can also not use it if we want. 

___

