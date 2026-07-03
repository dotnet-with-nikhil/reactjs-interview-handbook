# 🚀 React Interview Questions

Comprehensive GitHub-ready list of React interview topics.

---

# ⭐ 1. Controlled vs Uncontrolled Components

### Interview Questions

- What are Controlled Components?
- What are Uncontrolled Components?
- What are the differences?
- When should you use each approach?
- Real-world examples.

---

## ✅ What are Controlled Components?

A **Controlled Component** is a form element whose value is **controlled by React State**.

React stores the current value using `useState`, and every time the user types, React updates the state through the `onChange` event.

> **Simple Definition**
>
> **React controls the input.**

---

## 🔄 How it Works

```text
User Types
      │
      ▼
 onChange Event
      │
      ▼
 React State Updates
      │
      ▼
 Input Value Updates
```

---

## 💻 Example

```jsx
import { useState } from "react";

function App() {
  const [name, setName] = useState("");

  return (
    <>
      <input
        type="text"
        value={name}
        onChange={(e) => setName(e.target.value)}
      />

      <p>Your Name: {name}</p>
    </>
  );
}

export default App;
```

### Output

```
Input:
John

Output:
Your Name: John
```

Every keystroke updates the React state.

```
J
Jo
Joh
John
```

---

## ✅ Advantages

- Easy form validation
- Easy error handling
- Live UI updates
- Easy to reset forms
- Better control over user input
- Works well with dynamic forms

### Validation Example

```jsx
if (name.length < 3) {
    // Show validation message
}
```

### Disable Submit Button

```jsx
<button disabled={!name}>
    Submit
</button>
```

### Reset Form

```jsx
setName("");
```

---

## 🌍 Real-World Examples

- Login Form
- Registration Form
- Contact Form
- Search Box
- OTP Verification
- Checkout Form
- Multi-step Forms

> **Most React applications use Controlled Components.**

---

## ✅ What are Uncontrolled Components?

An **Uncontrolled Component** stores its own state inside the **DOM** instead of React.

React does not track the input value while the user is typing. Instead, it reads the value only when needed using `useRef()`.

> **Simple Definition**
>
> **The DOM controls the input.**

---

## 🔄 How it Works

```text
User Types
      │
      ▼
 Input Stores Value
      │
      ▼
 React Doesn't Know
      │
      ▼
 Read Value using Ref
```

---

## 💻 Example

```jsx
import { useRef } from "react";

function App() {

  const inputRef = useRef();

  const handleSubmit = () => {
    alert(inputRef.current.value);
  };

  return (
    <>
      <input
        type="text"
        ref={inputRef}
      />

      <button onClick={handleSubmit}>
        Submit
      </button>
    </>
  );
}

export default App;
```

---

### Output

User enters

```
John
```

Nothing happens while typing.

After clicking **Submit**

```
Alert:
John
```

React reads the value only when required.

---

## ✅ Advantages

- Less code
- Slightly better performance for large forms
- Useful with third-party libraries
- No state updates on every keystroke

---

## 🌍 Real-World Examples

- File Upload
- Legacy HTML Forms
- Third-party UI Libraries
- Simple Forms
- Reading values only during form submission

---
# 💻 Code Comparison

## Controlled Component

```jsx
const [email, setEmail] = useState("");

<input
    value={email}
    onChange={(e) => setEmail(e.target.value)}
/>
```

Every key press updates the React state.

---

## Uncontrolled Component

```jsx
const emailRef = useRef();

<input ref={emailRef} />
```

React reads the value only when needed.

---
## 🎯 When Should You Use Controlled Components?

Use Controlled Components when you need:

- Form Validation
- Error Messages
- Live Search
- Live Preview
- Dynamic Forms
- Conditional Rendering
- Multi-step Forms
- API Integration

### Example

Password Strength Checker

```
Password:
abc123

Strength:
Weak
```

The UI updates instantly as the user types.

---

## 🎯 When Should You Use Uncontrolled Components?

Use Uncontrolled Components when:

- File Upload
- Very Simple Forms
- Third-party Libraries
- Performance-sensitive forms
- Reading input values only during submission

### Example

```jsx
<input type="file" />
```

File inputs are naturally **Uncontrolled Components** because browsers do not allow JavaScript to set their value programmatically.

---
## 🌍 Real-World Interview Scenarios

### Scenario 1

#### Interviewer

> You are building a Login Page. Which approach would you use?

#### Answer

**Controlled Components**

#### Why?

- Email validation
- Password validation
- Disable Login button
- Show validation messages
- Easy API integration

---

### Scenario 2

#### Interviewer

> You are building a Resume Upload page. Which approach would you use?

#### Answer

**Uncontrolled Components**

#### Why?

- File inputs are naturally uncontrolled.
- The selected file is only required when submitting the form.

---


# ⭐ 2. Context API

### Interview Questions

- What is Context API?
- How does it avoid Prop Drilling?
- Context API vs Redux.
- When should Context API be used?

---

## ✅ What is Context API?

The **Context API** is a built-in feature of React that allows you to **share data globally** across multiple components without passing props manually.

It is mainly used to share data like:

- Logged-in User
- Theme (Dark/Light)
- Language
- Authentication Status
- Application Settings

> **Simple Definition**
>
> **Context API lets you share data across components without passing props manually.**

---

## 🤔 Why was Context API Introduced?

Imagine you have multiple nested components.

```
App
 │
 ▼
Header
 │
 ▼
Navbar
 │
 ▼
Profile
```

