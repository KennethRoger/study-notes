# DOM Manipulation and Performance Issues

DOM (Document Object Model): A tree structure where each element, attribute, and text in HTML is represented as an object.

Browser Rendering Pipeline: When the DOM is manipulated, it triggers a sequence of steps:

1. **DOM Tree Construction**: HTML is parsed to build the DOM tree.
2. **CSSOM Tree Construction**: CSS is parsed to build the CSSOM.
3. **Render Tree Construction**: The DOM and CSSOM are combined to create the render tree (only elements visible on the page).
4. **Layout (Reflow)**: The browser calculates sizes and positions of elements.
5. **Paint**: The browser paints the visual elements on the screen.
6. **Composite Layers**: The browser creates layers (if necessary) and composites them for display.

## Performance Cost:

Even small DOM changes can trigger expensive operations like reflow and repaint.

**Reflow**: A change in element size, position, or visibility can affect the layout of the entire page.
**Repaint**: Changes to appearance (like color or text) require redrawing the affected part of the page.

In large, complex web pages, frequent updates to the DOM can lead to performance bottlenecks due to excessive reflows and repaints.

# Why DOM Manipulations are Expensive

**Reflow is Recursive**: Changing one element’s layout can cause its parent, siblings, or children to reflow, propagating through the page.

**Synchronous DOM Operations**: Many DOM updates block other browser tasks because they require immediate reflow or repaint.

**Rendering Pipeline Triggered**: Even small changes (like text updates) can affect multiple steps of the rendering process.

# Virtual DOM and Performance Optimization

**Virtual DOM**: An in-memory representation of the real DOM used by frameworks like React to optimize performance.

## How It Works:

React updates the Virtual DOM first (in memory).
A **diffing algorithm** compares the old Virtual DOM with the new one to find the minimal set of changes.

Only the identified changes are applied to the real DOM.
Benefits:

Batching Updates: React batches multiple changes and applies them in a single operation, reducing the frequency of reflows and repaints.

**Efficient Diffing**: React updates only the parts of the DOM that actually changed.
Avoids Unnecessary DOM Manipulation: Changes happen in the Virtual DOM, limiting interaction with the real DOM.

# Virtual DOM in Practice

**In-memory Updates**: Instead of directly modifying the real DOM, React makes changes in memory first.
**Selective Updates**: Only the parts of the UI that are affected by changes are updated, not the entire DOM.
**Minimal Reflows/Repaints**: By avoiding frequent real DOM updates, React reduces the scope and number of layout recalculations and repaints.
**Reduced Complexity**: Virtual DOM is lightweight and much faster to manipulate compared to the real DOM.

## How Virtual DOM Solves Performance Issues

**Minimized DOM Interactions**: Directly manipulating the real DOM triggers expensive operations, but with the Virtual DOM, updates occur in memory first.
**Batching and Diffing**: Multiple updates are batched and then selectively applied, limiting the scope of reflows and repaints.
**Avoids Full Reflows**: React’s diffing algorithm identifies the exact changes and updates only those, avoiding recalculating layout for large portions of the page.
**Layered Optimization**: Modern browsers optimize rendering by splitting complex elements into layers. React minimizes the work needed to update these layers.

# Virtual DOM

Virtual DOM is a lightweight, in-memory representation of the real DOM.It is used by frameworks like React(and other libraries like vue.js) to optimize UI updates and improve performance. Instead of directly manipulating the real DOM, changes are made in the Virtual DOM first, avoiding costly operations like reflows and repaints.

## Key features

1. The Virtual DOM is a lightweight, in-memory copy of the actual DOM. It allows React to update the UI in memory first, rather than making changes directly to the real DOM every time something changes.

# Creating and nesting components

React apps are made out of **components**. A component is a piece of the UI (user interface) that has its own logic and appearance. A component can be as small as a button, or as large as an entire page.

**React components are JavaScript functions that return markup**

```jsx
function MyButton () {
    return (
        <button>I'm a button </button>
    )
}
```

The component `MyButton` can nest it into another component:

```jsx
export default function MyApp() {
    return (
        <div>
            <h1>Welcome to my app</h1>
            <MyButton />
        </div>
    );
}
```

