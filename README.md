# reactjs-interview-handbook

# React Interview Experiences

A curated collection of **real React interview experiences**, questions, coding challenges, and answers asked by various companies.

⭐ If you find this repository helpful, please consider giving it a star.

---

## Companies

| Company | Interview Experience |
|----------|----------|
| PwC | [View Questions](#-pwc-react-interview-experience) |
| Deloitte | [View Questions](#-deloitte-react-interview-experience) |
| Infosys | [View Questions](#-infosys-react-interview-experience) |

---

# 📌 PwC React Interview Experience

**Role:** React Developer

**Experience Level:** 2–5 Years

**Interview Round:** Technical Round

## Questions Asked

### 1. What are React Hooks? Explain them in detail.

#### Answer

React Hooks are special functions introduced in React 16.8 that allow developers to use state and lifecycle features inside functional components without writing class components.

Common React Hooks:

- useState
- useEffect
- useContext
- useMemo
- useCallback
- useRef
- useReducer

Example:

```jsx
import { useState } from "react";

function Counter() {
  const [count, setCount] = useState(0);

  return (
    <button onClick={() => setCount(count + 1)}>
      {count}
    </button>
  );
}
```

---

### 2. How does useState work with Empty and Filled Arrays?

#### Answer

Empty Array:

```jsx
const [users, setUsers] = useState([]);
```

Initial value:

```js
[]
```

Filled Array:

```jsx
const [users, setUsers] = useState([
  {
    id: 1,
    name: "John"
  }
]);
```

Initial value:

```js
[
  {
    id: 1,
    name: "John"
  }
]
```

React re-renders the component whenever a new array reference is passed to `setUsers()`.

---

### 3. What are real-world use cases of useMemo and useCallback?

#### useMemo

Used to memoize expensive calculations.

```jsx
const filteredUsers = useMemo(() => {
  return users.filter(user => user.isActive);
}, [users]);
```

Common Use Cases:

- Large table filtering
- Dashboard calculations
- Data transformations
- Performance optimization

#### useCallback

Used to memoize functions.

```jsx
const handleClick = useCallback(() => {
  console.log("Button Clicked");
}, []);
```

Common Use Cases:

- Passing callbacks to child components
- Preventing unnecessary re-renders
- Working with React.memo

#### Difference

| useMemo | useCallback |
|----------|----------|
| Memoizes value | Memoizes function |
| Returns value | Returns function |

---

### 4. Create a Counter Component with Increment and Decrement Buttons

#### Solution

```jsx
import { useState } from "react";

function Counter() {
  const [count, setCount] = useState(0);

  return (
    <div>
      <h2>Count: {count}</h2>

      <button
        onClick={() => setCount(count + 1)}
      >
        Increment
      </button>

      <button
        onClick={() => setCount(count - 1)}
      >
        Decrement
      </button>
    </div>
  );
}

export default Counter;
```

---

### 5. What is the output of the following JavaScript code?

#### Code

```javascript
var a = 10;
var b = 20;

console.log(a + b);
console.log(a - b);
console.log(a * b);
```

#### Output

```text
30
-10
200
```

#### Explanation

```javascript
10 + 20 = 30
10 - 20 = -10
10 * 20 = 200
```

---

## Interview Feedback

### Difficulty Level

⭐⭐⭐☆☆ (Moderate)

### Topics Covered

- React Hooks
- JavaScript Fundamentals
- State Management
- Performance Optimization
- Coding Round

---

# 📌 Deloitte React Interview Experience

**Role:** Frontend Developer

**Experience Level:** 3+ Years

## Questions Asked

### 1. What is the difference between useMemo and useCallback?

#### Answer

| useMemo | useCallback |
|----------|----------|
| Memoizes values | Memoizes functions |
| Returns computed value | Returns callback function |

Example:

```jsx
const total = useMemo(() => {
  return calculateTotal(items);
}, [items]);

const handleClick = useCallback(() => {
  console.log("Clicked");
}, []);
```

---

### 2. What is Virtual DOM?

#### Answer

Virtual DOM is a lightweight JavaScript representation of the real DOM.

React updates the Virtual DOM first and then efficiently updates only the changed parts of the real DOM.

---

### 3. Explain React Reconciliation.

#### Answer

Reconciliation is React's algorithm that compares the previous Virtual DOM with the new Virtual DOM and determines the minimum number of DOM updates required.

---

### 4. Explain Context API.

#### Answer

Context API allows sharing data across components without passing props manually through every level.

Example:

```jsx
import { createContext } from "react";

const UserContext = createContext();
```

---

### 5. Controlled vs Uncontrolled Components?

#### Controlled Component

```jsx
<input
  value={name}
  onChange={(e) => setName(e.target.value)}
/>
```

#### Uncontrolled Component

```jsx
<input ref={inputRef} />
```

---

# 📌 Infosys React Interview Experience

**Role:** React Developer

**Experience Level:** 1–4 Years

## Questions Asked

### 1. What is JSX?

#### Answer

JSX stands for JavaScript XML.

It allows developers to write HTML-like syntax inside JavaScript.

Example:

```jsx
const heading = <h1>Hello React</h1>;
```

---

### 2. Difference Between Props and State?

| Props | State |
|----------|----------|
| Read-only | Mutable |
| Passed from parent | Managed inside component |
| External data | Internal data |

---

### 3. Explain useEffect.

#### Answer

useEffect is used to perform side effects in React.

Common Use Cases:

- API calls
- Timers
- Event listeners
- Subscriptions

Example:

```jsx
useEffect(() => {
  fetchUsers();
}, []);
```

---

### 4. What is React.memo?

#### Answer

React.memo is a Higher Order Component that prevents unnecessary re-renders when props remain unchanged.

Example:

```jsx
export default React.memo(UserCard);
```

---

### 5. Difference Between == and === ?

#### Answer

```javascript
1 == "1"; // true
1 === "1"; // false
```

`==`

- Compares values only
- Performs type coercion

`===`

- Compares value and type
- No type coercion

---

## Contributing

Have you recently attended a React interview?

Feel free to contribute your interview experience by creating a Pull Request.

### Suggested Format

- Company Name
- Role
- Experience Level
- Questions Asked
- Answers
- Difficulty Level

---

## License

MIT License
