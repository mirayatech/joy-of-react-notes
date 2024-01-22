## 🎛️ React Event Handlers

React makes it easy to add event handlers to JSX elements:

- **Automatic Cleanup**: React automatically removes event handlers when components are not in use, preventing memory issues.
- **Better Performance**: React optimizes these handlers for faster performance.
- **Simplified Code**: You can add event handlers directly in JSX, avoiding complex DOM code.

```jsx
function App() {
  function handleClick() {
    alert("Hello World");
  }

  return <button onClick={handleClick}>Click Me</button>;
}
```

### Passing a function reference

When passing in we do it witout the `()` because we want to pass in the function reference. If we add the `()` we will immediately invoke the function.

```jsx
// ✅ We want to do this:
<button onClick={doSomething} />

// 🚫 Not this:
<button onClick={doSomething()} />
```

### With arguments

If we want to pass in arguments to the function, we can use an arrow function. Because if we just pass in the function reference, it will be invoked immediately.

```jsx
// 🚫 Invalid: calls the function right away
<button onClick={setTheme('dark')}>
  Toggle theme
</button>

// ✅ Valid:
<button onClick={() => setTheme('dark')}>
  Toggle theme
</button>
```

### ⚡ Is It Slow?

You might've heard that using functions like this could slow things down.

Let's break it down:

- Whether you use anonymous or named functions, arrow or traditional ones, it doesn't really impact performance much.

- Creating new functions is super quick, even on slower devices.

- React is clever! It optimizes things for us, especially with events.

Some people worry about using different types of functions in React, but there's a tool called `useCallback` that helps. With `useCallback``, it doesn't matter what kind of function you use, React makes sure they all work well.

```javascript
// Anonymous Function
const handleClick = () => {
  // Do something cool!
};

// Named Function
function handleEvent() {
  // Do something awesome!
}
```

## 🪝 useState Hook

The hook `useState` is a function that returns an array with two elements:

- The first element is the current state value.
- The second element is a function that lets us update the state value.

```jsx
const [count, setCount] = useState(0);
```

## 💃🏽 Why the Dance?

Why can't reactjust do `let count = 0` and `count = count + 1`? Why do we need to use `useState`?

The reason is that React needs to keep track of the state value. If we just use `let count = 0` and `count = count + 1`, React wouldn't know that the state value has changed. So we need to use `useState` to let React know that the state value has changed.

## 📝 Gotchas

```jsx
// 🚫 Incorrect:
const [email, setEmail] = React.useState();

// ✅ Correct:
const [email, setEmail] = React.useState("");

// 🚫 Incorrect:
const [value, setValue] = React.useState();

// ✅ Correct:
const [value, setValue] = React.useState(5);
```
