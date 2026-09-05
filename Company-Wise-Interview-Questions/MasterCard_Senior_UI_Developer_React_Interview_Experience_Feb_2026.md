# Interview Experience – MasterCard
## Senior UI Developer | React | 5–12 Years Experience
### February 2026 – React Interview Questions & Answers

> **Purpose:** This document contains interview-ready answers, practical examples, real-time use cases, and “why/when to use” guidance for a Senior UI Developer interview. The examples use modern React functional components and hooks.

---

## 1. What is a real-time use case of `useCallback`, `useMemo`, and `useReducer` in React?

These three hooks solve different problems:

- **`useCallback`** memoizes a function reference.
- **`useMemo`** memoizes a calculated value.
- **`useReducer`** manages complex state transitions.

### `useCallback` – real-time use case

Suppose a parent component renders a large list of users. We pass a callback to a memoized child component.

```jsx
const UserRow = React.memo(({ user, onSelect }) => {
  console.log("UserRow rendered:", user.name);

  return (
    <button onClick={() => onSelect(user.id)}>
      {user.name}
    </button>
  );
});

function Users({ users }) {
  const [selectedId, setSelectedId] = useState(null);

  const handleSelect = useCallback((id) => {
    setSelectedId(id);
  }, []);

  return users.map(user => (
    <UserRow
      key={user.id}
      user={user}
      onSelect={handleSelect}
    />
  ));
}
```

### Why?

Without `useCallback`, `handleSelect` is recreated on every parent render. `React.memo` may then see a new function reference and re-render the child.

### When to use?

Use it when:

- Passing callbacks to memoized children.
- A callback is a dependency of another hook and its identity matters.
- Function recreation is actually contributing to a performance problem.

**Don't use it everywhere.** Memoization also has a cost and can make code harder to understand.

---

### `useMemo` – real-time use case

Imagine an employee page with thousands of employees and an expensive filtering/sorting operation.

```jsx
const filteredEmployees = useMemo(() => {
  return employees
    .filter(e => e.name.toLowerCase().includes(search.toLowerCase()))
    .sort((a, b) => a.name.localeCompare(b.name));
}, [employees, search]);
```

Now React recalculates the result only when `employees` or `search` changes.

### When to use?

Use `useMemo` for:

- Expensive calculations.
- Derived data used by memoized children.
- Expensive filtering, sorting, grouping, or transformations.

Do not use it just because a calculation exists.

---

### `useReducer` – real-time use case

Consider a complex API-driven form with states such as loading, success, error, reset, and field updates.

```jsx
const initialState = {
  loading: false,
  data: null,
  error: null
};

function reducer(state, action) {
  switch (action.type) {
    case "FETCH_START":
      return { ...state, loading: true, error: null };

    case "FETCH_SUCCESS":
      return { loading: false, data: action.payload, error: null };

    case "FETCH_ERROR":
      return { loading: false, data: null, error: action.payload };

    default:
      return state;
  }
}
```

```jsx
const [state, dispatch] = useReducer(reducer, initialState);

dispatch({ type: "FETCH_START" });
dispatch({ type: "FETCH_SUCCESS", payload: data });
dispatch({ type: "FETCH_ERROR", payload: error });
```

### Why use `useReducer`?

When state has many related values and transitions, a reducer makes state changes predictable and centralized.

### Interview summary

> "`useCallback` is mainly for stable function references, `useMemo` is for memoizing expensive calculated values, and `useReducer` is for managing complex state transitions."

---

# 2. Have you created a custom hook? Give a real-time implementation.

Yes. A custom hook is a reusable function that starts with `use` and can use other React hooks.

A common real-world example is a reusable API hook.

