# React Hooks

## What are React Hooks?

**Hooks** are special functions provided by React that allow functional components to use React features such as:

- State
- Side effects
- Context
- References
- Performance optimizations
- Complex state management

Hooks were introduced in **React 16.8**.

Before Hooks, state and lifecycle-related functionality was primarily handled using **class components**.

With Hooks, we can use these features directly inside functional components.

---

# Why Do We Use Hooks?

Hooks make React components:

- Easier to write
- Easier to understand
- Easier to reuse
- Easier to test
- Less dependent on class components
- Better organized

For example, without `useState`, changing a normal JavaScript variable does not tell React that the UI needs to be updated.

```jsx
function Counter() {
    let count = 0;

    const increment = () => {
        count++;
    };

    return (
        <>
            <h2>{count}</h2>

            <button onClick={increment}>
                Increment
            </button>
        </>
    );
}
```

Changing `count` does not cause React to re-render the component.

Using `useState` solves this problem:

```jsx
import { useState } from "react";

function Counter() {
    const [count, setCount] = useState(0);

    return (
        <>
            <h2>{count}</h2>

            <button onClick={() => setCount(count + 1)}>
                Increment
            </button>
        </>
    );
}

export default Counter;
```

Now React knows that the state has changed and re-renders the component.

---

# Common React Hooks

| Hook | Purpose |
|---|---|
| `useState` | Store and update component state |
| `useEffect` | Perform side effects |
| `useContext` | Access shared data |
| `useRef` | Store references or values without causing re-render |
| `useReducer` | Manage complex state |
| `useMemo` | Cache calculated values |
| `useCallback` | Cache functions |
| `useLayoutEffect` | Run an effect before browser painting |

---

# 1. useState

`useState` is used to store data that can change during the lifetime of a component.

## Example

```jsx
import { useState } from "react";

function Counter() {

    const [count, setCount] = useState(0);

    return (
        <>
            <h2>Count: {count}</h2>

            <button onClick={() => setCount(count + 1)}>
                Increment
            </button>
        </>
    );
}

export default Counter;
```

### How it works

```jsx
const [count, setCount] = useState(0);
```

Here:

```text
count      → Current state value
setCount   → Function used to update the state
0          → Initial value
```

When we call:

```jsx
setCount(count + 1);
```

React updates the state and re-renders the component.

### Common Uses

- Counter
- Form fields
- Dropdown
- Modal
- Search box
- Selected item
- Toggle button

### Remember

```text
useState = Component Memory
```

---

# 2. useEffect

`useEffect` is used to perform **side effects** in a React component.

Examples of side effects:

- API calls
- Timers
- Event listeners
- Subscriptions
- Browser APIs

## Example - API Call

```jsx
import { useEffect, useState } from "react";

function Users() {

    const [users, setUsers] = useState([]);

    useEffect(() => {

        fetch("https://jsonplaceholder.typicode.com/users")
            .then(response => response.json())
            .then(data => setUsers(data));

    }, []);

    return (
        <ul>
            {users.map(user => (
                <li key={user.id}>
                    {user.name}
                </li>
            ))}
        </ul>
    );
}

export default Users;
```

The empty dependency array:

```jsx
[]
```

means the effect runs when the component is mounted.

### Common Uses

```text
Component loads
      ↓
useEffect
      ↓
API call
      ↓
Receive data
      ↓
setUsers()
      ↓
Component re-renders
```

### Remember

```text
useEffect = Perform side effects
```

---

# 3. useContext

`useContext` is used when multiple components need access to the same data.

For example:

```text
App
 ├── Header
 ├── Sidebar
 └── Dashboard
```

Suppose all these components need the logged-in user.

Instead of passing the user through multiple components using props, we can use Context.

## Example

```jsx
import { createContext, useContext } from "react";

const UserContext = createContext();

function App() {

    const user = {
        name: "Nikhil",
        role: "Admin"
    };

    return (
        <UserContext.Provider value={user}>
            <Dashboard />
        </UserContext.Provider>
    );
}

function Dashboard() {

    const user = useContext(UserContext);

    return (
        <h2>
            Welcome {user.name}
        </h2>
    );
}

export default App;
```

The `Dashboard` component can directly access the user.

### Without Context

```text
App
 ↓
Dashboard
 ↓
UserProfile
 ↓
UserDetails
```

We may need to pass the user through multiple components.

### With Context

```text
UserContext
     ↓
Dashboard
     ↓
useContext()
     ↓
User
```

### Common Uses

- Logged-in user
- Theme
- Language
- Application settings
- Authentication information

### Remember

```text
useContext = Share data between components
```

---

# 4. useRef

`useRef` is commonly used to:

1. Access DOM elements
2. Store a value without causing a re-render

## Example - Access Input

```jsx
import { useRef } from "react";

function Login() {

    const inputRef = useRef();

    const focusInput = () => {
        inputRef.current.focus();
    };

    return (
        <>
            <input ref={inputRef} />

            <button onClick={focusInput}>
                Focus Input
            </button>
        </>
    );
}

export default Login;
```

Here:

```jsx
<input ref={inputRef} />
```

connects the input element with the reference.

Then:

```jsx
inputRef.current.focus();
```

focuses the input.

### Common Uses

- Focus input
- Access DOM element
- Store timer ID
- Store previous value
- Store mutable values without re-rendering

### Remember

```text
useRef = Reference / Persistent Value
```

---

# 5. useReducer

`useReducer` is useful when state management becomes more complicated.

For example, instead of managing many related states:

```jsx
const [name, setName] = useState("");
const [email, setEmail] = useState("");
const [loading, setLoading] = useState(false);
const [error, setError] = useState("");
```

we can use a reducer.

## Basic Example