Suppose the **Profile** component needs the logged-in user's name.

Without Context API, you must pass the data through every intermediate component.

```
App
 ↓
Header
 ↓
Navbar
 ↓
Profile
```

Even though **Header** and **Navbar** don't use the data, they still have to pass it down.

This is called **Prop Drilling**.

---

## ❌ What is Prop Drilling?

**Prop Drilling** means passing props from a parent component through multiple intermediate components just so a deeply nested child can access the data.

---

## Example of Prop Drilling

### App.jsx

```jsx
import Header from "./Header";

function App() {
  const user = "Nikhil";

  return <Header user={user} />;
}
```

---

### Header.jsx

```jsx
import Navbar from "./Navbar";

function Header({ user }) {
  return <Navbar user={user} />;
}
```

---

### Navbar.jsx

```jsx
import Profile from "./Profile";

function Navbar({ user }) {
  return <Profile user={user} />;
}
```

---

### Profile.jsx

```jsx
function Profile({ user }) {
  return <h2>Welcome {user}</h2>;
}
```

---

### Component Flow

```text
App
 │ user
 ▼
Header
 │ user
 ▼
Navbar
 │ user
 ▼
Profile
```

Notice that **Header** and **Navbar** don't need the `user` data—they only forward it.

This unnecessary passing of props is called **Prop Drilling**.

---

## ✅ How Does Context API Avoid Prop Drilling?

Instead of passing props through every component, React stores the data inside a **Context Provider**.

Any component inside the Provider can access the data directly using `useContext()`.

---

## Component Flow with Context API

```text
                User Context
                     │
                     ▼
                  App
                /     \
          Header     Footer
             │
          Navbar
             │
          Profile
```

Profile directly accesses the context.

No props are passed through Header or Navbar.

---

## 💻 Step 1: Create Context

```jsx
import { createContext } from "react";

const UserContext = createContext();

export default UserContext;
```

---

## 💻 Step 2: Provide Context

```jsx
import UserContext from "./UserContext";
import Header from "./Header";

function App() {

  const user = "Nikhil";

  return (
    <UserContext.Provider value={user}>
      <Header />
    </UserContext.Provider>
  );
}

export default App;
```

---

## 💻 Step 3: Consume Context

```jsx
import { useContext } from "react";
import UserContext from "./UserContext";

function Profile() {

  const user = useContext(UserContext);

  return <h2>Welcome {user}</h2>;
}
```

---

### Output

```
Welcome Nikhil
```

Notice that neither **Header** nor **Navbar** receives any props.

---

## 🔄 How Context API Works

```text
createContext()
        │
        ▼
Provider stores data
        │
        ▼
Child Components
        │
        ▼
useContext() reads data
```

---

## 🌍 Real-World Examples

Context API is commonly used for:

- 🌙 Dark/Light Theme
- 👤 Logged-in User
- 🌐 Language Selection
- 🔐 Authentication
- 🛒 Shopping Cart Count
- ⚙️ Application Settings

---
# 🎯 When Should You Use Context API?

Use Context API when:

- Theme Switching
- Logged-in User Information
- Language Settings
- Authentication Status
- Application Settings
- Small to Medium Applications
- Data shared by many components

---

# 🚫 When Should You Avoid Context API?

Avoid Context API when:

- Your application has a very large global state.
- Multiple unrelated states are updated frequently.
- You need advanced debugging or middleware.
- Your application has complex business logic.

In such cases, tools like **Redux**, **Redux Toolkit**, or other state management libraries are often a better fit.

---
## 🌍 Real-World Interview Scenarios

### Scenario 1

#### Interviewer

> Your application has a Dark/Light Theme used by every page. What would you use?

#### Answer

**Context API**

Because the theme is shared across the application and doesn't require complex state management.

---

### Scenario 2

#### Interviewer

> Your application has authentication, shopping cart, orders, payments, notifications, and many complex states. What would you use?

#### Answer

**Redux (or Redux Toolkit)**

Because it provides centralized state management, middleware support, DevTools, and scales well for large applications.

---

## 💡 Interview Tip

A strong interview answer is:

> **Context API is React's built-in solution for sharing global data without prop drilling. It is ideal for simple shared state like themes, authentication, or language preferences. For complex applications with large and frequently changing global state, Redux or Redux Toolkit is generally a better choice.**

---


## ⭐ 3. React Lifecycle Methods

### Interview Questions

- Explain Mounting, Updating and Unmounting.
- componentDidMount
- componentDidUpdate
- componentWillUnmount
- Hooks equivalent using useEffect.

---

# ⭐ 3. React Lifecycle Methods

React Lifecycle Methods describe the different stages a component goes through during its lifetime.

Understanding the React lifecycle is one of the most frequently asked React interview topics.

---

# 📌 Interview Questions

- What are React Lifecycle Methods?
- Explain Mounting, Updating, and Unmounting.
- What is `componentDidMount()`?
- What is `componentDidUpdate()`?
- What is `componentWillUnmount()`?
- What is the Hooks equivalent of Lifecycle Methods?
- How does `useEffect()` replace Lifecycle Methods?

---

## 🎯 What is a Component Lifecycle?

Just like a human being has different stages of life:

```
Birth → Growth → Death
```

A React component also goes through different stages.

```
Created
    │
    ▼
Mounted
    │
    ▼
Updated
    │
    ▼
Unmounted
```

These stages are called the **Component Lifecycle**.

---