```jsx
import { useEffect, useState } from "react";

function useFetch(url) {
  const [data, setData] = useState(null);
  const [loading, setLoading] = useState(true);
  const [error, setError] = useState(null);

  useEffect(() => {
    const controller = new AbortController();

    async function loadData() {
      try {
        setLoading(true);
        setError(null);

        const response = await fetch(url, {
          signal: controller.signal
        });

        if (!response.ok) {
          throw new Error("API request failed");
        }

        const result = await response.json();
        setData(result);
      } catch (err) {
        if (err.name !== "AbortError") {
          setError(err);
        }
      } finally {
        if (!controller.signal.aborted) {
          setLoading(false);
        }
      }
    }

    loadData();

    return () => controller.abort();
  }, [url]);

  return { data, loading, error };
}
```

Usage:

```jsx
function EmployeeList() {
  const {
    data: employees,
    loading,
    error
  } = useFetch("/api/employees");

  if (loading) return <p>Loading...</p>;
  if (error) return <p>Something went wrong.</p>;

  return employees.map(e => (
    <div key={e.id}>{e.name}</div>
  ));
}
```

### Why create custom hooks?

They allow us to reuse **behavior**, not UI.

For example:

- `useFetch`
- `useDebounce`
- `useLocalStorage`
- `useAuth`
- `usePagination`
- `usePermission`

### Interview point

> "A custom hook helps us extract reusable stateful logic while keeping components focused on presentation."

---

# 3. Why can `useEffect()` sometimes cause an infinite loop?

An effect can cause an infinite loop when:

1. The effect runs.
2. It updates state.
3. State update causes a render.
4. The effect runs again.
5. It updates state again.

Example:

```jsx
function Component() {
  const [count, setCount] = useState(0);

  useEffect(() => {
    setCount(count + 1);
  }, [count]);

  return <div>{count}</div>;
}
```

Here, `count` changes inside the effect and `count` is also a dependency.

### Another common problem: unstable objects/functions

```jsx
const options = {
  page: 1
};

useEffect(() => {
  fetchData(options);
}, [options]);
```

`options` is a new object on every render, so React sees a different reference.

A better approach is:

```jsx
useEffect(() => {
  fetchData({ page: 1 });
}, []);
```

Or memoize a genuinely needed object:

```jsx
const options = useMemo(() => ({
  page
}), [page]);
```

### How to prevent loops?

- Check whether the effect really needs to update state.
- Keep dependencies correct.
- Avoid unnecessary object/function dependencies.
- Use functional state updates where appropriate.
- Separate unrelated effects.

> **Senior-level point:** Do not simply remove dependencies to silence a linter. Understand why the dependency exists and redesign the effect if necessary.

---

# 4. What happens if you omit the dependency array from `useEffect()`?

If we write:

```jsx
useEffect(() => {
  console.log("Effect executed");
});
```

the effect runs **after every completed render**.

For example:

```jsx
useEffect(() => {
  document.title = `Count: ${count}`;
});
```

This is valid because the title should be synchronized after every render.

But this can be dangerous:

```jsx
useEffect(() => {
  fetch("/api/users");
});
```

The API request will happen after every render, potentially causing excessive requests or even a render loop if the effect updates state.

### Compare

```jsx
useEffect(() => {
  // every render
});
```

```jsx
useEffect(() => {
  // once after mount
}, []);
```

```jsx
useEffect(() => {
  // when userId changes
}, [userId]);
```

### Interview answer

> "Without a dependency array, `useEffect` runs after every render. An empty array means the effect does not react to changing dependencies, while a dependency list controls when React should re-run the synchronization."

---

# 5. Write code to fetch data from an API using `fetch` and Axios.

## Using Fetch

```jsx
useEffect(() => {
  async function getEmployees() {
    try {
      const response = await fetch("/api/employees");

      if (!response.ok) {
        throw new Error(`HTTP error: ${response.status}`);
      }

      const data = await response.json();
      setEmployees(data);
    } catch (error) {
      setError(error.message);
    }
  }

  getEmployees();
}, []);
```

## Using Axios

```jsx
import axios from "axios";

useEffect(() => {
  async function getEmployees() {
    try {
      const response = await axios.get("/api/employees");
      setEmployees(response.data);
    } catch (error) {
      setError(error.message);
    }
  }

  getEmployees();
}, []);
```

### Fetch vs Axios