Note that react component(<MyButton />) always starts with a capital letter while html tags should be lower case.

# Writing markup with JSX

The markup syntax you’ve seen above is called JSX. It is optional, but most React projects use JSX for its convenience.

JSX is stricter than HTML. You have to close tags like <br />. Your component also can’t return multiple JSX tags. You have to wrap them into a shared parent, like a <div>...</div> or an empty <>...</> wrapper:

```jsx
function AboutPage() {
  return (
    <>
      <h1>About</h1>
      <p>Hello there.<br />How do you do?</p>
    </>
  );
}
```

# Adding Styles

In React, you specify a CSS class with `className`. It works the same way as the HTML `class` attribute

```jsx
<img className="avatar" />
```

in css file

```css
/* In your CSS */
.avatar {
  border-radius: 50%;
}
```

# Displaying data

JSX lets you put markup into JavaScript. Curly braces let you “escape back” into JavaScript so that you can embed some variable from your code and display it to the user. For example, this will display `user.name`

```jsx
return (
  <h1>
    {user.name}
  </h1>
);
```

A more complex expression insdie the jsx curly braces like string concatenation:

```jsx
const user = {
    name: 'Hedy Lmarr',
    imageUrl: 'https://i.imgur.com/yXOvdOSs.jpg',
    imageSize: 90,
};

export default function Profile() {
    return (
        <>
            <h1>{user.name}</h1>
            <img
                className="avatar"
                src={user.imageUrl}
                alt={'Photo of' + user.name}
                style={{
                    width: user.imageSize,
                    height: user.imageSize
                }}
            />
        </>
    );
}
```

# JSX

JSX (JavaScript XML) is a syntax extension for JavaScript that allows you to write HTML-like code directly within JavaScript. It’s commonly used in React to describe what the UI should look like. SX is not required in React, but it makes it easier to visualize and structure the component tree in a more readable way.

Like this:

```jsx
const element = <h1>Hello, World!</h1>;
```

This is tranformed by Babel (or another compiler) into standard JS calls. JSX isn't a string or HTML—it produces React elements, which are plain JavaScript objects that React uses to build the actual DOM.

```
const element = React.createElement('h1', null, 'Hello, World!');
```

### Using React without JSX:

You can write your components entirely in JavaScript without JSX:

```jsx
function MyComponent() {
  return React.createElement(
    'div',
    null,
    React.createElement('h1', null, 'Welcome to React!'),
    React.createElement('p', null, 'This is a component.')
  );
}
```

## Returning multiple JSX elements

If you're returning multiple JSX elements, they must be wrapped inside a parent element (like a <div>, <section>, or <Fragment>).

```jsx
return (
  <div>
    <h1>Hello, World!</h1>
    <p>This is JSX.</p>
  </div>
);
```

Alternatively, you can use React's Fragment (<>...</>) to avoid unnecessary wrapper elements(like the div here).

# How JSX is Compiled

JSX is not valid JavaScript, so it needs to be transpiled into regular JavaScript before the browser can understand it.

Here's the steps:

1. **The JSX you write in your code**

Your code:

```jsx
const element = <h1>Hello, World!</h1>;
```

2. **Babel Transpilation**

Before the browser can understand JSX, a tool like Babel is used to transpile it into regular JavaScript.

It converts the above JSX elements into somthing like this:

```jsx
const element = React.createElement('h1', null, 'Hello, World!');
```

The `React.createElement()` function is called to describe what the UI should look like. It creates a plain JavaScript object representing the element.

3. **JavaScript Execution**

The browser runs this JavaScript code, and React uses this information to build the **Virtual DOM** (Not familiar? Read about virtual dom above).

4. **React.createElement():**

Every JSX element gets compiled to a **React.createElement()** call. For example:

```jsx
<div>
  <h1>Hello</h1>
  <p>World</p>
</div>
```

which get's transpiled into:

```jsx
React.createElement('div', null,
  React.createElement('h1', null, 'Hello'),
  React.createElement('p', null, 'World')
);
```

Here createElement function typically takes three arguments:-

`type` - type of element or component you want to create.

`props` - An object representing any attributes or properites you want to set on element (eg:- **className**, **onClick**, etc). **null** meaning there are no propes.

