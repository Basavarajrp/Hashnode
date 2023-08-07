---
title: "Architecting Custom State Management in React"
seoTitle: "Building Custom State Management in React: A Step-by-Step Guide"
seoDescription: "Create Custom State Management like Redux and Zustand in React: A Step-by-Step Guide."
datePublished: Sat Aug 05 2023 20:37:54 GMT+0000 (Coordinated Universal Time)
cuid: clkyh83vc000109jodozp9wom
slug: architecting-custom-state-management-in-react
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1691258558078/edd297e0-2c0d-43d0-8655-aa2054143f6e.png
ogImage: https://cdn.hashnode.com/res/hashnode/image/upload/v1691267602391/056bb974-4139-469c-87c6-92b5977253f1.png
tags: js, reactjs, frontend-development, nextjs, reacthooks

---

# Introduction

**Hello everyone! 👋,** I was just exploring state management tools in React JS and thought of replicating the same using core React APIs. So here I am in front of you all with this new blog.

While well-established libraries like Redux and Zustand offer powerful solutions, understanding the core principles behind them can deepen your grasp of React's capabilities. In this blog, we'll embark on a journey to construct a state management system using a custom hook called `useCustomeState`.

---

***Let's Go 🚀***

# What is State Management?

State management lies at the heart of every modern web application, enabling efficient data sharing and synchronization among different parts of the user interface.

In today's web apps, we split the design into smaller parts, like building blocks, called components. These components come together to make a complete web page. Now, to handle information for all these components, we need a storage place. This storage lets components grab the data and show it to users on the page.

And when this information changes, the component automatically updates on the page, without needing to reload the whole page.

# Basic functionality of State Management

We'll create a central store to store and manage data. This data will be accessible to components that have subscribed and depend on it.

Additionally, when the state changes, the subscribed components should receive notifications to update the component with the new state value.

---

<div data-node-type="callout">
<div data-node-type="callout-emoji">💡</div>
<div data-node-type="callout-text">Before we begin, it's important to have a clear understanding of React Custom Hooks. For more information, you can refer to the official React <a target="_blank" rel="noopener noreferrer nofollow" href="https://react.dev/learn/reusing-logic-with-custom-hooks" style="pointer-events: none">documentation</a> on this topic.</div>
</div>

# Steps

1. Create the Central Store to update and access the data.
    
2. Create the Custome Hook to inject into the components, if any state change occurs the components will be notified and updated.
    
3. Use the Hooks in the required components.
    

# Create the central store.

1. Create react project:
    
    `npx create-react-app custom-state-app`
    
2. Inside your project directory, create a `lib` folder. In this folder, create a new file named `customState.js`
    
3. Let's create the central store.
    

```javascript
const createStore = (initialState = {}) => {
  let state = initialState;
  const subscribers = [];

  const getState = () => state;

  const updateState = (newState) => {
    state = { ...state, ...newState };
    subscribers.forEach((subscriber) => subscriber(state));
  };

  const subscribe = (callback) => {
    subscribers.push(callback);
    return () => {
      const index = subscribers.indexOf(callback);
      if (index !== -1) subscribers.splice(index, 1);
    };
  };

  return { getState, updateState, subscribe };
};
```

1. In this above code, we have created the store where the data is managed with a few helper functions like `getState` to read the data and `setState` to update the data
    
2. If you take a closer look, you'll notice that we've also created another function called the `subscribe` function. This function is what enables a component to subscribe to state changes.
    
3. Also in the `setState` the function we are looping into the `subscribers` array and executing the registered callback functions on every state change.
    

<mark>What's important to grasp about the function above is that it contains three key helper functions:</mark>

1. `getState`<mark>: This function allows us to read the data from the global state.</mark>
    
2. `setState`<mark>: Used to update the data within the global state.</mark>
    
3. `subscribe`<mark>: This function stores references to callback functions defined in the components. These callbacks are executed whenever there's a change in the state.</mark>
    

---

# 🪝Create a custom hook to read, update and subscribe components to the global state.

**This is the step where all the magic happens, let's understand the importance of this custom hook.**

<mark>Thing's to understand from this custom hook:</mark>

1. How does this `useCustomeState` hook help to update the component or re-render the component whenever the state value changes?
    
2. How does this `useCustomeState` custom hook act as the messenger between the components that use this hook and the `createStore`(global data store)?
    
3. How this hook binds the component to the `createStore`(global store)?
    

**Let's create the hook to inject them into the components.**

```javascript
import { useEffect } from 'react';

// hook that subscribes and updates the child component.
const useCustomeState = (store) => {
  const [state, setState] = useState(store.getState());

  const handleStateChange = (newState) => {
    setState(newState);
  };

  // run the side effect on the state
  useEffect(() => {
    const unsubscribe = store.subscribe(handleStateChange);
    return () => {
      unsubscribe();
    };
  }, []);

  return { state, setState };
};

export { createStore, useCustomeState };
```

In this above code, we created a hook that takes the store object (`{ getState, setState, subscribe }`) returned from the `createStore()` function.

The point we need to understand from the above code is, we pass the initial state value coming from the `createStore` function itself.

But what it additionally does is, it runs the side effect using `useEffect` , whenever the state changes it passes the callback function and stores them in the `subscribers` array.

**🤔 <mark>What is this callback function?</mark>**

1. When we use these `useCustomState` hooks in components, we're essentially injecting the code from the `useCustomState` function directly inside the components themselves.
    
2. When these components are executed, the `state` and the `useEffect` defined in the hooks also come into play.
    