| Feature | Fetch | Axios |
|---|---|---|
| Built into browser | Yes | No |
| Automatic JSON parsing | No | Yes |
| HTTP error rejection by default | No | Yes |
| Interceptors | No built-in equivalent | Yes |
| Request cancellation | AbortController | AbortController support |
| Bundle dependency | None | Additional package |

### Real enterprise use case

In a large application, Axios can be convenient when we need:

- Request/response interceptors.
- Centralized authentication headers.
- Common error handling.
- API client configuration.

For small applications, native `fetch` may be enough.

---

# 6. What is prop drilling and how can you solve it?

Prop drilling means passing data through intermediate components that don't actually need the data.

Example:

```text
App
 ↓
Dashboard
 ↓
EmployeePage
 ↓
EmployeeList
 ↓
EmployeeRow
```

Suppose `user` is needed only by `EmployeeRow`, but every parent has to pass it.

```jsx
<App user={user} />
<Dashboard user={user} />
<EmployeePage user={user} />
<EmployeeList user={user} />
<EmployeeRow user={user} />
```

### Solutions

#### 1. React Context

```jsx
const UserContext = createContext(null);

function App() {
  return (
    <UserContext.Provider value={user}>
      <Dashboard />
    </UserContext.Provider>
  );
}

function EmployeeRow() {
  const user = useContext(UserContext);

  return <div>{user.name}</div>;
}
```

#### 2. State management libraries

For larger cross-cutting application state, we can use Redux Toolkit or another suitable state-management solution.

#### 3. Component composition

Sometimes the best solution is to restructure components rather than introduce global state.

### Interview point

> "I don't automatically use Context or Redux for every prop-drilling problem. First I check whether composition or lifting state differently can solve it. For genuinely shared application state, Context or a state-management library can be appropriate."

---

# 7. Explain Redux architecture and the flow of data.

Redux follows a predictable one-way data flow.

```text
UI
 ↓ dispatch(action)
Action
 ↓
Reducer
 ↓
Store
 ↓
UI re-renders
```

Modern React applications generally use **Redux Toolkit** rather than writing Redux boilerplate manually.

### Example

```jsx
const counterSlice = createSlice({
  name: "counter",
  initialState: { value: 0 },
  reducers: {
    increment: (state) => {
      state.value += 1;
    },
    decrement: (state) => {
      state.value -= 1;
    }
  }
});
```

Component:

```jsx
const dispatch = useDispatch();

<button onClick={() => dispatch(increment())}>
  Increment
</button>
```

Reading state:

```jsx
const count = useSelector(
  state => state.counter.value
);
```

### What happens?

1. User clicks button.
2. Component dispatches an action.
3. Redux sends the action through the reducer logic.
4. Reducer calculates the next state.
5. Store contains the updated state.
6. Components selecting affected state can re-render.

### Where does API logic go?

In Redux Toolkit, asynchronous server interaction can be handled with tools such as `createAsyncThunk`, RTK Query, or a separate API/service layer depending on architecture.

---

# 8. How do you decide whether state should be local, global, or server state?

I classify state based on **who owns it and how long it needs to live**.

### Local state

Examples:

- Modal open/close.
- Input value.
- Selected tab.
- Accordion state.

```jsx
const [isOpen, setIsOpen] = useState(false);
```

Use local state when only one component or a small component subtree needs it.

### Global/client state

Examples:

- Theme.
- User preferences.
- Authentication/session information.
- Cross-page UI state.
- Shared shopping cart state.

Use Context or a state-management library when many unrelated components need the same client-side state.

### Server state

Examples:

- Employee data.
- Product catalog.
- Orders.
- Customer information.

This state comes from the backend and often needs caching, refetching, invalidation, retries, pagination, etc.

A dedicated server-state solution such as TanStack Query can be useful.

### Interview rule

> "I avoid putting everything into Redux. I keep transient UI state local, genuinely shared client state global, and server-owned data in a server-state/data-fetching layer."

---

# 9. How would you identify why a React component is rendering unnecessarily?

