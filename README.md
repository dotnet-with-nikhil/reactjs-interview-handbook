# React Interview Experiences

A curated collection of **real React interview experiences**, questions, coding challenges, and answers asked by various companies.

⭐ If you find this repository helpful, please consider giving it a star.

---

## Companies

| Company | Interview Experience |
|----------|----------|
| PwC | [View Questions](#-pwc-react-interview-experience)

---

# 📌 PwC React Interview Experience

**Role:** React Developer

**Experience Level:** 3-5+ Years

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

### 3. What are the real-world use cases of useMemo and useCallback?

#### useMemo

**Definition:**

`useMemo` is a React Hook that remembers (memoizes) the result of a calculation and only recalculates it when its dependencies change.

**Simple Example:**

```jsx
const squaredNumber = useMemo(() => {
  return number * number;
}, [number]);
```

If `number` does not change, React reuses the previous value instead of recalculating it.

**Real Use Case:**

- Filtering large lists
- Sorting data
- Expensive calculations

---

#### useCallback

**Definition:**

`useCallback` is a React Hook that remembers (memoizes) a function and prevents it from being recreated on every render.

**Simple Example:**

```jsx
const handleClick = useCallback(() => {
  console.log("Button Clicked");
}, []);
```

React will reuse the same function reference on every render.

**Real Use Case:**

- Passing functions to child components
- Working with `React.memo`
- Avoiding unnecessary re-renders

---

#### Difference

| useMemo | useCallback |
|----------|------------|
| Caches a value | Caches a function |
| Returns a value | Returns a function |

**Easy way to remember:**

```text
useMemo → Remember a VALUE
useCallback → Remember a FUNCTION
```

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
var b = '20';

console.log(a + b);
console.log(a - b);
console.log(a * b);
```

#### Output

```text
1020
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

## License

MIT License