3. <mark>This means that the callback functions we pass to the store's subscribe function are individually defined within these components. We then simply hand over references to these functions and store them in the </mark> `subscribers` <mark>array.</mark>
    
4. Whenever the state changes, the program goes through the `subscribers` array and triggers these callback functions that were defined. As a result, the state gets updated in the component's local state that uses `useCustomState` hook, leading the component to refresh and display the new state value.
    

This is how the components subscribe to the global state created by the createStore function using the useCustomState hook as a messenger.

**<mark>Here's a consolidated response to the top three questions that we asked ourselves on </mark>** `useCustomState` **<mark>hook.</mark>**

In the `useEffect` hook, the `store.subscribe` method is employed to link the `handleStateChange` function with any alterations to the global state. This implies that whenever a change occurs in the global state (for instance, through the `updateState` function of the global store), the `handleStateChange` function will be invoked.

When the `handleStateChange` function is triggered (owing to modifications in the global state), it updates the local state of the component using the `setState` function. Consequently, this action initiates a re-render of the component, causing the new global state to be visually reflected in the user interface.

---

# Create the components and inject the hook.

### Let's build a counter application in React that uses our custom state management library for managing the counter state just like using Redux and Zustand.

We're going to make two components, A and B. These components will use our custom hook to read and update data in a global state. When we change something in the global state, the fresh and updated value should automatically show up on the page in both components.

Let's put all the code together and create the components.

**Hook and createStore code:**

```javascript
// lib/customeState.js
import { useEffect, useState } from "react";

const createStore = (initialState = {}) => {
  let state = initialState;
  const subscribers = [];

  const getState = () => state;

  const updateState = (newState) => {
    state = { ...state, ...newState };
    subscribers.forEach((subscriber) => subscriber(state));
  };

  const subscribe = (callback) => {
    subscribers.push(callback);
    return () => {
      const index = subscribers.indexOf(callback);
      if (index !== -1) subscribers.splice(index, 1);
    };
  };

  return { getState, updateState, subscribe };
};

// hook that subscribes and updates the component.
const useCustomeState = (store) => {
  const [state, setState] = useState(store.getState());

  // this function is passed as the callback to subscribe function
  const handleStateChange = (newState) => {
    setState(newState);
  };

  // run the side effect on the state
  useEffect(() => {
    const unsubscribe = store.subscribe(handleStateChange);
    return () => {
      unsubscribe();
    };
  }, []);

  return { state, setState };
};
export { createStore, useCustomeState };
```

<mark>In this above code, we just created the </mark> `store` <mark>and </mark> `useCustomeState` <mark>on a single page and exported both functions.</mark>

---

Let's create and initialize the store object to use that in the components.

```javascript
// store.js

import { createStore } from "./customState";

const initialState = { counter: 0 };
const store = createStore(initialState);

export default store;
```

---

**Component A:**

```javascript
// components/ComponentA.js

import React from "react";
import { useCustomeState } from "../lib/customState";
import store from "../lib/store";

const ComponentA = () => {
  const { state } = useCustomeState(store);

  const incrementCounter = () => {
    const newCounterValue = state.counter + 1;
    store.updateState({ counter: newCounterValue });
  };

  return (
    <div>
      <h2>Component A</h2>
      <p>Counter: {state.counter}</p>
      <button onClick={incrementCounter}>Increment Counter</button>
    </div>
  );
};

export default ComponentA;
```

**Component B:**

```javascript
// components/ComponentB.js

import React from "react";
import { useCustomeState } from "../lib/customState";
import store from "../lib/store";

const ComponentB = () => {
  const { state } = useCustomeState(store);

  return (
    <div>
      <h2>Component B</h2>
      <p>Counter: {state.counter}</p>
    </div>
  );
};

export default ComponentB;
```

---

**App.js :**

```javascript
// App.js

import React from "react";
import ComponentA from "./components/ComponentA";
import ComponentB from "./components/ComponentB";

function App() {
  return (
    <div className="App">
      <h1>Counter App</h1>
      <ComponentA />
      <ComponentB />
    </div>
  );
}

export default App;
```

# 🤔 Why we should not directly update the state values, but rather use the function to update the state?

**<mark>Now we can also correlate and understand why we should always use the function to update the state value while using the </mark>** `useState` **<mark>hook in react.</mark>**

The reason is if we update the state value directly react cannot re-render the component with the new state values.

In our example the `ComponentA` which uses `updateState` funciton from the `createStore` also triggers the `componentB` to re-render with the latest values, by looping the subscriber array and executing the callback functions defined inside the components.

```javascript
const setState = (newState) => {
    state = { ...state, ...newState };
    subscribers.forEach((subscriber) => subscriber(state));
  };
```

If we don't use the `updateState` function to modify the state, the state value may change, but the components subscribed to this state change will not receive notifications.

<mark>Once you run this counter app, you'll notice that components A and B are subscribed to the global store. While the state update occurs only in </mark> `componentA`<mark>, but both </mark> `componentA` <mark>and </mark> `componentB` <mark>receives the latest values. This behavior occurs because both components are subscribed to the global store using the </mark> `useCustomState` <mark>hook. As a result, any state change triggers updates and re-renders in both components.</mark>

# Conclusion:

I tried my best to explain and make you all understand on building your custom own state management library using React.

If you have any questions or regards please feel free to reach out to me on social networks, and let's have a chat there on this topic.

It's time to say bye now 👋. See you all in the next one, until that keep writing some code.

Happy Coding 👨‍💻.

# Resources:

Source Code: [GitHub](https://github.com/Basavarajrp/custom-state-app/tree/main)