I would follow a systematic process.

### Step 1 – React DevTools Profiler

Use the Profiler to identify:

- Which components rendered.
- How often they rendered.
- What triggered the render.
- Which renders are expensive.

### Step 2 – Check props

Look for changing object/function references:

```jsx
<Child
  options={{ page: 1 }}
  onClick={() => handleClick()}
/>
```

Both values are recreated during each parent render.

### Step 3 – Check state

Ask:

- Is state changing unnecessarily?
- Is state stored too high in the component tree?
- Can the state be colocated?

### Step 4 – Check Context

A Context consumer can re-render when the provider value changes.

For example:

```jsx
<UserContext.Provider value={{ user, logout }}>
```

The object is recreated on renders.

### Step 5 – Check parent renders

A child can render because its parent rendered even when its own data did not meaningfully change.

### Tools

- React DevTools Profiler.
- React DevTools component inspection.
- Browser Performance tools.
- `React.memo` where appropriate.
- Careful logging during diagnosis.

### Senior-level answer

> "I measure first rather than blindly adding memoization."

---

# 10. How would you optimize a React application that has performance issues?

I would use a **measure → identify → optimize → measure again** approach.

### 1. Identify the bottleneck

Use:

- React Profiler.
- Browser Performance panel.
- Network panel.
- Bundle analysis.

### 2. Reduce unnecessary renders

Use:

- State colocation.
- `React.memo`.
- `useMemo`.
- `useCallback`.

Only where measurement shows value.

### 3. Optimize large lists

Use:

- Pagination.
- Virtualization.
- Server-side filtering/sorting.

### 4. Optimize API calls

Use:

- Caching.
- Request deduplication.
- Debouncing search.
- Pagination.
- Avoid duplicate requests.

### 5. Optimize bundle size

Use:

- Code splitting.
- Lazy loading.
- Remove unused dependencies.
- Tree-shaking-friendly imports.

### 6. Optimize images/assets

- Proper image formats.
- Responsive images.
- Lazy loading where appropriate.

### 7. Avoid unnecessary work

For example, don't repeatedly sort a huge array during every render if the input hasn't changed.

### Interview answer

> "I would not start by adding `useMemo` everywhere. First I identify whether the bottleneck is rendering, JavaScript computation, network, bundle size, or DOM size, and then optimize the actual bottleneck."

---

# 11. What is code splitting in React?

Code splitting means dividing a large JavaScript bundle into smaller chunks that can be loaded when required.

Without code splitting:

```text
Application
 └── Large JavaScript bundle
```

With code splitting:

```text
Initial bundle
 ├── Dashboard chunk
 ├── Reports chunk
 ├── Admin chunk
 └── Settings chunk
```

For example, an application may have an Admin module that most users rarely access.

Instead of loading Admin code for everyone, load it when the user navigates to that route.

### Benefits

- Smaller initial download.
- Faster initial page load.
- Less JavaScript parsed/executed initially.
- Better scalability for large applications.

---

# 12. What are `React.lazy()` and `Suspense` used for?

`React.lazy()` lets us dynamically load a component.

```jsx
const Reports = React.lazy(
  () => import("./Reports")
);
```

Then wrap it with `Suspense`:

```jsx
<Suspense fallback={<div>Loading reports...</div>}>
  <Reports />
</Suspense>
```

### What happens?

Initially, the Reports component code does not have to be included in the initial JavaScript execution path. React loads the module when it is needed.

### Real-time use case

An enterprise application may have:

- Dashboard.
- Reports.
- Administration.
- Audit.
- Settings.

Reports and Administration may be used rarely, so these routes can be lazy-loaded.

### Important distinction

> `React.lazy()` is about dynamically loading a component module. `Suspense` provides a fallback while React is waiting for something that suspends.

For route-level code splitting, routing libraries commonly integrate with lazy loading.

---

# 13. How would you optimize a React page containing 10,000+ records?

I would avoid rendering all 10,000 rows at once.

### Approach 1 – Server-side pagination

Instead of:

```text
GET /employees
→ 10,000 records
```

Use:

```text
GET /employees?page=1&pageSize=50
```

The server returns only the required records.

### Approach 2 – Server-side filtering/sorting

Instead of downloading everything:

```text
GET /employees?search=nikhil&sort=name
```

### Approach 3 – Virtualization

Only render rows visible in the viewport.

### Approach 4 – Memoization

If rows are complex:

```jsx
const EmployeeRow = React.memo(EmployeeRowComponent);
```

### Approach 5 – Debounce search

Don't call the API for every keystroke.

```text
N
Ni
Nik
Nikh
Nikhil
```

Instead, wait a short period after the user stops typing.

### Best enterprise solution

For 10,000+ records, I would typically combine:

> **Server-side pagination/filtering/sorting + virtualization when the UX requires a large scrolling dataset + optimized row rendering.**

---

# 14. What is list virtualization and when would you use it?

List virtualization means rendering only the items currently visible in the viewport rather than rendering the entire list.

Suppose we have:

```text
50,000 records
```

Instead of creating DOM nodes for all 50,000 rows, the UI may render approximately the visible rows plus a small buffer.

Conceptually:

```text
50,000 data items
       ↓
Virtualization
       ↓
~20–100 DOM rows
```

As the user scrolls, rows are reused/repositioned.

### Popular solutions

Examples include:

- `react-window`
- `react-virtualized`
- TanStack Virtual

### When should I use it?

Use virtualization when:

- The list is very large.
- DOM rendering is the bottleneck.
- Users need a continuous scrolling experience.

### When not to use it?

For a list of 50 items, virtualization may add unnecessary complexity.

### Interview point

> "Virtualization reduces DOM work; it does not reduce the amount of data stored in memory. For huge datasets, I may combine virtualization with server-side pagination."

---

# 15. How would you handle loading, success, empty, and error states for an API call?

I usually model the API lifecycle explicitly.

```jsx
const [status, setStatus] = useState("idle");
const [data, setData] = useState([]);
const [error, setError] = useState(null);
```

The states could be:

```text
idle
loading
success
empty
error
```

Example:

```jsx
if (status === "loading") {
  return <Loader />;
}

if (status === "error") {
  return <ErrorMessage />;
}

if (status === "success" && data.length === 0) {
  return <EmptyState />;
}

return <EmployeeList employees={data} />;
```

### Better user experience

For production applications, I would consider:

- Skeleton loaders.
- Retry action.
- Friendly error message.
- Empty-state message.
- Previous data during refetch where appropriate.
- Request cancellation.

### Important distinction

An **empty state is not an error**.

For example:

```text
API succeeded → 200 OK → []
```

means there are currently no records.

---

# 16. How would you cancel an API request when a component unmounts?

With `fetch`, use `AbortController`.

```jsx
useEffect(() => {
  const controller = new AbortController();

  async function loadData() {
    try {
      const response = await fetch("/api/employees", {
        signal: controller.signal
      });

      const data = await response.json();
      setEmployees(data);
    } catch (error) {
      if (error.name !== "AbortError") {
        setError(error);
      }
    }
  }

  loadData();

  return () => {
    controller.abort();
  };
}, []);
```

### Why?

Imagine the user navigates away before the request finishes.

Without cancellation:

```text
Component
   ↓
API request ───────────────→ response
   ↓
Component unmounted
```

Cancellation allows us to stop an obsolete request.

### Axios

Modern Axios also supports `AbortController`:

```jsx
const controller = new AbortController();

axios.get("/api/employees", {
  signal: controller.signal
});

controller.abort();
```

---

# 17. How do you prevent race conditions when multiple API requests are triggered?

A common example is search.

The user enters:

```text
react
```

Requests may be:

```text
Request A → "r"
Request B → "re"
Request C → "rea"
Request D → "react"
```

The problem is that an older request can finish after a newer request.

```text
Request D finishes
Request A finishes later
```

If we blindly update state, old data can overwrite new data.

### Solution 1 – Abort previous request

