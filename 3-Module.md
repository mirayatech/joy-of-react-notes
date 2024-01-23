# 🎣 React Hooks

### 🩴 useId hook

**useId** is a custom hook that generates a unique id for a component.

## ⚠️ Strict Mode

Effect hooks run twice in strict mode, but this is not a bug. Its just a way to highlight potential problems. That's why when we `console.log` in a useEffect hook, we see the log twice.

```jsx
import React from "react";

root.render(
  <React.StrictMode>
    <App />
  </React.StrictMode>
);
```

## 🪄 useEffect

**useEffect** is a hook that lets you perform side effects in function components. It is a close replacement for `componentDidMount`, `componentDidUpdate`, and `componentWillUnmount`.

```jsx
import React from "react";

import Toggle from "./Toggle";

export default function App() {
  const [isDarkMode, setIsDarkMode] = React.useState(() => {
    const storedValue = window.localStorage.getItem("is-dark-mode");
    return JSON.parse(storedValue) || false;
  });

  React.useEffect(() => {
    window.localStorage.setItem("is-dark-mode", isDarkMode);
  }, [isDarkMode]);

  return (
    <div
      className="wrapper"
      style={{
        "--color-bg": isDarkMode ? "black" : "white",
        "--color-text": isDarkMode ? "white" : "black",
      }}
    >
      <Toggle
        label="Dark Mode"
        checked={isDarkMode}
        handleToggle={setIsDarkMode}
      />
    </div>
  );
}
```

## 🧼 Cleaning up

We clean up effects by returning a function from the effect hook. We need to do this, otherweise we consume more memories and resources, which can lead to memory leaks. Also the garbage collector will not be able to clean up the memory.

```jsx
// 🛑 Before
React.useEffect(() => {
  window.addEventListener("mousemove", handleMouseMove);
  return () => {
    window.removeEventListener("mousemove", handleMouseMove);
  };
}, []);

// ✅ After
React.useEffect(() => {
  const handleMouseMove = (event) => {
    console.log(event);
  };
  window.addEventListener("mousemove", handleMouseMove);
  return () => {
    window.removeEventListener("mousemove", handleMouseMove);
  };
}, []);
```

## 📦 Memoization

Youve probably heard of memoization before. It is a technique that lets you cache the results of a function. This is useful when you have a function that takes a long time to run, and you want to avoid running it again and again.

Three tools that can help us with memoization are:

- The `useMemo` hook
- The `useCallback` hook
- The `React.memo` component wrapper

## Why React Re-renders?

React re-renders when:

- The props of a component change
- The state of a component changes
- The parent of a component re-renders
- The context value of a component changes

BUt its not true that React re-renders when the state of a component changes. React re-renders when the state of a component changes and the component is subscribed to that state. So the App component will not re-render when the state of the Toggle component changes.

### Pure Components

Pure components are components that only re-render when their props change. They do not re-render when their state changes. They do not re-render when their parent re-renders. They do not re-render when the context value changes.

```jsx
import React from "react";

export default function Pure() {
  const [count, setCount] = React.useState(0);

  const increment = () => {
    setCount((c) => c + 1);
  };

  return (
    <div>
      <button onClick={increment}>Count: {count}</button>
    </div>
  );
}

// OR

function Decoration() {
  return (
    <div className="decoration">
      ⛵️
    </div>
  );
}

const PureDecoration = React.memo(Decoration);

export default PureDecoration;

// OR

function Decoration() {
  return (
    <div className="decoration">
      ⛵️
    </div>
  );
}
```

## 📦 useMemo

**useMemo** is a hook that lets you memoize the result of a function. It takes a function and an array of dependencies. It returns the result of the function.

```jsx
import React from "react";

export default function App() {
  const [count, setCount] = React.useState(0);
  const [dark, setDark] = React.useState(false);

  const double = React.useMemo(() => {
    return slowFunction(count);
  }, [count]);

  const themeStyles = React.useMemo(() => {
    return {
      backgroundColor: dark ? "black" : "white",
      color: dark ? "white" : "black",
    };
  }, [dark]);

  return (
    <div>
      <input
        type="number"
        value={count}
        onChange={(e) => setCount(parseInt(e.target.value))}
      />
      <button onClick={() => setDark((prevDark) => !prevDark)}>
        Change Theme
      </button>
      <div style={themeStyles}>{double}</div>
    </div>
  );
}
```

## 📦 useCallback

**useCallback** is a hook that lets you memoize a function. It takes a function and an array of dependencies. It returns the function.

The difference between `useMemo` and `useCallback` is that `useMemo` returns the result of a function, while `useCallback` returns the function itself.

```jsx
import React from "react";

export default function App() {
  const [count, setCount] = React.useState(0);
  const [dark, setDark] = React.useState(false);

  const double = React.useCallback(() => {
    return slowFunction(count);
  }, [count]);

  const themeStyles = React.useMemo(() => {
    return {
      backgroundColor: dark ? "black" : "white",
      color: dark ? "white" : "black",
    };
  }, [dark]);

  return (
    <div>
      <input
        type="number"
        value={count}
        onChange={(e) => setCount(parseInt(e.target.value))}
      />
      <button onClick={() => setDark((prevDark) => !prevDark)}>
        Change Theme
      </button>
      <div style={themeStyles}>{double}</div>
    </div>
  );
}
```