## 📊 React Lifecycle Phases

There are **three main phases** in a React component's lifecycle.

| Phase | Meaning |
|--------|---------|
| Mounting | Component is created and added to the DOM |
| Updating | Component is re-rendered because data or props changed |
| Unmounting | Component is removed from the DOM |

---

## 1️⃣ Mounting Phase

### What is Mounting?

Mounting is the process of **creating a component and inserting it into the DOM**.

It happens only **once** when the component first appears on the screen.

---

### Real-Life Example

Imagine opening Netflix.

```
Open Netflix
      │
      ▼
Home Page Loads
      │
      ▼
API Call Happens
      │
      ▼
Movies Display
```

The page loads only once.

This is the **Mounting Phase**.

---

## Class Component

```jsx
class Home extends React.Component {

    componentDidMount() {
        console.log("Component Mounted");
    }

    render() {
        return <h1>Home</h1>;
    }
}
```

### Output

```
Component Mounted
```

Runs only once.

---

## Functional Component (Hooks)

```jsx
import { useEffect } from "react";

function Home() {

    useEffect(() => {
        console.log("Component Mounted");
    }, []);

    return <h1>Home</h1>;
}
```

### Why `[]`?

An empty dependency array (`[]`) means:

```
Run only once
```

after the first render.

---

## Common Use Cases

- Calling APIs
- Loading user profile
- Fetching products
- Starting timers
- Loading dashboard data
- Initializing third-party libraries

---

## 2️⃣ Updating Phase

### What is Updating?

Updating happens whenever:

- State changes
- Props change
- Parent component re-renders (and causes this component to re-render)

React updates the UI to reflect the latest data.

---

### Real-Life Example

Shopping Cart

```
Add Product
      │
      ▼
Cart Count Changes
      │
      ▼
Component Updates
```

---

### Class Component

```jsx
class Counter extends React.Component {

    componentDidUpdate() {
        console.log("Component Updated");
    }

    render() {
        return <h1>Counter</h1>;
    }
}
```

Runs after every update.

---

### Functional Component

```jsx
import { useState, useEffect } from "react";

function Counter() {

    const [count, setCount] = useState(0);

    useEffect(() => {
        console.log("Count Updated");
    }, [count]);

    return (
        <>
            <h2>{count}</h2>

            <button onClick={() => setCount(count + 1)}>
                Increment
            </button>
        </>
    );
}
```

### Output

```
Click Button

Count Updated

Click Again

Count Updated
```

The effect runs whenever `count` changes.

---

### Common Use Cases

- Search suggestions
- Updating charts
- Fetching data when filters change
- Updating page title
- Saving form changes automatically

---

## 3️⃣ Unmounting Phase

### What is Unmounting?

Unmounting happens when a component is **removed from the DOM**.

This is the last stage of the component lifecycle.

---

### Real-Life Example

Imagine leaving a chat application.

```
Open Chat
      │
      ▼
Receive Messages
      │
      ▼
Close Chat
      │
      ▼
Stop Listening
```

When the chat closes, resources should be cleaned up.

---

### Class Component

```jsx
class Timer extends React.Component {

    componentWillUnmount() {
        console.log("Component Removed");
    }

    render() {
        return <h1>Timer</h1>;
    }
}
```

---

### Functional Component

```jsx
import { useEffect } from "react";

function Timer() {

    useEffect(() => {

        console.log("Timer Started");

        return () => {
            console.log("Timer Stopped");
        };

    }, []);

    return <h1>Timer</h1>;
}
```

When the component is removed:

```
Timer Stopped
```

---

### Common Use Cases

- Clear timers
- Remove event listeners
- Close WebSocket connections
- Cancel API requests
- Stop subscriptions

---

## 🔄 Complete Lifecycle Flow

```text
Component Created
        │
        ▼
Mounting
(componentDidMount)
(useEffect(() => {}, []))
        │
        ▼
Updating
(componentDidUpdate)
(useEffect(() => {}, [dependency]))
        │
        ▼
Unmounting
(componentWillUnmount)
(return () => {})
```

---

## 🪝 Hooks Equivalent

| Class Component | Functional Component |
|-----------------|----------------------|
| `componentDidMount()` | `useEffect(() => {}, [])` |
| `componentDidUpdate()` | `useEffect(() => {}, [dependency])` |
| `componentWillUnmount()` | `return () => {}` inside `useEffect()` |

---

## 💻 One Example Covering All Three Phases

```jsx
import { useState, useEffect } from "react";

function Counter() {

    const [count, setCount] = useState(0);

    useEffect(() => {

        console.log("Mounted");

        return () => {
            console.log("Unmounted");
        };

    }, []);

    useEffect(() => {
        console.log("Updated:", count);
    }, [count]);

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

### Console Output

```
Mounted

Updated: 1

Updated: 2

Updated: 3