```jsx
import { useReducer } from "react";

const initialState = {
    count: 0
};

function reducer(state, action) {

    switch (action.type) {

        case "increment":
            return {
                count: state.count + 1
            };

        case "decrement":
            return {
                count: state.count - 1
            };

        default:
            return state;
    }
}

function Counter() {

    const [state, dispatch] = useReducer(
        reducer,
        initialState
    );

    return (
        <>
            <h2>{state.count}</h2>

            <button
                onClick={() =>
                    dispatch({ type: "increment" })
                }
            >
                +
            </button>

            <button
                onClick={() =>
                    dispatch({ type: "decrement" })
                }
            >
                -
            </button>
        </>
    );
}

export default Counter;
```

### Remember

```text
useReducer = Manage complex state
```

---

# 6. useMemo

`useMemo` is used to cache the result of an expensive calculation.

## Example

```jsx
const total = useMemo(() => {
    return calculateTotal(products);
}, [products]);
```

React will recalculate the value when `products` changes.

### Why use it?

Suppose calculating something is expensive:

```text
Component renders
       ↓
Expensive calculation
       ↓
Component renders again
       ↓
Same expensive calculation
       ↓
Unnecessary work
```

`useMemo` can remember the previous result.

```text
products changed?
      │
   ┌──┴──┐
  Yes    No
   ↓      ↓
Calculate Use cached value
```

### Remember

```text
useMemo = Remember calculated value
```

> Do not use `useMemo` everywhere. It is mainly a performance optimization.

---

# 7. useCallback

`useCallback` is used to cache a function.

## Example

```jsx
const handleClick = useCallback(() => {
    console.log("Clicked");
}, []);
```

The function reference can be reused between renders.

It is particularly useful when passing functions to child components.

### Remember

```text
useCallback = Remember a function
```

---

# useMemo vs useCallback

This is a common interview question.

| Hook | Caches |
|---|---|
| `useMemo` | A calculated value |
| `useCallback` | A function |

Example:

```jsx
const total = useMemo(() => {
    return calculateTotal(products);
}, [products]);
```

`useMemo` returns a **value**.

```jsx
const handleClick = useCallback(() => {
    console.log("Clicked");
}, []);
```

`useCallback` returns a **function**.

---

# Hooks - Easy Way to Remember

```text
useState
   ↓
Store data

useEffect
   ↓
Perform side effects

useContext
   ↓
Share data

useRef
   ↓
Reference / persistent value

useReducer
   ↓
Complex state management

useMemo
   ↓
Remember calculated value

useCallback
   ↓
Remember function
```

---

# Which Hooks Should You Learn First?

For beginners, learn Hooks in this order:

```text
1. useState
      ↓
2. useEffect
      ↓
3. useContext
      ↓
4. useRef
      ↓
5. useReducer
      ↓
6. useMemo
      ↓
7. useCallback
```

You will use `useState` and `useEffect` very frequently in normal React applications.

---

# Real-World Example

Consider an e-commerce application.

```text
Product Page
│
├── useState
│      └── Store selected quantity
│
├── useEffect
│      └── Load product from API
│
├── useContext
│      └── Access logged-in user/cart
│
├── useRef
│      └── Focus search/input
│
├── useReducer
│      └── Manage complex cart state
│
├── useMemo
│      └── Calculate cart total
│
└── useCallback
       └── Cache cart event handlers
```

---

# Rules of Hooks

There are two important rules.

## Rule 1 - Only call Hooks at the top level

Do not call Hooks inside:

- `if`
- `for`
- `while`
- Nested functions

Incorrect:

```jsx
if (isLoggedIn) {
    const [user, setUser] = useState(null);
}
```

Correct:

```jsx
const [user, setUser] = useState(null);

if (isLoggedIn) {
    // Use user here
}
```

---

## Rule 2 - Only call Hooks from React functions

Hooks should be called from:

- React functional components
- Custom Hooks

Example:

```jsx
function Counter() {

    const [count, setCount] = useState(0);

    return <h2>{count}</h2>;
}
```

---

# What Problem Do Hooks Solve?

Before Hooks:

```text
Functional Components
       ↓
Limited state/lifecycle functionality

Class Components
       ↓
State + Lifecycle
       ↓
More complex syntax
```

With Hooks:

```text
Functional Component
       ↓
React Hooks
       ↓
State
Effects
Context
Refs
Performance
       ↓
Simple and reusable code
```

---

# Quick Interview Questions

### 1. What are React Hooks?

Hooks are special React functions that allow functional components to use features such as state, effects, context and refs.

### 2. Why were Hooks introduced?

Hooks were introduced to make it easier to use state and other React features inside functional components and to improve code reuse.

### 3. What is `useState`?

`useState` is used to create and manage state inside a functional component.

### 4. What is `useEffect`?

`useEffect` is used to perform side effects such as API calls, timers and event subscriptions.

### 5. What is `useContext`?

`useContext` allows components to access shared data without passing props through every component.

### 6. What is `useRef`?

`useRef` is used to reference DOM elements or store values that persist between renders without causing a re-render.

### 7. Difference between `useMemo` and `useCallback`?

`useMemo` caches a calculated value, while `useCallback` caches a function.

### 8. Can Hooks be called inside an `if` statement?

No. Hooks must be called at the top level of a React component or custom Hook.

---

# Summary

React Hooks allow functional components to use powerful React features.

The most important Hooks are:

```text
useState      → State
useEffect     → Side effects
useContext    → Shared data
useRef        → References
useReducer    → Complex state
useMemo       → Cached value
useCallback   → Cached function
```

**Start with `useState` and `useEffect`. Once these are clear, move to `useContext`, `useRef`, and then the performance-related Hooks.**