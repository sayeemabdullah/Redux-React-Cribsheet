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

