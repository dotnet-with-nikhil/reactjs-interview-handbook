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

# 🤔 Why was Context API Introduced?

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

# ❌ What is Prop Drilling?

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

# ✅ How Does Context API Avoid Prop Drilling?

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

# 💻 Step 1: Create Context

```jsx
import { createContext } from "react";

const UserContext = createContext();

export default UserContext;
```

---

# 💻 Step 2: Provide Context

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

# 💻 Step 3: Consume Context

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

# 🔄 How Context API Works

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

# 🌍 Real-World Examples

Context API is commonly used for:

- 🌙 Dark/Light Theme
- 👤 Logged-in User
- 🌐 Language Selection
- 🔐 Authentication
- 🛒 Shopping Cart Count
- ⚙️ Application Settings

---



## ⭐ 3. React Lifecycle Methods

### Interview Questions

- Explain Mounting, Updating and Unmounting.
- componentDidMount
- componentDidUpdate
- componentWillUnmount
- Hooks equivalent using useEffect.

---

## ⭐ 4. Virtual DOM

### Interview Questions

- What is Virtual DOM?
- How React updates the UI efficiently?
- DOM vs Virtual DOM.

---

## ⭐ 5. Reconciliation

### Interview Questions

- What is Reconciliation?
- How does React identify changes?
- Explain the Diffing Algorithm.

---

## ⭐ 6. React Hooks

### Interview Questions

- useState
- useEffect
- useRef
- useMemo
- useCallback
- useContext
- What happens internally when state changes?

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