```jsx
useEffect(() => {
  const controller = new AbortController();

  async function searchEmployees() {
    try {
      const response = await fetch(
        `/api/employees?search=${encodeURIComponent(search)}`,
        { signal: controller.signal }
      );

      const data = await response.json();
      setEmployees(data);
    } catch (error) {
      if (error.name !== "AbortError") {
        setError(error);
      }
    }
  }

  searchEmployees();

  return () => controller.abort();
}, [search]);
```

### Solution 2 – Request ID / sequence number

Track the latest request and only accept the latest response.

### Solution 3 – Use a server-state library

Libraries such as TanStack Query provide caching and request lifecycle management that can simplify many server-state scenarios.

### Also use debouncing

For search boxes, debounce input so we don't issue a request for every character.

> **Senior-level answer:** "I prevent stale responses from updating the UI by cancelling obsolete requests or ignoring responses that are no longer current. For search, I usually combine this with debouncing."

---

# 18. Where should API/business logic be placed in a large React application?

I avoid putting large API/business logic directly inside UI components.

A scalable structure might be:

```text
src/
├── components/
├── pages/
├── features/
│   ├── employees/
│   │   ├── EmployeeList.jsx
│   │   ├── employeeApi.js
│   │   ├── employeeHooks.js
│   │   └── employeeSlice.js
├── services/
│   ├── apiClient.js
│   └── authService.js
├── store/
├── hooks/
├── utils/
└── routes/
```

### API layer

```jsx
export async function getEmployees() {
  const response = await apiClient.get("/employees");
  return response.data;
}
```

### UI layer

```jsx
const { data, loading } = useEmployees();
```

### Business logic

Business rules can live in:

- Feature/domain services.
- Custom hooks for UI-facing behavior.
- Selectors.
- Utility/domain functions.
- State-management logic.

### Why?

It improves:

- Testability.
- Reusability.
- Maintainability.
- Separation of concerns.
- Team scalability.

### Important distinction

A service should not become a dumping ground for every piece of logic. Business/domain rules should be organized around features and responsibilities.

---

# 19. How would you implement authentication and protected routes in React?

I separate **authentication** from **authorization**.

### Authentication

Authentication answers:

> "Who is the user?"

Typically:

```text
Login
 ↓
Identity Provider / Backend
 ↓
Session or token
 ↓
Authenticated application
```

For an enterprise application, I would prefer an established identity solution such as OAuth 2.0 / OpenID Connect rather than building authentication from scratch.

### Protected route

Conceptually:

```jsx
function ProtectedRoute({ children }) {
  const { isAuthenticated, isLoading } = useAuth();

  if (isLoading) {
    return <Loader />;
  }

  if (!isAuthenticated) {
    return <Navigate to="/login" replace />;
  }

  return children;
}
```

Usage:

```jsx
<Route
  path="/dashboard"
  element={
    <ProtectedRoute>
      <Dashboard />
    </ProtectedRoute>
  }
/>
```

### Authorization

Authentication:

```text
Is the user logged in?
```

Authorization:

```text
Can this logged-in user access Admin?
```

Example:

```jsx
if (!user.roles.includes("Admin")) {
  return <Navigate to="/forbidden" />;
}
```

### Security point

React route protection is primarily a **UI/UX control**. The backend must independently enforce authorization on protected APIs.

Never rely on hiding a React route as the only security mechanism.

---

# 20. What causes a React component to re-render?

A component can re-render when:

### 1. Its state changes

```jsx
setCount(count + 1);
```

### 2. Its parent re-renders

A parent render can cause child components to render unless appropriate optimization/bailout mechanisms apply.

### 3. Its props change

```jsx
<User name={name} />
```

If `name` changes, the child may re-render.

### 4. A consumed Context value changes

```jsx
const theme = useContext(ThemeContext);
```

When the relevant context value changes, consumers can re-render.

### 5. An external store subscription changes

For example, state-management libraries can notify subscribed components when selected state changes.

### Important clarification

A re-render does **not** necessarily mean the DOM is fully recreated.