Unmounted
```

---

## 🌍 Real-World Examples

| Lifecycle | Example |
|------------|----------|
| Mounting | Fetch user profile when page opens |
| Updating | Update search results as the user types |
| Unmounting | Stop timers or WebSocket connections when leaving the page |

---

## 🎯 Interview Scenario

### Interviewer

> Where would you call an API in React?

### Answer

During the **Mounting Phase**.

```jsx
useEffect(() => {

    fetchUsers();

}, []);
```

---

### Interviewer

> Where would you clear a timer?

### Answer

During the **Unmounting Phase**.

```jsx
useEffect(() => {

    const timer = setInterval(() => {

    }, 1000);

    return () => clearInterval(timer);

}, []);
```

---

### Interviewer

> When does `componentDidUpdate()` execute?

### Answer

It executes **after every update** caused by changes in **state or props**.

The Hooks equivalent is:

```jsx
useEffect(() => {

    console.log("Updated");

}, [dependency]);
```

---

## ⚖️ Class Lifecycle vs Hooks

| Lifecycle Stage | Class Component | Functional Component |
|-----------------|-----------------|----------------------|
| Mounting | `componentDidMount()` | `useEffect(() => {}, [])` |
| Updating | `componentDidUpdate()` | `useEffect(() => {}, [dependency])` |
| Unmounting | `componentWillUnmount()` | `return () => {}` inside `useEffect()` |

---

## 📝 Memory Trick

```
M → Mount
U → Update
U → Unmount

M U U
```

Or remember:

```
Born
 ↓
Mounted

Changes
 ↓
Updated

Removed
 ↓
Unmounted
```

---

## 💡 Interview Tip

A strong interview answer is:

> **React components go through three lifecycle phases: Mounting, Updating, and Unmounting. In class components, these are handled using `componentDidMount()`, `componentDidUpdate()`, and `componentWillUnmount()`. In functional components, the `useEffect()` Hook replaces these lifecycle methods by controlling when side effects run and when cleanup occurs.**

---

## ⭐ Quick Revision

| Phase | Class Component | Hooks |
|--------|-----------------|--------|
| Mounting | `componentDidMount()` | `useEffect(() => {}, [])` |
| Updating | `componentDidUpdate()` | `useEffect(() => {}, [dependency])` |
| Unmounting | `componentWillUnmount()` | `return () => {}` |

---

## 🚀 Interview One-Liner

> **React components have three lifecycle phases—Mounting, Updating, and Unmounting. In modern React, the `useEffect()` Hook replaces lifecycle methods by handling initialization, updates based on dependencies, and cleanup when the component is removed.**

---


# ⭐ 4. Virtual DOM

### Interview Questions

- What is Virtual DOM?
- How React updates the UI efficiently?
- DOM vs Virtual DOM.

---

## 🎯 What is DOM?

**DOM (Document Object Model)** is a tree-like representation of an HTML page created by the browser.

Every HTML element becomes a node in the DOM.

For example:

```html
<body>
    <h1>React Interview</h1>
    <button>Click Me</button>
</body>
```

DOM Tree

```text
Body
 ├── h1
 └── Button
```

Whenever JavaScript changes an element, the browser updates the DOM and may need to recalculate layout and repaint the page.

These operations are relatively expensive.

---

## 🎯 What is Virtual DOM?

The **Virtual DOM (VDOM)** is a lightweight JavaScript object that represents the structure of the real DOM.

Instead of updating the browser DOM directly, React first updates the Virtual DOM.

React then compares the old Virtual DOM with the new Virtual DOM and updates only the parts that have changed.

> **Simple Definition**
>
> **Virtual DOM is a lightweight copy of the Real DOM that React uses to update the UI efficiently.**

---

## 🤔 Why Do We Need Virtual DOM?

Imagine a page with 1,000 HTML elements.

If only **one button's text changes**, updating the entire page would be inefficient.

Instead, React identifies the changed element and updates **only that specific part** of the Real DOM.

This makes React applications faster and more efficient.

---

## 🔄 How React Updates the UI Efficiently

Whenever **state** or **props** change, React follows these steps:

```text
State Changes
      │
      ▼
Create New Virtual DOM
      │
      ▼
Compare with Previous Virtual DOM
      │
      ▼
Find Differences (Diffing)
      │
      ▼
Update Only Changed Elements
      │
      ▼
Browser UI Updates
```

---

## 🔍 What is Reconciliation?

**Reconciliation** is React's process of comparing the **old Virtual DOM** with the **new Virtual DOM** to determine what has changed.

React then updates only the affected elements in the Real DOM.

---

## 🔍 What is Diffing?

**Diffing** is the algorithm React uses during reconciliation to compare the old and new Virtual DOM trees.

Its goal is to identify the minimum number of changes required to update the UI.

---

## 💻 Example

```jsx
import { useState } from "react";

function Counter() {

    const [count, setCount] = useState(0);

    return (
        <>
            <h1>{count}</h1>

            <button onClick={() => setCount(count + 1)}>
                Increment
            </button>
        </>
    );
}

export default Counter;
```

---

### Initial UI

```text
0

[Increment]
```

---

### User Clicks Button

```text
1

[Increment]
```

Only the `<h1>` value changes from:

```text
0
```

to

```text
1
```

The button remains unchanged.

React updates only the `<h1>` element instead of rebuilding the entire page.

---

## 🧠 Behind the Scenes

### Old Virtual DOM

```text
App
 ├── h1 → 0
 └── Button
```

---

### New Virtual DOM

```text
App
 ├── h1 → 1
 └── Button
```

---

### React Comparison

```text
h1 Changed ✅
Button Same ❌
```

---

### Actual DOM Update

```text
Update only:

<h1>1</h1>
```

The button is not re-created.

---
## 🌍 Real-World Example

Imagine editing your profile.

Before editing:

```text
Name : John
Email: john@gmail.com
City : Pune
```

You change only the city.

```text
City : Mumbai
```

React updates only the **City** field instead of rebuilding the entire profile page.

---

## ⚖️ DOM vs Virtual DOM

| Feature | DOM | Virtual DOM |
|----------|-----|-------------|
| Created By | Browser | React |
| Type | Actual HTML Elements | JavaScript Objects |
| Update Speed | Slower | Faster |
| Memory Usage | Higher | Lower |
| Updates | Entire DOM operations as needed | Only changed elements are applied to the Real DOM |
| Performance | Less Efficient | More Efficient |

---

## 📊 Visual Comparison

## Traditional DOM

```text
State Changes
      │
      ▼
Update Real DOM
      │
      ▼
Recalculate Layout
      │
      ▼
Repaint Screen
```

---

## React Virtual DOM

```text
State Changes
      │
      ▼
Update Virtual DOM
      │
      ▼
Compare Changes
      │
      ▼
Update Only Changed Nodes
      │
      ▼
Repaint
```

---

## 🚀 Advantages of Virtual DOM

- Faster UI updates
- Better performance
- Efficient rendering
- Fewer direct DOM manipulations
- Better user experience
- Optimized updates using Diffing and Reconciliation

---

## ⚠️ Common Misconception

Many people think:

> **"React never updates the Real DOM."**

This is **incorrect**.

React **does update the Real DOM**, but **only after comparing the old and new Virtual DOMs** and only for the parts that changed.

---

## 🌍 Real-World Interview Scenarios

### Scenario 1

#### Interviewer

> If one item changes in a list of 1,000 items, does React recreate all 1,000 elements?

#### Answer

No.

React compares the old and new Virtual DOM trees, identifies the changed item through the Diffing algorithm, and updates only that specific element in the Real DOM.

---

### Scenario 2

#### Interviewer

> Why is React considered fast?

#### Answer

Because React uses the **Virtual DOM**, **Diffing**, and **Reconciliation** to minimize expensive Real DOM updates.

---

### Scenario 3

#### Interviewer

> Does Virtual DOM replace the Real DOM?

#### Answer

No.

The Virtual DOM is a lightweight copy used for comparison. The browser still displays the **Real DOM**, and React updates it efficiently after determining what changed.

---
# 📝 Memory Trick

```
State Changes
      │
      ▼
Virtual DOM
      │
Compare
      │
      ▼
Update Real DOM
```

Remember:

```
Virtual DOM
      ↓
Compare
      ↓
Update Only Changes
```

---

### 💡 Interview Tip

A strong interview answer is:

> **Virtual DOM is a lightweight JavaScript representation of the Real DOM. When state or props change, React creates a new Virtual DOM, compares it with the previous one using the Diffing algorithm (Reconciliation), and updates only the changed elements in the Real DOM. This minimizes expensive DOM operations and improves performance.**

---



# ⭐ 5. Reconciliation

### Interview Questions

- What is Reconciliation?
- How does React identify changes?
- Explain the Diffing Algorithm.

---

## 🎯 What is Reconciliation?

**Reconciliation** is the process React uses to compare the **old Virtual DOM** with the **new Virtual DOM** and determine the minimum number of changes required to update the **Real DOM**.

Instead of rebuilding the entire UI, React updates only the parts that have changed.

> **Simple Definition**
>
> **Reconciliation is React's process of comparing two Virtual DOM trees and updating only the changed elements in the Real DOM.**

---

## 🤔 Why Do We Need Reconciliation?

Imagine a webpage with **500 elements**.

If only **one button's text changes**, rebuilding the entire page would be slow and inefficient.

Instead, React:

- Creates a new Virtual DOM.
- Compares it with the previous Virtual DOM.
- Finds the differences.
- Updates only the changed elements.

This makes React applications much faster.

---

## 🔄 Reconciliation Process

Whenever **state** or **props** change:

```text
State / Props Change
        │
        ▼
Create New Virtual DOM
        │
        ▼
Compare with Previous Virtual DOM
        │
        ▼
Find Differences (Diffing)
        │
        ▼
Update Only Changed Nodes
        │
        ▼
Real DOM Updated
```

---

## 💻 Example

```jsx
import { useState } from "react";

function Counter() {

    const [count, setCount] = useState(0);

    return (
        <>
            <h1>{count}</h1>

            <button onClick={() => setCount(count + 1)}>
                Increment
            </button>
        </>
    );
}

export default Counter;
```

---

### Initial Render

```text
0

[Increment]
```

---

### After Clicking Button

```text
1

[Increment]
```

React notices:

```
Old Value = 0

New Value = 1
```

Only the `<h1>` changes.

The button remains the same.

---

## 🧠 Behind the Scenes

### Old Virtual DOM

```text
App
 ├── h1 → 0
 └── Button
```

---

### New Virtual DOM

```text
App
 ├── h1 → 1
 └── Button
```

---

## Comparison

```text
h1      Changed ✅

Button  Same ❌
```

---

## Final Update

```text
Only update:

<h1>1</h1>
```

Everything else remains unchanged.

---

## 🔍 How Does React Identify Changes?

React identifies changes by comparing:

- Element Type
- Element Position
- Props
- Children
- Keys (for lists)

This comparison is performed using the **Diffing Algorithm**.

---

## 🎯 What is the Diffing Algorithm?

The **Diffing Algorithm** is React's algorithm for comparing the **old Virtual DOM** with the **new Virtual DOM**.

Its goal is to determine the **minimum number of changes** required to update the Real DOM.

Instead of comparing every node in the most expensive way, React uses a fast heuristic approach.

---

## 🧩 Rule 1: Different Element Types

If the element type changes, React destroys the old element and creates a new one.

### Example

Old

```jsx
<h1>Hello</h1>
```

New

```jsx
<p>Hello</p>
```

Since `h1` and `p` are different element types:

```
Remove h1

