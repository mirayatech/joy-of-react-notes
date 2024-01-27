# 🏠 Component API Design

## 🎨 Design system

A design system is a set of rules that create a brand's unique style, including color schemes and typography. It outlines designs for UI elements and is usually created by designers using tools like Figma or Sketch. Think of the design system as a recipe book, with the component library as the prepped ingredients, and the web application as the final dish. One issue is that third-party component libraries often come with their own pre-defined design system.

## 🔄 Prop Delegation

Prop delegation lets us pass props directly from a parent component to a specific child component, like `Button`, without passing them through all intermediate components. This method is efficient for directly transferring props in a component tree.

```jsx
// Button.js
import React from "react";

const Button = ({ children, ...props }) => (
  <button {...props}>{children}</button>
);

export default Button;
```

## 🔗 Forward Refs

Forwarding refs is a technique that allows us to pass a ref from a parent component to a child component. It's a way of passing a ref down the component tree without having to manually pass it through every component in between. This is useful when we want to pass a ref from a parent component to a child component, but we don't want to pass it through every component in between.

Note: Key and Refs can't be passed as props, they are special props. That's why we use forwardRef.

```jsx
// App.js
import React from "react";
import Button from "./Button";

const App = () => {
  const ref = React.useRef();

  return (
    <div>
      <Button ref={ref}>Click me</Button>
    </div>
  );
};

// Button.js

import React from "react";

const Button = React.forwardRef(({ children, ...props }, ref) => (
  <button ref={ref} {...props}>
    {children}
  </button>
));

export default Button;
```

### 💡 Memo and forwardRef?

Each of these functions is known as a higher-order component — it takes a component as input, and produces a new component as output. And so we can "nest" these functions, and everything works the way you'd expect.

```jsx
// Button.js

import React from "react";

const Button = React.memo(
  React.forwardRef(({ children, ...props }, ref) => (
    <button ref={ref} {...props}>
      {children}
    </button>
  ))
);

export default Button;
```

## 🌀 Polymorphism

Polymorphism is a technique that allows us to use the same component in different ways. It's a way of making a component more flexible, so that it can be used in different contexts. This is useful when we want to use the same component in different ways, but we don't want to create multiple components for each use case.

NOTE: Use the right HTML tags for web interfaces. Use a button tag for clickable actions in JavaScript. If clicking changes the URL, use an anchor tag (`<a>`). Remember, the function of a tag is more important than its look. You can style a `<button>` with CSS to change its appearance. This approach is simpler than adding features to other tags.

BEFORE:

```jsx
import React from "react";
import styles from "./LinkButton.module.css";

function LinkButton({ href, children, ...delegated }) {
  if (typeof href === "string") {
    return (
      <a href={href} className={styles.button} {...delegated}>
        {children}
      </a>
    );
  }

  return (
    <button className={styles.button} {...delegated}>
      {children}
    </button>
  );
}

export default LinkButton;
```

AFTER:

```jsx
import React from 'react';
import styles from './LinkButton.module.css';

function LinkButton({
  href,
  children,
  ...delegated
}) {
  const Tag = typeof href === 'string'
    ? 'a'
    : 'button';

  return (
    <Tag
      href={href}
      className={styles.button}
      {...delegated}
    >
      {children}
    </Tag>
  );
}

export default LinkButton;

// app.js
import React from 'react';
import LinkButton from './LinkButton'

function App() {
  function exportData() {
    // Imagine there was logic here, maybe
    // a network request to generate the data.
    console.log('exportData function invoked');
  }

  return (
    <main>
      <LinkButton href="/add-transaction">
        Add Transaction
      </LinkButton>
      <LinkButton href="/report">
        View Report
      </LinkButton>


      <LinkButton onClick={exportData}>
        Export All Data
      </LinkButton>
    </main>
  );
}

export default App;
```

## 🧩 Compound Components

Compound components are a technique that allows us to group components together. It's a way of making a component more flexible, so that it can be used in different contexts. This is useful when we want to group components together, but we don't want to create multiple components for each use case.

```jsx

// Tabs.js
import React from 'react';
import styles from './Tabs.module.css';

function Tabs({ children }) {
  const [activeIndex, setActiveIndex] = React.useState(0);

  const childrenArray = React.Children.toArray(children);
  the activeChild = childrenArray[activeIndex];

  return (
    <div className={styles.tabs}>
      <div className={styles.buttons}>
        {childrenArray.map((child, index) => (
          <button
            key={index}
            className={styles.button}
            onClick={() => setActiveIndex(index)}
          >
            {child.props.label}
          </button>
        ))}
      </div>
      <div className={styles.content}>
        {activeChild}
      </div>
    </div>
  );
}

export default Tabs;

// Tab.js

import React from 'react';

function Tab({ children }) {
  return (
    <div>
      {children}
    </div>
  );
}

export default Tab;


// App.js\
import React from 'react';
import Tabs from './Tabs';

function App() {
  return (
    <main>
      <Tabs>
        <Tabs.Tab label="First Tab">
          <p>This is the first tab.</p>
        </Tabs.Tab>
        <Tabs.Tab label="Second Tab">
          <p>This is the second tab.</p>
        </Tabs.Tab>
        <Tabs.Tab label="Third Tab">
          <p>This is the third tab.</p>
        </Tabs.Tab>
      </Tabs>
    </main>
  );
}

```

## 🚀 Performance

pure components (created with React.memo) only re-render when their props, state, or context values change. If a pure component uses context, like a favorite color context, it will re-render whenever the context value changes. Context acts like internal props.