React renders the component, compares the result with the previous render, and commits the necessary DOM updates.

### Example

```jsx
function Counter() {
  const [count, setCount] = useState(0);

  console.log("render");

  return (
    <button onClick={() => setCount(c => c + 1)}>
      {count}
    </button>
  );
}
```

Calling `setCount` schedules another render because the component's state changed.

### Interview summary

> "The main causes are state changes, parent renders, changed props, context updates, and updates from subscribed external stores. I then use React DevTools Profiler to determine whether the render is actually expensive or unnecessary."

---

# Quick Interview Revision – One-Line Answers

| # | Question | Interview-ready answer |
|---|---|---|
| 1 | `useCallback`, `useMemo`, `useReducer` | `useCallback` memoizes function references, `useMemo` memoizes calculated values, and `useReducer` manages complex state transitions. |
| 2 | Custom Hook | A custom hook extracts reusable stateful behavior such as fetching, authentication, debouncing, or pagination. |
| 3 | `useEffect` infinite loop | Usually caused when an effect updates state that is also one of its dependencies, or unstable dependencies change on every render. |
| 4 | No dependency array | The effect runs after every completed render. |
| 5 | Fetch/Axios | Both can call APIs; Axios adds conveniences such as interceptors and automatic JSON handling. |
| 6 | Prop drilling | Passing props through components that don't need them; solve with composition, Context, or appropriate state management. |
| 7 | Redux | UI dispatches an action, state logic calculates the next state, the store updates, and subscribed UI reads the new state. |
| 8 | State classification | Local for component UI state, global for shared client state, server-state tools for backend-owned data. |
| 9 | Unnecessary renders | Use React DevTools Profiler, inspect props/state/context, and identify unstable references or overly broad state. |
| 10 | Performance | Measure first, then optimize rendering, API calls, bundle size, DOM size, and expensive computations. |
| 11 | Code splitting | Splitting JavaScript into smaller chunks loaded when needed. |
| 12 | `lazy`/`Suspense` | `lazy` dynamically imports a component; `Suspense` provides a fallback while suspended content loads. |
| 13 | 10,000+ records | Prefer server-side pagination/filtering/sorting and add virtualization when a large scrolling UI is required. |
| 14 | Virtualization | Render only visible list items instead of the entire dataset. |
| 15 | API states | Explicitly handle loading, success, empty, and error states. |
| 16 | Cancel API | Use `AbortController` and abort in the effect cleanup. |
| 17 | Race conditions | Cancel obsolete requests or ignore stale responses; debounce search inputs. |
| 18 | API/business logic | Keep UI, API access, and business/domain logic separated and organized by feature. |
| 19 | Authentication | Establish a trusted identity/session, protect routes in the UI, enforce authorization on the backend. |
| 20 | Re-render | State, props, parent renders, context, and external-store updates can trigger rendering. |

---

# Senior-Level Closing Points

For a **5–12 year Senior UI Developer** interview, avoid answering React questions only with definitions. A stronger answer follows this pattern:

```text
Definition
   ↓
Real-time use case
   ↓
Small code example
   ↓
Why?
   ↓
When to use?
   ↓
When NOT to use?
   ↓
Performance / scalability / security consideration
```

For example, instead of saying:

> "useMemo is used for optimization."

Say:

> "`useMemo` caches the result of a calculation between renders. I would use it when an expensive calculation such as filtering or sorting a large dataset is repeated unnecessarily. I would first measure the performance issue, because using `useMemo` everywhere adds complexity and isn't automatically beneficial."

That style demonstrates **practical experience rather than memorized definitions**, which is particularly important for senior-level React interviews.

---

## Suggested Video Structure

For recording the interview-experience video, you can explain each question in approximately:

- **30–45 sec:** Definition
- **60–90 sec:** Real-world scenario
- **60–90 sec:** Code/example
- **30–45 sec:** Why/when/not to use
- **15–30 sec:** Interview takeaway

For 20 questions, this gives approximately **45–60 minutes of strong interview-focused content**.