Create p
```

---

## 🧩 Rule 2: Same Element Type

If the element type is the same, React updates only the changed attributes or content.

### Example

Old

```jsx
<h1>Hello</h1>
```

New

```jsx
<h1>Welcome</h1>
```

Only the text changes.

React updates only the text node.

---

## 🧩 Rule 3: List Rendering (Keys)

Lists are a common interview topic.

Without **keys**, React has difficulty identifying which item changed.

### Without Keys

```jsx
<li>Apple</li>
<li>Mango</li>
<li>Orange</li>
```

Insert Banana at the beginning.

```jsx
<li>Banana</li>
<li>Apple</li>
<li>Mango</li>
<li>Orange</li>
```

React may think every item has changed.

---

### With Keys

```jsx
items.map(item =>

    <li key={item.id}>
        {item.name}
    </li>

)
```

Now React knows exactly which item is new.

Only the new item is inserted.

---

## 🌍 Real-World Example

Imagine a shopping cart.

Before:

```
Apple

Orange

Mango
```

User adds:

```
Banana
```

Instead of recreating the entire list,

React simply inserts:

```
Banana
```

The remaining items stay unchanged.

---

## ⚖️ Reconciliation vs Diffing

| Reconciliation | Diffing |
|----------------|----------|
| Overall process of updating the UI | Algorithm used during reconciliation |
| Compares old and new Virtual DOM | Finds differences between nodes |
| Updates the Real DOM | Decides what should change |
| Uses the Diffing Algorithm | Part of Reconciliation |

---

## 🚀 Advantages of Reconciliation

- Faster rendering
- Better performance
- Fewer DOM updates
- Better user experience
- Efficient handling of UI changes
- Optimized list rendering with keys

---

## 🌍 Real-World Interview Scenarios

### Scenario 1

#### Interviewer

> If only one button changes on a page containing 1,000 elements, what happens?

#### Answer

React performs **Reconciliation**, compares the old and new Virtual DOM using the **Diffing Algorithm**, and updates only the changed button instead of rebuilding the entire page.

---

## Scenario 2

#### Interviewer

> Why are `key` props important in React lists?

#### Answer

Keys help React uniquely identify list items during the Diffing process.

This allows React to update, insert, or remove only the affected items instead of re-rendering the whole list.

---

## Scenario 3

#### Interviewer

> What happens if an element changes from `<h1>` to `<p>`?

#### Answer

Since the element types are different, React removes the old `<h1>` element and creates a new `<p>` element.

---

## 📝 Memory Trick

```
State Changes
      │
      ▼
Virtual DOM
      │
      ▼
Reconciliation
      │
      ▼
Diffing
      │
      ▼
Update Real DOM
```

Remember:

```
Reconciliation
        │
Uses
        ▼
Diffing
        │
Updates
        ▼
Real DOM
```

---

## 💡 Interview Tip

A strong interview answer is:

> **Reconciliation is React's process of comparing the old and new Virtual DOM whenever state or props change. During this process, React uses the Diffing Algorithm to identify the minimum set of changes required and updates only the affected elements in the Real DOM, resulting in efficient rendering and improved performance.**

---

## ⭐ Quick Revision

| Topic | Summary |
|--------|---------|
| Reconciliation | Process of comparing Virtual DOM trees |
| Diffing | Algorithm used during Reconciliation |
| Purpose | Identify minimal UI changes |
| Updates | Only changed elements in the Real DOM |
| List Optimization | Uses `key` prop |
| Benefit | Faster rendering and better performance |

---



# ⭐ 6. React Hooks

React Hooks are one of the most important topics in React interviews.

Hooks allow **Functional Components** to use features like **state, lifecycle methods, context, refs, and performance optimizations** without writing Class Components.

> **Simple Definition**
>
> **Hooks are special React functions that let Functional Components use React features such as state, lifecycle methods, context, and refs.**

---

## 📌 Interview Questions

- What are React Hooks?
- Why were Hooks introduced?
- Explain `useState()`.
- Explain `useEffect()`.
- Explain `useRef()`.
- Explain `useMemo()`.
- Explain `useCallback()`.
- Explain `useContext()`.
- What happens internally when state changes?
- What are State and Props in React?
- Can we use useState without set variable?
---

## 🎯 What are React Hooks?

Hooks are built-in functions introduced in **React 16.8** that allow Functional Components to use React features without writing Class Components.

Before Hooks, features like state and lifecycle methods were only available in Class Components.

With Hooks, Functional Components became the recommended way to build React applications.

---

## Why Were Hooks Introduced?

Before Hooks:

- State was available only in Class Components.
- Lifecycle methods were available only in Class Components.
- Code reuse was difficult.
- Class Components were more complex.

After Hooks:

- Functional Components can use state.
- Functional Components can use lifecycle methods.
- Better code reuse.
- Cleaner and simpler code.

---

## 📊 Common React Hooks

| Hook | Purpose |
|--------|----------|
| `useState()` | Manage component state |
| `useEffect()` | Perform side effects |
| `useRef()` | Access DOM elements or store mutable values |
| `useMemo()` | Memoize expensive calculations |
| `useCallback()` | Memoize functions |
| `useContext()` | Access Context API values |

---

## ⭐ useState()

### What is useState?

`useState()` allows a Functional Component to store and update data.

Whenever the state changes, React re-renders the component.

> **Simple Definition**
>
> **useState is used to store and update component data.**

---

### Syntax

```jsx
const [state, setState] = useState(initialValue);
```

---
### Example

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
```