`children` - Content inside the element, which could be a text, other elements or an array of elements

The `createElement` function returns a plain JavaScript object that represents a React element. This object contains information about the element type, any props (attributes), and its children

```jsx
{
  type: 'h1',
  props: {
    children: 'Hello, World!'
  },
  key: null,
  ref: null
}
```

`type`: `'h1'` — The type of the DOM element to be created.

`props`: `{ children: 'Hello, World!' }` — Since there are no props passed (the second argument is `null`), React creates a default `props` object, with a `children` property set to `'Hello, World!'`.

`key` and `ref`: These are special properties used by React internally for managing lists of elements and component references. In this simple example, they are null.

### More consice example with multiple child elements

```jsx
React.createElement('div', null,
  React.createElement('h1', null, 'Hello'),
  React.createElement('p', null, 'World')
);
```

which creates the following React element object:

```jsx
{
  type: 'div',
  props: {
    children: [
      {
        type: 'h1',
        props: { children: 'Hello' },
        key: null,
        ref: null
      },
      {
        type: 'p',
        props: { children: 'World' },
        key: null,
        ref: null
      }
    ]
  },
  key: null,
  ref: null
}
```

After `React.createElement()` builds this JavaScript object, React uses it internally during the rendering process to construct the **Virtual DOM**.

This object is **not the actual DOM node**, it’s just a **description** of what the DOM should look like (a **virtual representation**).

### What Does React Do with This Object?

1. **Virtual DOM Creation**:

The object returned by `React.createElement()` becomes part of React's **Virtual DOM**, a lightweight copy of the actual DOM that React maintains in memory.

This Virtual DOM is just JavaScript objects representing the structure of your UI.

2. **Reconciliation and Diffing**:

React uses the Virtual DOM to compare with previous versions of the UI. It determines what has changed by "diffing"(an algorith) the new Virtual DOM against the previous one.

3. **Updating the Real DOM**:

After React figures out the minimal changes needed (based on the Virtual DOM diffing), it efficiently updates the real DOM by making only the necessary changes.

# React's Rendering Process

Once the JavaScript (including the JSX-compiled code) is running, here's how React builds and updates the UI.

1. **Initial Rendering**

When react renders a component for the first time, it runs the components `render()` fundtion (or in function components, it just calls the function). Inside the render react returns a **React element**(a plain js object) describing what the DOM should look like. React builds the **Virtual DOM** based on these React elements.

2. **Virtual DOM**

React doesn't manipulate the real DOM directly because real DOM operations are expensive so it creattes a lighhtweight virtual copy of the real DOM in memory, where it keeps track of ehat the UI should look like.

3. **React DOM and React Fiber**

Reactt uses a system called **React Fiber**, an internal mechanism that manages the rendering updates. Fiber allows React to break rendering work into units, making update more efficient. When changes occur React compares the new Virtual DOM with the previous one. This is called **reconciliation**. It figues out the minimal set of changes needed to update the actual DOM by **diffing** the virtual DOM trees.

4. **Diffing Algorithm**

An efficient algorithm to compare the new Virtual DOM with the old one. This a;gortihm identifies changes and only update those parts in the real DOM. This avoids the need to ew-render the entire document, whch is inefficient.

5. **Real DOM Update**

React finds the minimal changes and update the real DOM
eg: if only one text node in a large list of elements changes, React will only update that specific text node, not the entire list.

6. **Commit Phase**

Once React has figured out which changes to make, it batches these updates and applies them to the real DOM during the `commit phase`. This is when the browser's `reflow` and `repaint` processes occur.

## Breifing on How React Builds the DOM

- JSX is compiled by Babel into React.`createElement()` calls.

- React creates a **Virtual DOM** from these calls.

- When updates happen (via state changes, prop updates, etc.), React uses a **diffing algorithm** to compare the new Virtual DOM with the old one.

- Only the necessary updates are applied to the real DOM, reducing the performance overhead.

- The Virtual DOM and React’s reconciliation process make updates faster and more efficient than direct DOM manipulation in vanilla JavaScript.

By handling DOM updates this way, React offers significant performance advantages, especially for **dynamic, complex user interfaces**.