### Output

```
0

Click Button

1

Click Again

2
```

---

### Common Use Cases

- Counter
- Login Form
- Search Box
- Toggle Button
- Shopping Cart Count

---

## ⭐ useEffect()

### What is useEffect?

`useEffect()` is used to perform **side effects**.

Examples:

- API Calls
- Timers
- Event Listeners
- Updating the document title
- WebSocket connections

---

### Syntax

```jsx
useEffect(() => {

}, []);
```

---

### Example

```jsx
useEffect(() => {

    console.log("Component Mounted");

}, []);
```

Runs only once after the first render.

---

### Run on Every Render

```jsx
useEffect(() => {

    console.log("Runs Every Render");

});
```

---

### Run When Dependency Changes

```jsx
useEffect(() => {

    console.log("Count Changed");

}, [count]);
```

---

### Cleanup Function

```jsx
useEffect(() => {

    const timer = setInterval(() => {

    }, 1000);

    return () => clearInterval(timer);

}, []);
```

---

### Common Use Cases

- Fetch API Data
- Start Timers
- Subscribe to Events
- Cleanup Resources

---

## ⭐ useRef()

### What is useRef?

`useRef()` creates a mutable object whose value persists across renders **without causing a re-render**.

It is mainly used for:

- Accessing DOM elements
- Storing mutable values

---

### Example

```jsx
import { useRef } from "react";

function App() {

    const inputRef = useRef();

    return (
        <>
            <input ref={inputRef} />

            <button
                onClick={() => inputRef.current.focus()}
            >
                Focus
            </button>
        </>
    );
}
```

---

### Common Use Cases

- Focus an input
- Store previous value
- Store timers
- Access DOM elements

---

## ⭐ useMemo()

### What is useMemo?

`useMemo()` caches (memoizes) the result of an expensive calculation so it is only recalculated when its dependencies change.

> **Simple Definition**
>
> **useMemo remembers a calculated value.**

---

### Example

```jsx
const total = useMemo(() => {

    return products.reduce(
        (sum, item) => sum + item.price,
        0
    );

}, [products]);
```

Without `useMemo()`, the calculation runs on every render.

With `useMemo()`, it runs only when `products` changes.

---

### Common Use Cases

- Large Calculations
- Sorting
- Filtering
- Searching
- Expensive Computations

---

## ⭐ useCallback()

### What is useCallback?

`useCallback()` memoizes a function so React doesn't create a new function on every render.

> **Simple Definition**
>
> **useCallback remembers a function.**

---

### Example

```jsx
const handleClick = useCallback(() => {

    console.log("Clicked");

}, []);
```

---

### Why is it Useful?

Without `useCallback()`:

```
Render

↓

New Function Created
```

With `useCallback()`:

```
Render

↓

Same Function Reused
```

This is useful when passing functions to child components wrapped with `React.memo()`.

---

### Common Use Cases

- Event Handlers
- Parent → Child Components
- Performance Optimization

---

## ⭐ useContext()

### What is useContext?

`useContext()` allows a component to access data from the **Context API** without passing props manually.

---

### Example

```jsx
const user = useContext(UserContext);
```

---

### Common Use Cases

- Authentication
- Theme
- Language
- User Information
- Application Settings

---

## 🔄 What Happens Internally When State Changes?

Suppose we have:

```jsx
setCount(count + 1);
```

React performs the following steps:

```text
setState()
      │
      ▼
State Changes
      │
      ▼
Component Re-renders
      │
      ▼
New Virtual DOM Created
      │
      ▼
Compare with Old Virtual DOM
      │
      ▼
Diffing
      │
      ▼
Update Real DOM
```

Only the changed parts of the UI are updated.

---

## 🎯 What is State?

**State** is data that belongs to a component and can change over time.

When state changes, React automatically re-renders the component.

---

### Example

```jsx
const [count, setCount] = useState(0);
```

State belongs to the component itself.

---

## 🎯 What are Props?

**Props (Properties)** are values passed from a **Parent Component** to a **Child Component**.

Props are **read-only** inside the child component.

---

### Example

#### Parent Component

```jsx
<Profile name="Nikhil" />
```

### Child Component

```jsx
function Profile({ name }) {

    return <h2>{name}</h2>;

}
```

Output

```
Nikhil
```

---

## ⚖️ State vs Props

| Feature | State | Props |
|----------|--------|--------|
| Owned By | Current Component | Parent Component |
| Mutable | ✅ Yes | ❌ No |
| Can Change | Yes | Only Parent can change it |
| Causes Re-render | ✅ Yes | ✅ Yes (when prop value changes) |
| Created Using | `useState()` | Passed from Parent |

---

## 🌍 Real-World Examples

| Hook | Real-World Example |
|--------|--------------------|
| `useState()` | Counter, Login Form |
| `useEffect()` | API Calls |
| `useRef()` | Focus Input |
| `useMemo()` | Filter Products |
| `useCallback()` | Optimize Child Components |
| `useContext()` | Theme, Authentication |

---

## 🎯 Interview Scenarios

### Scenario 1

#### Interviewer

> Which Hook is used to call an API?

#### Answer

```jsx
useEffect(() => {

    fetchUsers();

}, []);
```

---

### Scenario 2

#### Interviewer

> Which Hook is used to focus an input field?

#### Answer

```jsx
const inputRef = useRef();

inputRef.current.focus();
```

---

### Scenario 3

#### Interviewer

> Which Hook is used to optimize expensive calculations?

#### Answer

`useMemo()`

---

### Scenario 4

#### Interviewer

> Which Hook prevents unnecessary recreation of functions?

#### Answer

`useCallback()`

---

### Scenario 5

#### Interviewer

> Which Hook replaces Prop Drilling?

#### Answer

`useContext()`

---

## 📝 Memory Trick

```
S → useState

E → useEffect

R → useRef

M → useMemo

C → useCallback

C → useContext
```

Remember:

```
State
↓

Effect
↓

Ref
↓

Memo

↓

Callback

↓

Context
```

---

## 💡 Interview Tip

A strong interview answer is:

> **React Hooks allow Functional Components to use state, lifecycle methods, context, and other React features. `useState` manages component state, `useEffect` handles side effects, `useRef` accesses DOM elements or stores mutable values, `useMemo` caches expensive calculations, `useCallback` caches functions, and `useContext` accesses shared data from the Context API. Together, these Hooks simplify React development and improve code readability and performance.**

---

## ⭐ Quick Revision

| Hook | Purpose |
|--------|----------|
| `useState()` | Store component state |
| `useEffect()` | Side effects |
| `useRef()` | DOM access & mutable values |
| `useMemo()` | Cache expensive calculations |
| `useCallback()` | Cache functions |
| `useContext()` | Access shared context |

---


## ⭐ 7. useEffect Deep Dive

### Interview Questions

- useEffect(() => {}, [])
- useEffect(() => {}, [count])
- useEffect(() => {})
- When does it execute?
- Cleanup function?
- API calls?

---

## ⭐ 8. useMemo vs useCallback

### Interview Questions

- Differences
- Use cases
- Performance optimization

---

## ⭐ 9. useRef

### Interview Questions

- DOM manipulation
- Avoiding re-renders
- Storing mutable values

---

## ⭐ 10. React.memo

### Interview Questions

- What does React.memo do?
- How does it prevent unnecessary re-renders?

---

## ⭐ 11. Redux

### Interview Questions

- Why Redux?
- Redux Flow
- Store
- Action
- Reducer
- Dispatch

---

## ⭐ 12. Redux Toolkit

### Interview Questions

- createSlice
- configureStore
- createAsyncThunk
- Why preferred?

---

## ⭐ 13. Context API vs Redux

### Interview Questions

- Differences
- When to use each?

---

## ⭐ 14. Parent-Child Communication

### Interview Questions

- Parent → Child
- Child → Parent
- Sibling → Sibling

---

## ⭐ 15. Props vs State

### Interview Questions

- Differences
- Use cases

---

## ⭐ 16. Bundle Size Optimization

### Interview Questions

- React.lazy
- Suspense
- Dynamic Imports
- Code Splitting
- Tree Shaking

---

## ⭐ 17. Lazy Loading

### Interview Questions

- React.lazy()
- Benefits
- Real-world usage

---

## ⭐ 18. Code Splitting

### Interview Questions

- Route-level
- Component-level

---

## ⭐ 19. Preventing Re-renders

### Interview Questions

- React.memo
- useMemo
- useCallback

---

## ⭐ 20. API Calls in React

### Interview Questions

- Fetch API
- Axios
- Where to call APIs?
- Error handling

---

## ⭐ 21. Axios vs Fetch

### Interview Questions

- Differences
- Pros and Cons

---

## ⭐ 22. API Cancellation

### Interview Questions

- AbortController
- Cancel pending requests

---

## ⭐ 23. Form Handling

### Interview Questions

- Controlled Forms
- Validation
- Error Handling

---

## ⭐ 24. Formik / React Hook Form

### Interview Questions

- Have you used React Hook Form?
- Benefits

---

## ⭐ 25. React Router

### Interview Questions

- BrowserRouter
- Routes
- Route
- Navigate
- useNavigate
- useParams

---

## ⭐ 26. Protected Routes

### Interview Questions

- Authentication-based routing
- Authorization

---

## ⭐ 27. What Causes Re-render?

### Interview Questions

- setState()
- Parent re-render
- Context change

---

## ⭐ 28. How to Stop Re-rendering?

### Interview Questions

- React.memo
- useMemo
- useCallback

---

## ⭐ 29. Synthetic Events

### Interview Questions

- What are Synthetic Events?
- Difference from native DOM events

---

## ⭐ 30. Event Bubbling and Capturing

### Interview Questions

- Explain Bubbling
- Capturing
- stopPropagation()

---

## ⭐ 31. Memory Leaks in React

### Interview Questions

- Causes
- Detection
- Cleanup in useEffect

---

## ⭐ 33. Higher Order Components (HOC)

### Interview Questions

- What are HOCs?
- Real-world use cases

---

## ⭐ 34. Custom Hooks

### Interview Questions

- What are Custom Hooks?
- Create a reusable useFetch()

---

## ⭐ 35. Error Boundaries

### Interview Questions

- What are Error Boundaries?
- Why are they needed?

---

## ⭐ 36. Portals

### Interview Questions

- ReactDOM.createPortal()
- Modal
- Popup
- Tooltip

---

## ⭐ 37. ForwardRef

### Interview Questions

- forwardRef()
- Use cases
- What causes component re-render?

---
