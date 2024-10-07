# DOM Manipulation and Performance Issues

DOM (Document Object Model): A tree structure where each element, attribute, and text in HTML is represented as an object.

Browser Rendering Pipeline: When the DOM is manipulated, it triggers a sequence of steps:

1. **DOM Tree Construction**: HTML is parsed to build the DOM tree.
2. **CSSOM Tree Construction**: CSS is parsed to build the CSSOM.
3. **Render Tree Construction**: The DOM and CSSOM are combined to create the render tree (only elements visible on the page).
4. **Layout (Reflow)**: The browser calculates sizes and positions of elements.
5. **Paint**: The browser paints the visual elements on the screen.
6. **Composite Layers**: The browser creates layers (if necessary) and composites them for display.

# What All Process Happens When An Html File Is Opened

1. ### HTML File Fetching

   The browser requests the HTML file from the server (if hosted online) or the local filesystem.
   The HTML file is downloaded or read by the browser.

2. ### HTML Parsing & DOM Tree Construction

   The browser parses the HTML file line by line.
   As it parses, it constructs the DOM (Document Object Model) tree. The DOM is a structured representation of the HTML document in memory.
   Each HTML element (e.g., <div>, <p>) becomes a node in the DOM tree.
   Elements are nested inside one another based on their hierarchy in the HTML.

3. ### CSS Parsing & CSSOM Tree Construction

   If there are any <style> tags or external CSS files linked, the browser fetches and parses the CSS.
   The parsed CSS is used to create the CSSOM (CSS Object Model), a tree structure similar to the DOM but for styles.
   CSS rules are matched to corresponding DOM elements.

4. ### Render Tree Construction

   The browser combines the DOM and CSSOM to create the Render Tree.
   The render tree contains only the nodes that need to be rendered on the screen (e.g., elements with display: none are excluded).
   It associates visual information (like dimensions, color) with DOM nodes.

5. ### Layout (Reflow)

   The browser calculates the positions and sizes of all elements in the render tree.
   This is called layout or reflow.
   It ensures elements are placed correctly according to their styles (e.g., width, height, margin, etc.).

6. ### Paint

   Once the layout is calculated, the browser paints the pixels for each visible element on the screen.
   This is when actual content like text, images, and borders get drawn.

7. ### #Composite Layers

   If the page has complex elements (e.g., animations, 3D transforms), the browser creates layers for those parts.
   These layers are composited together to produce the final rendered page.

Once all these steps are complete, the web page becomes visible to you.

## Performance Cost:

Even small DOM changes can trigger expensive operations like reflow and repaint.

**Reflow**: A change in element size, position, or visibility can affect the layout of the entire page.
**Repaint**: Changes to appearance (like color or text) require redrawing the affected part of the page.

In large, complex web pages, frequent updates to the DOM can lead to performance bottlenecks due to excessive reflows and repaints.

## What All Process Happend for Different Changes

1. ### DOM Manipulation

Changes to the structure or content of the DOM (e.g., adding, removing, or modifying elements).

Example Change:

Adding a new <div> element to the DOM using JavaScript:

```javascript
const newDiv = document.createElement("div");
document.body.appendChild(newDiv);
```

**Processes Triggered:**

- DOM Tree Update:
  The DOM tree gets updated to reflect the addition of the new element.

- Layout (Reflow):
  Since the structure has changed, the browser needs to recalculate the layout of the entire page, determining the new positions and sizes of elements. This is known as reflow.

- Paint:
  After the layout is recalculated, the browser paints the newly added element on the screen.

- Composite Layers (if needed):
  If the change involves complex transformations or animations, the browser may create additional composite layers.

2. ### Style Changes (CSS or Inline Styles)

Changes to the appearance of elements without modifying the DOM structure (e.g., changing the color, width, background).

Example Change:

Changing the background color of an element:

```javascript
const element = document.getElementById("myElement");
element.style.backgroundColor = "blue";
```

**Processes Triggered:**

- CSSOM Update:
  If the style change affects CSS rules, the CSSOM is updated.

- Layout (Reflow) (Sometimes):
  If the style change affects the size or position of elements (e.g., width, height, margin), the browser needs to reflow and recalculate the layout. For example, increasing the width of an element triggers a reflow.

- Paint:
  The browser paints the changes (e.g., the new background color).

No Composite Layers (unless there are complex visual effects involved).

3. ### Visual or Graphical Changes (No Layout Impact)

Some changes only affect the visual appearance of an element but don’t require recalculating layout positions (e.g., changing color, opacity, transform, box-shadow).

Example Change:

Changing the opacity of an element:

```javascript
const element = document.getElementById("myElement");
element.style.opacity = 0.5;
```

**Processes Triggered:**

- Paint:
  Since there’s no structural change to the DOM or layout, the browser only needs to repaint the element with the new visual style (e.g., semi-transparent appearance).

- No Reflow:
  Reflow isn’t required because the element’s size and position remain the same.

- Composite Layers:
  For properties like opacity, transform, or box-shadow, the browser often handles these using separate composite layers for better performance.

4. ### Content Changes (Text or Media)

Modifying the content of an element (e.g., changing text, updating an image source).

Example Change:

Changing the text of a paragraph element:

```javascript
const element = document.getElementById("myElement");
element.textContent = "New text content";
```

**Processes Triggered:**

- DOM Tree Update:
  The content within the node is updated in the DOM tree.

- Layout (Reflow) (Sometimes):
  If the new content changes the size of the element (e.g., longer text), the layout needs to be recalculated (reflow).

- Paint:
  The browser paints the updated content on the screen.

5. ### User-Triggered Changes (e.g., Input Fields, Scrolling)

   Changes caused by user interaction (e.g., typing in a text field, scrolling the page).

Example Change:

    A user types into an input field.

**Processes Triggered:**

    * DOM Tree Update (if necessary):
        If the content of the input field needs to be stored in the DOM, it may trigger an update.

    * No Layout (for text input):
        In most cases, typing in an input field doesn’t affect the layout unless the input box resizes.

    * Repaint (if necessary):
        The browser may repaint the updated content, especially in cases where the field visually changes (e.g., the cursor moves, characters are displayed).

# Wha is React?

An open source library for building user interfaces. React has a rich ecosystem and plays really well with other libraries and is more than well capable to build full filled libraries. IT is a project created and maintained by facebook.

Reach has a component based architecture. This lets you break down your application into small enacapsulated part which can then be composed to make more complex UI's.

**React is declarative** - Tell react what you want and React will build the actul UI

React can integrate with any of the application. A portion or complete or even an entire application itself.

## Components

A component represent a part of the user interface. Components are reusable. A component can have nested components.

### Component Types

Two types of component types

1. **Stateless Functional Component**

Functional components are literally JS functions. They return HTML which describes UI. Functinal components can ooptionally recieve an object of properties(props).

Eg:-

```jsx
function Welcome(props) {
  return <h1>Hello {props}</h1>;
}
////////////////////////////////////

const Welcome = () => <h1>Hello Vishwas</h1>;
```

2. **Stateful Class Component**

Class components are regular ES6 classes that extend the component class from the react library. They must contain a render method which inturn return HTML. Similar to functional componen class component can optionally recieve properties(props).

A class component can also maintain private internal state. In simple terms it can maintain some information which is private to that component and use that information to describe that component.

# Functional vs Class components

### Functional | Class

                                   |

- Simple functions | \* More feature rich
- Use Func components as much as | _ Miantain their own private data
  | which is state.
  possible | _ Complex UI logic
- Absenece of this keyword is an | _ Provide lifecycle hooks
  advantage | _ Stateful/ Smart/ Container
  | components
- Need to think of solution without|
  using state |
- Mainly responsible for the UI |

Eg:-

```jsx
class Welcome extends React.component {
  render() {
    return <h1>Hello, {this.props.name}</h1>;
  }
}
```

# What Happens When React Is Introduced

React introduces a layer of abstraction over the traditional DOM with concepts like the **Virtual DOM** and **React components**.

1. ### React Application Initialization

When you run a React app, the browser loads the HTML file (usually index.html), which has a root <div> element like:

```html
<div id="root"></div>
```

React’s JavaScript (usually index.js or app.js) is executed, which mounts the React components onto this root element.

2. ### Virtual DOM Creation (React Elements)

When a React component is rendered, it uses React.createElement (or JSX) to create a Virtual DOM representation.

React components return React Elements, which are lightweight, JavaScript objects representing the DOM structure in memory (virtual DOM). These objects contain:
Type of the element (e.g., 'div', 'h1').
Properties (e.g., attributes like className, id).
Children (nested elements or components).

**Example of React Element creation:**

```javascript
React.createElement("h1", null, "Hello World");
```

When JSX is used, it gets compiled into React.createElement calls behind the scenes.

3. ### Initial Virtual DOM Comparison

- React uses a Virtual DOM to keep track of changes in the UI.

- At first, React builds an initial Virtual DOM representation of your component tree.

- Nothing is rendered on the screen yet, as the Virtual DOM is just an in-memory structure.

4. ### Reconciliation: Diffing Virtual DOM with the Real DOM

- React compares the initial Virtual DOM with the real DOM.

  - Since this is the first render, the real DOM is empty (except the root <div id="root">), so React calculates what needs to be updated.

- React identifies that the real DOM needs to be populated with the elements described in the Virtual DOM.

5. ### Real DOM Update

- React then updates the real DOM based on the Virtual DOM.

  - React performs minimal operations on the real DOM, only rendering what’s necessary.

- The real DOM is now updated with the elements, and they are visible on the screen.

6. ### React State and Props Changes (Subsequent Renders)

- React components are dynamic because of state and props.

- When a component’s state or props change (e.g., due to user interaction, API calls), React re-renders that component.

**Here’s how React handles the changes:**

7. ### New Virtual DOM Creation

- Whenever a change occurs in the application, React creates a new Virtual DOM.

- React components re-run, producing an updated Virtual DOM structure.

8. ### Virtual DOM Diffing (Reconciliation)

- React compares the new Virtual DOM with the previous Virtual DOM.
- This process is called reconciliation.
  - It uses a diffing algorithm to identify which parts of the Virtual DOM have changed.

9. ### Batching and Efficient DOM Updates

- React batches changes to minimize updates to the real DOM.

- After diffing, React only updates the changed parts of the real DOM, instead of re-rendering the entire page.

  - This optimizes performance by avoiding unnecessary reflows and repaints.

10. ### Real DOM Update (Minimal)

- React then applies the minimal changes to the real DOM.

  - It might add, remove, or update specific DOM elements based on the new Virtual DOM.

11. ### React Component Lifecycle Methods

- During these updates, React invokes specific lifecycle methods (for class components) or hooks (for functional components with `useEffect`, `useState`) at different points in the render cycle.

  - For example, `componentDidMount` is called after the component is mounted, and `componentDidUpdate` is called after re-render.

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
function MyButton() {
  return <button>I'm a button </button>;
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
      <p>
        Hello there.
        <br />
        How do you do?
      </p>
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
return <h1>{user.name}</h1>;
```

A more complex expression inside the jsx curly braces like string concatenation:

```jsx
const user = {
  name: "Hedy Lmarr",
  imageUrl: "https://i.imgur.com/yXOvdOSs.jpg",
  imageSize: 90,
};

export default function Profile() {
  return (
    <>
      <h1>{user.name}</h1>
      <img
        className="avatar"
        src={user.imageUrl}
        alt={"Photo of" + user.name}
        style={{
          width: user.imageSize,
          height: user.imageSize,
        }}
      />
    </>
  );
}
```

# Responding to events

You can respond to events by declaring event handler functions inside your components:

```jsx
function MyButton() {
  function handleClick() {
    alert("You clicked me!");
  }

  return <button onClick={handleClick}>Click me</button>;
}
```

Notice how `onClick={handleClick}` has no parentheses at the end! Do not call the event handler function: you only need to pass it down. React will call your event handler when the user clicks the button.

# Updating the screen

Often, you’ll want your component to “remember” some information and display it. For example, maybe you want to count the number of times a button is clicked. To do this, add state to your component.

First, import `useState` from React:

```jsx
import { useState } from "react";
```

Now you can declare a state variable inside your component:

```jsx
function MyButton() {
  const [count, setCount] = useState(0);
  // ...
```

You’ll get two things from `useState`: the current state (`count`), and the function that lets you update it (`setCount`). You can give them any names, but the convention is to write `[something, setSomething]`.

The first time the button is displayed, `count` will be `0` because you passed 0 to `useState()`. When you want to change state, call `setCount()` and pass the new value to it. Clicking this button will increment the counter:

```jsx
function MyButton() {
  const [count, setCount] = useState(0);

  function handleClick() {
    setCount(count + 1);
  }

  return <button onClick={handleClick}>Clicked {count} times</button>;
}
```

React will call your component function again. This time, count will be 1. Then it will be 2. And so on.

**If you render the same component multiple times, each will get its own state. **

# Using Hooks

Functions starting with `use` are called Hooks. `useState` is a built-in Hook provided by React.

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

Each JSX element is a syntactic sugar for calling React.createElement. THat is you can write your components entirely in JavaScript without JSX:

```jsx
function MyComponent() {
  return React.createElement(
    "div",
    null,
    React.createElement("h1", null, "Welcome to React!"),
    React.createElement("p", null, "This is a component.")
  );
}
```

React.createElement("html tag", properties, child elemment)

Here React.createElement takes the arguments:

1. A string which specifies the Html tag to be rendered
2. Optional properties - object of key value pairs that will be applied to the element. Basicaly atributes for the element given inside an object. Unknown properties are ignored.
3. Child element

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

Alternatively, you can use <React.Fragment>...</> or <>...</> to avoid unnecessary wrapper elements(like the div here).

### differences provided by JSX

class -> className
for -> htmlFor
camelCase property naming convention

- onclick -> onClick
- tabindex -> tabIndex

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
const element = React.createElement("h1", null, "Hello, World!");
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
React.createElement(
  "div",
  null,
  React.createElement("h1", null, "Hello"),
  React.createElement("p", null, "World")
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
React.createElement(
  "div",
  null,
  React.createElement("h1", null, "Hello"),
  React.createElement("p", null, "World")
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

# Webpack

Webpack is a powerful and popular module bundler for JavaScript applications. It takes various modules and assets (JavaScript, CSS, images, fonts, etc.) and combines them into one or more bundled files (or "chunks"). Webpack helps manage dependencies and optimizes the project for production by creating smaller, faster, and more efficient files.
React apps often use a lot of JavaScript modules, including custom components, libraries, and third-party dependencies. Each of these is typically written in separate files. Webpack bundles all of these modules together into one or more files that can be efficiently served to the browser.

**Without Webpack**: You would have to manually manage and load all the JavaScript files individually in the correct order, which is inefficient.

**With Webpack**: It intelligently bundles everything into as few files as possible to minimize HTTP requests, reducing load times.

# Commonly used library in React

## 1. Lodash

**Lodash** is a popular JavaScript utility library that provides a wide variety of functions to simplify common programming tasks such as array manipulation, object manipulation, and data transformation.

## 2. Axios

Axios is a promise-based HTTP client for JavaScript, which makes it easier to send requests to a server (e.g., GET, POST, PUT, DELETE) and handle the responses. It works both in the browser and Node.js, making it highly versatile.

Axios is popular in **React** for making API requests because it simplifies managing asynchronous operations, includes useful default configurations, and can be used with `useEffect` to fetch data in functional components.

## 3. Moment.js

Handle complex route structures with nested routes, rendering child routes within parent components.

**Moment.js** is a JavaScript library for parsing, validating, manipulating, and formatting dates and times. Although it is no longer under active development, it remains widely used in legacy codebases.

## 4. React Router

**React Router** is the standard routing library for React applications. It allows developers to create dynamic, single-page applications (SPAs) with multiple views, enabling navigation between components without reloading the page.

### Some features

- **Declarative Routing**: Define routes and the components associated with each route using JSX.

- **Nested Routes**: Handle complex route structures with nested routes, rendering child routes within parent components.

- **Dynamic Routing**: Supports URL parameters and dynamic routes for building more interactive and flexible navigation.

- **Hooks Support**: `useNavigate`, `useParams`, and `useLocation` hooks provide powerful access to navigation, route parameters, and URL details.

```jsx
import { BrowserRouter as Router, Routes, Route } from "react-router-dom";

function App() {
  return (
    <Router>
      <Routes>
        <Route path="/" element={<Home />} />
        <Route path="/about" element={<About />} />
        <Route path="/profile/:id" element={<UserProfile />} />{" "}
        {/* Dynamic Route */}
      </Routes>
    </Router>
  );
}
```

**State management: React state management is the process of managing a React component's state, which determines how the component appears and behaves. The component's current state is represented by the React state, which is subject to change over time because of user input, network requests, or other events.**

## 5. Redux / Zustand / Recoil

State management libraries are often needed in large React applications to manage global state. Here are a few common ones:

- **Redux**: A widely used state management library that helps manage complex state across large applications. It uses a single global store, with reducers and actions to update the state. Redux is powerful but can be overly complex for small apps.
  **Redux Toolkit**: A more modern and simplified approach to Redux, which includes built-in utilities like `createSlice`, `configureStore`, and `createAsyncThunk` to make the Redux process more efficient and developer-friendly.

**Zustand**: A lightweight and simple state management library that's gaining popularity for being easy to set up. It's more minimalistic than Redux but still effective for managing global state in React apps.

**Recoil**: A state management library developed by Facebook for React, offering a more flexible and granular approach to state compared to Redux. It integrates well with hooks and provides better support for managing asynchronous data.

```jsx
// Redux example

// Define a slice in Redux Toolkit
import { createSlice } from "@reduxjs/toolkit";

const counterSlice = createSlice({
  name: "counter",
  initialState: 0,
  reducers: {
    increment: (state) => state + 1,
    decrement: (state) => state - 1,
  },
});

export const { increment, decrement } = counterSlice.actions;
export default counterSlice.reducer;
```

## 6. Formik / React Hook Form

These libraries help with form management, validation, and handling form submissions in React apps.

- **Formik**: A popular library for building forms in React. It simplifies form validation, submission, and managing form state (e.g., touched fields, errors).

- **React Hook Form**: A lightweight form library that uses React hooks (`useForm`) to manage form states and validations. It’s praised for its minimalistic approach and fast performance.

```jsx
// with react hook

import { useForm } from "react-hook-form";

function MyForm() {
  const {
    register,
    handleSubmit,
    formState: { errors },
  } = useForm();

  const onSubmit = (data) => {
    console.log(data);
  };

  return (
    <form onSubmit={handleSubmit(onSubmit)}>
      <input {...register("name", { required: true })} />
      {errors.name && <span>This field is required</span>}
      <button type="submit">Submit</button>
    </form>
  );
}
```

## 7. Styled Components

**Styled Components** is a CSS-in-JS library that allows developers to write CSS code inside JavaScript components in a scoped and modular way. It enables dynamic styling and theming without relying on external stylesheets.

## 8. Jest and React Testing Library

Jest is a popular testing framework for JavaScript, and React Testing Library is built specifically to test React components. Together, they simplify writing unit and integration tests for React applications.

## 9. Framer Motion

Framer Motion is an animation library for React that makes it easy to animate components with fluid and customizable animations. It provides a declarative way to handle animations using props, and it integrates seamlessly with React’s JSX syntax.

## 10. SWR / React Query

These are data-fetching libraries designed to manage server state in React.

- **SWR**: A library developed by Vercel for remote data fetching. SWR stands for “stale-while-revalidate,” a cache invalidation strategy that allows fetching data while keeping the old one until the new data arrives.

- **React Query**: A robust data-fetching library that handles caching, synchronization, and background fetching, making it easier to work with server-side state.

# CLASS IN JS

In JavaScript, a class is a blueprint for creating objects with shared properties and methods. It allows you to define the structure and behavior of objects in a more organized and reusable way. class syntax was introduced in ES6 (ECMAScript 2015) and provides a cleaner, more intuitive way to implement object-oriented programming (OOP) principles.

### Key Uses of Classes in JS

1. **Object-Oriented Programming (OOP)**: Classes are foundational to object-oriented programming, which organizes code around objects, each representing a real-world entity or concept. By defining classes, you can create objects that encapsulate data (properties) and behavior (methods).

2. **Encapsulation**: A class allows you to bundle data and behavior into a single unit. This encapsulation makes it easier to manage the complexity of larger programs by keeping related functionality together.

```js
class Person {
  constructor(name, age) {
    this.name = name;
    this.age = age;
  }

  greet() {
    console.log(`Hello, my name is ${this.name}.`);
  }
}

const person1 = new Person("John", 30);
person1.greet(); // "Hello, my name is John."
```

3. **Inheritance**: Classes support inheritance, allowing you to create new classes based on existing ones. This is useful for creating more specific types of objects while reusing common functionality from a parent class.

```js
class Animal {
  constructor(name) {
    this.name = name;
  }

  speak() {
    console.log(`${this.name} makes a sound.`);
  }
}

class Dog extends Animal {
  speak() {
    console.log(`${this.name} barks.`);
  }
}

const dog = new Dog("Buddy");
dog.speak(); // "Buddy barks."
```

4. **Reusability**: Classes provide a way to define reusable structures. You can create multiple instances (objects) of a class, each with its own specific data, while sharing the same methods. This promotes code reuse and consistency.

```js
const person2 = new Person("Alice", 25);
const person3 = new Person("Bob", 40);

person2.greet(); // "Hello, my name is Alice."
person3.greet(); // "Hello, my name is Bob."
```

5. **Constructor Function Replacement**: Before ES6, JavaScript developers used constructor functions and prototypes for creating objects and inheritance. Classes provide a more straightforward and readable syntax for these concepts, but under the hood, they still use prototypes.

```js
// Pre-ES6 constructor function
function Car(model) {
  this.model = model;
}

Car.prototype.drive = function () {
  console.log(`${this.model} is driving.`);
};

const car1 = new Car("Tesla");
car1.drive(); // "Tesla is driving."
```

**The above example can be written using ES6 classes:**

```js
class Car {
  constructor(model) {
    this.model = model;
  }

  drive() {
    console.log(`${this.model} is driving.`);
  }
}

const car1 = new Car("Tesla");
car1.drive(); // "Tesla is driving."
```

6. **Static Methods**: Classes can define static methods, which are called on the class itself rather than on instances of the class. These are useful for utility functions that don’t need to interact with the instance's properties.

```js
class MathUtility {
  static add(a, b) {
    return a + b;
  }
}

console.log(MathUtility.add(5, 3)); // 8
```

7. **Getters and Setters**: JavaScript classes support getters and setters, which allow you to define custom behavior when accessing or modifying a property. This can be useful for validation or computed properties.

```js
class Rectangle {
  constructor(width, height) {
    this.width = width;
    this.height = height;
  }

  get area() {
    return this.width * this.height;
  }

  set width(value) {
    if (value <= 0) {
      console.log("Width must be greater than zero.");
    } else {
      this._width = value;
    }
  }

  get width() {
    return this._width;
  }
}

const rect = new Rectangle(5, 10);
console.log(rect.area); // 50
rect.width = -3; // "Width must be greater than zero."
```

# STATE

State is an object privately maintained in a component. State can influence what is rendered in browser. State can be changed within a component.

When using setState:

- Always make use of setState and never modify the state directly
- Code has to b executed after the statehas been updated? Place that code in the callback function which is the second argument to the setState method.
- When you have to update state based on the previous state value, pass in a function as an argument instead of the regular object.

**Old Method to set state:**

```jsx
import { Component } from "react";

class Message extends Component {
  constructor() {
    super();
    this.state = {
      message: "Welcome visitor",
    };
  }

  changeMessage() {
    this.setState({
      message: "Thank you for clicking",
    });
  }
  render() {
    return (
      <>
        <h1>{this.state.message}</h1>
        <button onClick={() => this.changeMessage()}>click me</button>
      </>
    );
  }
}

function App() {
  return (
    <div className="App">
      <Message />
    </div>
  );
}

export default App;
```

**Counter example:-**

```jsx
import React, { Component } from "react";

class Counter extends Component {
  constructor() {
    super();
    this.state = {
      count: 0,
    };
  }
  increment() {
    // this.setState(
    //   {
    //     count: this.state.count + 1,
    //   },
    //   () => {
    //     console.log(this.state.count); // This properly shows the updated value
    //   }
    // );
    // console.log(this.state.count); // This shows the value before the state value is set

    this.setState(
      (prevState, props) => ({
        count: prevState.count + 1,
      }),
      () => {
        console.log("Properly increase the value", this.state.count);
      }
    );
  }

  incrementFive() {
    this.increment();
    this.increment();
    this.increment();
    this.increment();
    this.increment();
  }

  render() {
    return (
      <div>
        <h1>Count - {this.state.count}</h1>
        <button onClick={() => this.increment()}>add</button>
        <button onClick={() => this.incrementFive()}>add 5</button>
      </div>
    );
  }
}

export default Counter;
```

# PROPS vs STATE

- Props gets passed to the component whereas state is managed within the component
- Props is passed form functional pararmeters while state are variables declared in the function body
- Props are immutable while state can be changed
- How both is used in
  - In functional components - Props: props - state: useState Hook
  - In class components - Props: this.props - state: this.state

# Binding Event Handler (class components)

The reason why we bind event handlers is because of how this keyword works in JS. In class components, the value of `this` in a method like `clickHandler` might not automatically refer to the component instance unless you bind it. If you don't bind `this`, `clickHandler` won't have the correct context when called as an event handler.

An example without binding

```js
import React, { Component } from "react";

class EventBind extends Component {
  constructor(props) {
    super(props);
    this.state = {
      message: "Hello",
    };
  }

  clickHandler() {
    this.setState({
      message: "Goodbye",
    });
  }

  render() {
    return (
      <div>
        <div>{this.state.message}</div>
        <button onClick={this.clickHandler}>Click</button> // when button is clicked
        it shows error saying this is undefined
      </div>
    );
  }
}
```

**First approach** is that it can be mitigated by binding the this keyword

```js
....
<button onClick={this.clickHandler.bind(this)}>Click</button>

....

```

Even though this works fine it causes every update to the component to re-render. This inturn will generate a brand new event handler on every render. It could be severe on large applications and component that contains nested children.

**The second approach** is to use arrow functions in the render method

```js
....
<button onClick={() => this.clickHandler()}>Click</button>

....

```

**Third approach** is the most used and the approach used in the official react documentation.

This approach deals with binding the event handler in the constructor as opposed to binding in the render method.

```js
....
constructor(props) {
    super(props);
    this.state = {
      message: "Hello",
    };

    this.clickHandler = this.clickHandler.bind(this);
  }

  clickHandler() {
    this.setState({
      message: "Goodbye",
    });
  }

....

<button onClick>{this.clickHandler}>Click</button>

```

**Final approach** is to use arrow function as a class property

```js
clickHandler = () => {
  this.setState({
    message: "Goodbye",
  });
};
```

# Methods as Props (Child component to parent component)

We use props for this. We pass in reference to a method as props to the child component.

**Parent Component**:

```js
import React, { Component } from "react";
import ChildComponent from "./ChildComponent";

export class ParentComponent extends Component {
  constructor(props) {
    super(props);

    this.state = {
      parentName: "Parent",
    };

    this.greetParent = this.greetParent.bind(this);
  }

  greetParent(childName) {
    alert(`Hello ${this.state.parentName} from ${childName}`);
  }
  render() {
    return (
      <div>
        <ChildComponent greetParent={this.greetParent} />
      </div>
    );
  }
}

export default ParentComponent;
```

**Child Component**

```js
import React from "rea ct";

function ChildComponent({ greetParent }) {
  return (
    <div>
      <button onClick={() => greetParent("child")}>GreetParent</button>
    </div>
  );
}

export default ChildComponent;
```

# Conditional Rendering

1. **if/else**

In React, you can use `if/else` statements to conditionally render different components or elements. However, you cannot directly use `if/else` inside JSX as JSX is only a syntactic sugar for function calls and object construction. Instead, you handle it before the `return` statement, or inside the render method of class components.

```jsx
function Greeting(props) {
  const isLoggedIn = props.isLoggedIn;

  if (isLoggedIn) {
    return <h1>Welcome back!</h1>;
  } else {
    return <h1>Please log in.</h1>;
  }
}

export default Greeting;
```

Useful for more complex conditional logic.

2. **Element variables**

You can use variables to store elements and conditionally assign them based on logic. Then, you can use that variable in the JSX.

```jsx
function Greeting(props) {
  const isLoggedIn = props.isLoggedIn;
  let message;

  if (isLoggedIn) {
    message = <h1>Welcome back!</h1>;
  } else {
    message = <h1>Please log in.</h1>;
  }

  return <div>{message}</div>;
}

export default Greeting;
```

3. **Ternary conditional operator**

The ternary operator is a more concise way to conditionally render elements. It works similarly to if/else but can be written inside the JSX itself. It's a single line, which makes it good for simpler conditions.

```jsx
function Greeting(props) {
  return (
    <div>
      {props.isLoggedIn ? <h1>Welcome back!</h1> : <h1>Please log in.</h1>}
    </div>
  );
}

export default Greeting;
```

Useful for simple conditinal rendering in a compact form. Typically used directly inside the JSX

4. **Short circuit operator**

The short-circuiting `&&` operator allows conditional rendering by only rendering an element if the condition is `true`. If the condition is `false`, React skips rendering the expression after `&&`.

```jsx
function Greeting(props) {
  return <div>{props.isLoggedIn && <h1>Welcome back!</h1>}</div>;
}

export default Greeting;
```

If props.isLoggedIn is true 'Welcome back!' is rendered otherwise nothing is rendered.

Useful for conditional rendering where only one outcome is needed (no else condition).

# Styling React Components

1. CSS stylesheets

You can style React components using traditional CSS stylesheets by importing the `.css` file into your component.

**How to use**:

- Create a regular CSS file.
- Import the CSS file into your component.
- Use the class names from the CSS file in your JSX elements.

CSS:

```css
/* styles.css */
.container {
  background-color: lightblue;
  padding: 20px;
}
```

JSX:

```jsx
// App.js
import "./styles.css";

function App() {
  return <div className="container">Hello, World!</div>;
}

export default App;
```

2. Inline styling

You can apply styles directly to elements using the style attribute, which accepts a JavaScript object. Each CSS property is written in camelCase.

**How to use:**

- Use the style attribute and pass an object.
- CamelCase is required for multi-word CSS properties (e.g., `backgroundColor` instead of `background-color`).

```jsx
function App() {
  const style = {
    backgroundColor: "lightblue",
    padding: "20px",
    color: "darkblue",
  };

  return <div style={style}>Hello, World!</div>;
}

export default App;
```

3. CSS Modules

CSS Modules allow you to scope your CSS to a particular component, preventing conflicts and ensuring that styles are applied only to the component where they are imported.

How to use:

- Create a CSS file with a `.module.css` extension.
- Import the CSS file as a module.
- Use the styles in the JSX by referencing the imported object.

CSS:

```css
/* styles.module.css */
.container {
  background-color: lightblue;
  padding: 20px;
}
```

JSX:

```jsx
import styles from "./styles.module.css";

function App() {
  return <div className={styles.container}>Hello, World!</div>;
}

export default App;
```

4. CSS in JS libraries

CSS-in-JS refers to a styling approach where CSS is written within JavaScript. Libraries like styled-components, Emotion, and JSS allow you to define your styles in JavaScript, offering features like dynamic styling based on props and scoped styles.

**How to use (example with styled-components):**

- Install the library (e.g., `npm install styled-components`).
- Use tagged template literals to define your styles inside your- JavaScript components.
- The styles are scoped to the component and can be dynamic based on props.

```jsx
import styled from "styled-components";

const Container = styled.div`
  background-color: lightblue;
  padding: 20px;
  color: ${(props) => (props.primary ? "white" : "darkblue")};
`;

function App() {
  return <Container primary>Hello, World!</Container>;
}

export default App;
```

# Form Handling (Controlled component)

In regular HTML for elements like input, text area etc are responsible on their own to handle the user input and update their respective values. But in react the form elements values are controlled by react. These are called **controlled components**.

So basically there are two types:

**Controlled and uncontrolled component**

1. A **controlled component** is a component where React controls and manages the state of the form elements. The form element's value is tied to the component's state, and any changes to the input are handled via event handlers that update the state. Here the input value is controlled by the state which is '**one-way data binding**'.

```jsx
import { useState } from "react";

function ControlledForm() {
  const [name, setName] = useState("");

  const handleChange = (e) => {
    setName(e.target.value); // Update state when input changes
  };

  const handleSubmit = (e) => {
    e.preventDefault();
    console.log(name);
  };

  return (
    <form onSubmit={handleSubmit}>
      <input type="text" value={name} onChange={handleChange} />
      <button type="submit">Submit</button>
    </form>
  );
}

export default ControlledForm;
```

Class example:

```jsx
import React, { Component } from "react";

class Form extends Component {
  constructor(props) {
    super(props);

    this.state = {
      text: "",
      comment: "",
      option: "React",
    };
  }

  handleUsernameChange = (event) => {
    this.setState({
      text: event.target.value,
    });
  };

  handleUserComment = (event) => {
    this.setState({
      comment: event.target.value,
    });
  };

  handleUserSelect = (event) => {
    this.setState({
      option: event.target.value,
    });
  };

  handleOnSubmit = (event) => {
    event.preventDefault();
    alert(`${this.state.text} ${this.state.comment} ${this.state.option}`);
  };

  render() {
    return (
      <form onSubmit={this.handleOnSubmit}>
        <input
          type="text"
          value={this.state.text}
          onChange={this.handleUsernameChange}
        />
        <textarea
          value={this.state.comment}
          onChange={this.handleUserComment}
        ></textarea>
        <h1>{this.state.text}</h1>
        <h1>{this.state.comment}</h1>
        <select value={this.state.option} onChange={this.handleUserSelect}>
          <option>React</option>
          <option>Vue</option>
          <option>Angular</option>
        </select>
        <button>submit</button>
      </form>
    );
  }
}

export default Form;
```

2. An **uncontrolled component** allows the form element to maintain its own internal state. In this case, React does not control the input value; instead, the DOM handles it. To access the value of the input, you typically use a `ref` to directly read from the DOM.

```jsx
import { useRef } from "react";

function UncontrolledForm() {
  const inputRef = useRef(null);

  const handleSubmit = (e) => {
    e.preventDefault();
    console.log(inputRef.current.value); // Access the DOM value via the ref
  };

  return (
    <form onSubmit={handleSubmit}>
      <input type="text" ref={inputRef} />
      <button type="submit">Submit</button>
    </form>
  );
}

export default UncontrolledForm;
```

### When to Use Each:

Controlled Components:

- When you need to handle form input validation, enforce specific behaviors, or control the input in real-time.
- When you need to manage the form state across multiple fields in a consistent and predictable way.
- When you're working with larger forms where you need React to track the state of each field.

Uncontrolled Components:

- When you only need the form data at the time of submission, and real-time tracking or validation isn’t required.
- For simple forms where using state for every input might add unnecessary complexity.
- When you want to quickly prototype or work with legacy code where form state is already managed by the DOM.

# Lifecycle Methods

When we create a component the component goes through several stages in its lifecycle. React provides us with build in methods that we can override at particular stages in the lifecycle.

Lifecycle methods on a class component:

we can mainly classify the methods in four phases:

1. **Mounting** - Called when an instance of a component is being created and inserted into the DOM. During this phase we have four methods here:

- constructors
- static method `getDerivedStateFromProps`
- `render`
- `componentDidMount`

2. **Updating** - Called when a component is being re-rendered as a result of changes to either its props or state. During this phase we have five methods:

- static method `getDerivedStateFromProps`
- `shouldComponentUpdate`
- `render`
- `getSnapshotBeforeUpdate`
- `componentDidUpdate`

3. **Unmounting** - Called when a component is being removed from the DOM. During this phase we have only one method:

- `componentWillUnmount`

4. **Error handling** - Called when there is an error during rendering, in a lifecyle method, or in the constructor of any child component. For error handling we have two methods"

- static method `getDerivedStateFromError`
- `componentDidCatch`

## Mounting Lifecycle Methods

These are methods which are called when an instance of a component is being created and inserted into the DOM. The order in which they are invoked

1. **Constructor**

- A special function that will get called whenever a new component is created
- Perfect for initializing state or binding the event handlers to the class instance
- you should never cause side effects. eg: HTTP requests , or making AJAX calls within the constructor
- You have to call the special function called super(props).
- Constructor is the only place where you are expected to change the state by directly overwriting `this.state` fields. In ohter scenarios you use `this.setState`

2. **static getDerivedStateFromProps( props, state )**

- When the state of the component depends on changes in props over time. In this scenario it sets the state.
- Do not cause any side effects

3. **Render()**

- Only required method in a class component
- Reads props & state and return JSX
- Do not change state or interact with DOM or make AJAX calls
- Children components lifecycle methods are also executed

3. **ComponentDidMount()**

- Called only once in a components lifecycle and invoked immediately after a component and its children components have been rendered to the DOM

- Prefect place to cause side effects. eg: Interact with DOM, perform AJAX calls to load data, etc

Child component:

```jsx
import React, { Component } from "react";

class LifeCycleB extends Component {
  constructor(props) {
    super(props);

    this.state = {
      name: "kenneth",
    };
    console.log("LifecycleB constructor");
  }

  static getDerivedStateFromProps(props, state) {
    console.log("LifecycleB getDerivedFromStateProps");
    return null;
  }

  componentDidMount() {
    console.log("LifecycleB componentDidMount");
  }

  render() {
    console.log("LifecycleB render");
    return <div>Lifecycle B</div>;
  }
}

export default LifeCycleB;
```

Parent component:

```jsx
import React, { Component } from "react";
import LifeCycleB from "./LifecycleB";

class LifeCycleA extends Component {
  constructor(props) {
    super(props);

    this.state = {
      name: "kenneth",
    };
    console.log("LifecycleA constructor");
  }

  static getDerivedStateFromProps(props, state) {
    console.log("LifecycleA getDerivedFromStateProps");
    return null;
  }

  componentDidMount() {
    console.log("LifecycleA componentDidMount");
  }

  render() {
    console.log("LifecycleA render");
    return (
      <div>
        <div>Lifecycle A</div>
        <LifeCycleB />
      </div>
    );
  }
}

export default LifeCycleA;

// Expected output: -

// LifecycleA constructor
// LifecycleA getDerivedFromStateProps
// LifecycleA render
// LifecycleB constructor
// LifecycleB getDerivedFromStateProps
// LifecycleB render
// LifecycleB componentDidMount
// LifecycleA componentDidMount
```

## Updating Lifecycle Methods

1. **static getDerivedStateFromProps( props, state )**

- Called everytime a component is re-rendered
- When the state of the component depends on changes in props over time. In this scenario it sets the state.
- Do not cause any side effects

2. **shouldComponentUpdate(nextProps, nextState)** - rarely used

- Dictates if the component should re-render or not (prevent the default behaviour of re-render when the component state or prop changes ). In this method you can compare the existing props and state values with the next props and state values and return true or false to let react know whether the component should update or not
- basically used for performance optimization
- Do not cause side effects

3. **render()**

- Only required method in a class component
- Reads props & state and return JSX
- Do not change state or interact with DOM or make AJAX calls
- Children components lifecycle methods are also executed

4. **getSnapshotBeforeUpdate(prevProps, prevState)** - rarely used

- Called right before the changes from the virtual DOM are to be reflected in the DOM
- Capture some information from the DOM. eg: You can read a user's scroll position and after the update to maintain that scroll position by performing some calculations
- Will either return null or return a value. Returned value will be passed as the third parameter to the next method

5. **componentDidUpdate(prevProps, prevState, snapshot)**

- Called after the render is finished in the re-render cycles. Guaranteed to be called only once in eachh re-render cycles.
- Can cause side effects

eg:-
**Child component**

```jsx
import React, { Component } from "react";

class LifeCycleB extends Component {
  constructor(props) {
    super(props);

    this.state = {
      name: "kenneth",
    };
    console.log("LifecycleB constructor");
  }

  static getDerivedStateFromProps(props, state) {
    console.log("LifecycleB getDerivedFromStateProps");
    return null;
  }

  componentDidMount() {
    console.log("LifecycleB componentDidMount");
  }

  shouldComponentUpdate() {
    console.log("LifecycleB shouldComponentUpdate");
    return true;
  }

  getSnapshotBeforeUpdate(prevProps, prevState) {
    console.log("LifecycleB getSnapshotBeforeUpdate");
    return null;
  }

  componentDidUpdate() {
    console.log("LifecycleB componentDidUpdate");
  }

  render() {
    console.log("LifecycleB render");
    return <div>Lifecycle B</div>;
  }
}

export default LifeCycleB;
```

**Parent component**

```jsx
import React, { Component } from "react";
import LifeCycleB from "./LifecycleB";

class LifeCycleA extends Component {
  constructor(props) {
    super(props);

    this.state = {
      name: "kenneth",
    };
    console.log("LifecycleA constructor");
  }

  static getDerivedStateFromProps(props, state) {
    console.log("LifecycleA getDerivedFromStateProps");
    return null;
  }

  componentDidMount() {
    console.log("LifecycleA componentDidMount");
  }

  shouldComponentUpdate() {
    console.log("LifecycleA shouldComponentUpdate");
    return true;
  }

  getSnapshotBeforeUpdate(prevProps, prevState) {
    console.log("LifecycleA getSnapshotBeforeUpdate");
    return null;
  }

  componentDidUpdate() {
    console.log("LifecycleA componentDidUpdate");
  }

  changeState = () => {
    this.setState({
      name: "Codevolution",
    });
  };

  render() {
    console.log("LifecycleA render");
    return (
      <div>
        <div>Lifecycle A</div>
        <button onClick={this.changeState}>Change state</button>
        <LifeCycleB />
      </div>
    );
  }
}

export default LifeCycleA;

// Expected output: -

// Before button click to change state

// LifecycleA constructor
// LifecycleA getDerivedFromStateProps
// LifecycleA render
// LifecycleB constructor
// LifecycleB getDerivedFromStateProps
// LifecycleB render
// LifecycleB componentDidMount
// LifecycleA componentDidMount

// After button click to change state

// LifecycleA getDerivedFromStateProps
// LifecycleA shouldComponentUpdate
// LifecycleA render
// LifecycleB getDerivedFromStateProps
// LifecycleB shouldComponentUpdate
// LifecycleB render
// LifecycleB getSnapshotBeforeUpdate
// LifecycleB componentDidUpdate
// LifecycleA componentDidUpdate
```

# Unmounting Phase Method

1. componentWillUnmount()

- Method is invoked immediately before a component is unmounted and destroyed.
- Can perform clean up tasks like cancelling any network requests, removing event handlers, cancelling any subsriptions and also invalidating timers
- Do not call the setState method because the component is never re-rendered after it has been unmounted

# Error Handling Phase Methods

Two methods are:-
**static `getDerivedStateFromError(error)` and `componentDidCatch(error, info)`** - Called when there is an error during rendering, in a lifecycle method, or in the constructor of any child component.

Runtime errors during rendering could put our application in a broken state. There react basically unmounts the whole react component tree. To catch errors anywhere in the component tree and display a fallback UI these **error boundaries** takes the spotlight.

**Error boundary** - A class component that implements either one or both of the lifecycle methods `getDerivedStateFromError` or `componentDidCatch` becomes an **error boundary**.

- The static method `getDerivedStateFromError` method is used to render a fallback UI after an error is thrown and the `componentDidCatch` method is used to log the error information.

**ErrorBoundary.js**

```js
import React, { Component } from "react";

class ErrorBoundary extends Component {
  constructor(props) {
    super(props);

    this.state = {
      hasError: false,
    };
  }

  static getDerivedStateFromError(error) {
    return {
      hasError: true,
    };
  }
  render() {
    if (this.state.hasError) {
      return <h1>Something went wrong</h1>;
    }
    return this.props.children;
  }
}

export default ErrorBoundary;
```

**A component that migh cause error**

```js
import React from "react";

function Hero({ heroName }) {
  if (heroName === "Joker") {
    throw new Error("Not a hero!");
  }
  return <div>{heroName}</div>;
}

export default Hero;
```

**App.js**

```jsx
import React from "react";
import "./App.css";
import Hero from "./Hero";
import ErrorBoundary from "./ErrorBoundary";

function App() {
  return (
    // By wrapping it on induvidual component the error UI is only shown if there is an error on that component
    <React.Fragment>
      <ErrorBoundary>
        <Hero heroName={"Batman"} />
      </ErrorBoundary>
      <ErrorBoundary>
        <Hero heroName={"Superman"} />
      </ErrorBoundary>
      <ErrorBoundary>
        <Hero heroName={"Joker"} /> {/* This doesn't cause an error */}
      </ErrorBoundary>
    </React.Fragment>
  );
}

export default App;
```

`componentDidCatch()` accepts two parameters (error, info). With it we can log the information related that error to the console if it catches an error.

**Note**
These methods do not catch errors inside eventHandlers. Use regular `try...catch` for eventHandlers

# React Fragments

React fragment lets you wrap the elements. You can use this instead of div as react can only return single element which enclose the child elements. Used with <React.Fragment></React.Fragment> or <></>. With <React.Fragment> you can only pass key and children props inside fragments.

# Pure components

A Pure Component in React is a class component that implements `shouldComponentUpdate()` with a shallow comparison of props and state. It helps optimize rendering by preventing unnecessary re-renders if the component's input props and state haven't changed. That is **pure component is only re-rendered if the shallow comparison indicates that there is a difference between shallow comparison of prevState or prevProp with the currentState or currentProp**. Helps with performance by preventing unnecessary re-renders.

```jsx
import React, { PureComponent } from "react";

class Person extends PureComponent {
  render() {
    console.log("Person component rendered");
    const { name, age } = this.props;
    return (
      <div>
        <h1>{name}</h1>
        <p>Age: {age}</p>
      </div>
    );
  }
}

export default Person;
```

## Regular component vs Pure component

- A regular component does not implement `shouldComponentUpdate()` method and always return true by default while a pure component on the other hand implement `shouldComponentUpdate()` with a shallow props and state comparison.

# HOC (Higher Order Component)

A **Higher-Order Component (HOC)** in React is a function that takes a component as input and returns a new component with enhanced functionality. It's a design pattern for reusing component logic across multiple components.

Higher-Order Component shares common functionality between components

`const EnhancedComponent = higherOrderComponent(originalComponent)`

**Simple implemntation example**

```js
function withExtraProps(WrappedComponent) {
  return function EnhancedComponent(props) {
    return <WrappedComponent {...props} newProp="extra data" />;
  };
}
```

**Example:**

**With class components:**

HOC counter class:-

```js
import React, { Component } from "react";

const withCounter = (WrappedComponent) => {
  class NewComponent extends Component {
    constructor(props) {
      super(props);

      this.state = {
        count: 0,
      };
    }

    incrementCount = () => {
      this.setState((prevState) => {
        return { count: prevState.count + 1 };
      });
    };

    render() {
      return (
        <WrappedComponent
          count={this.state.count}
          incrementCount={this.incrementCount}
          {...this.props} {/* Passing remaining props if any in the parent as props passed from parent to other component which is wrapped by this HOC is passed to HOC itself */}
        />
      );
    }
  }
  return NewComponent;
};

export default withCounter;
```

Note: You can use different outputs based on the child from the HOC by passing another parameter other to the HOC. eg: `const withCounter = (WrappedComponent, increaseNum) => {...this.setState({count +=  increaseNum})`

```js
import React, { Component } from "react";
import withCounter from "./withCounter";

export class ClickCounter extends Component {
  render() {
    const { count, incrementCount } = this.props;
    return <button onClick={incrementCount}>Clicked {count} times</button>;
  }
}

export default withCounter(ClickCounter);
```

**With functional components:**

```js
function Greeting({ name }) {
  return <h1>Hello, {name}!</h1>;
}

// Higher-Order Component to add a "default" name
function withDefaultName(WrappedComponent) {
  return function EnhancedComponent(props) {
    return <WrappedComponent {...props} name="Guest" />;
  };
}

const EnhancedGreeting = withDefaultName(Greeting);

function App() {
  return <EnhancedGreeting />;
}
```

**Note: Render Props** is another pattern we use to share functionality between components. We can do that by sharing code between React components using a **prop whose value is a function**

# MEMO

We could achieve the same functionality as pure component which is only for class based component in functional component by thy use of `React.memo`.

`React.memo` is a higher-order component (HOC) in React that is used to optimize functional components by preventing unnecessary re-renders. It works similarly to `PureComponent` but for functional components. When you wrap a component in `React.memo`, React will only re-render that component if its props change, which can improve performance in some cases.

**How `React.memo` Works:**

- `React.memo` performs a shallow comparison of the component’s props.
- If the props remain the same (by shallow comparison), React will skip the rendering of the component.
- If the props change, the component will be re-rendered.

```js
const MemoizedComponent = React.memo(Component);
```

**In jsx:**

```jsx
export default React.memo(function MyComponent(props) {
  return <div>{props.value}</div>;
});
```

# REFS

**Refs** make it possible to access DOM node directly within React without relying on state or props. They provide a way to interact with elements, like focusing input fields, managing animations, or triggering imperative DOM updates.

```jsx
import React, { Component } from "react";

class RefsDemo extends Component {
  constructor(props) {
    super(props);
    this.inputRef = React.createRef();
    this.heading = React.createRef();
  }

  componentDidMount() {
    this.inputRef.current.focus();
    console.log(this.inputRef);

    console.log(this.heading);
  }

  clickHandler = () => {
    alert(this.inputRef.current.value);
  };

  render() {
    return (
      <div>
        <input type="text" ref={this.inputRef} />
        <h1 ref={this.heading}>Heading</h1>
        <button onClick={this.clickHandler}>Capture Input</button>
      </div>
    );
  }
}

export default RefsDemo;
```

An example with old way of using ref with callback

```jsx
import React, { Component } from "react";

class RefsDemo extends Component {
  constructor(props) {
    super(props);
    this.cbRef = null;
    this.setCbRef = (elem) => {
      this.cbRef = elem;
    };
  }

  componentDidMount() {
    // React will call the ref callback with the callback with the DOM element when the component mounts and call it with null when the component unmounts
    if (this.cbRef) {
      this.cbRef.focus();
    }
  }

  render() {
    return (
      <div>
        <input type="text" ref={this.setCbRef} />
      </div>
    );
  }
}

export default RefsDemo;
```

## Forwarding Refs

Normally, refs only work within the component where they are defined. Forwarding refs enables a parent component to access a child’s DOM node (e.g., for focusing an input or scrolling).

**Forwarding Refs** in React allows parent components to pass a ref to a child component, enabling direct access to a child’s DOM element or a component instance.

You use `React.forwardRef` to forward a ref from a parent to a child component.

### Example with functional components

```jsx
const MyInput = React.forwardRef(
  (
    props,
    ref // ref is the second parameter
  ) => <input ref={ref} {...props} />
);

function ParentComponent() {
  const inputRef = React.createRef();

  const focusInput = () => {
    inputRef.current.focus();
  };

  return (
    <div>
      <MyInput ref={inputRef} />
      <button onClick={focusInput}>Focus Input</button>
    </div>
  );
}
```

# React Portals

React portals provide a way to render children into a DOM node that exists outside the DOM heirarchy of the parent component. You can render component onto a DOM node that is not this root element.

In the **index.html** file with the root element add another element div with an id (eg: id="portal-root")

example:

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>React</title>
  </head>
  <body>
    <div id="root"></div>
    <div id="protal-root"></div>
    <script type="module" src="/src/main.jsx"></script>
  </body>
</html>
```

And afterwards create a component that should fall under the new element (...id="portal-root") with `ReactDOM.createPortal(element to render, element to be rendered in - portal-dom)`

```jsx
import React from "react";
import ReactDOM from "react-dom";

function PortalDemo() {
  return ReactDOM.createPortal(
    <h1>Portals Demo</h1>,
    document.getElementById("portal-root")
  );
}

export default PortalDemo;
```

## Why use React Portals?

1. **Breaking Out of the DOM Hierarchy**: Sometimes, components need to be rendered outside the parent container's DOM structure, but still maintain their functionality and event behavior. For example, a modal might need to appear over the rest of the page content, regardless of its parent’s overflow settings.

2. **Handling Styling Issues**: Elements like modals or pop-ups might have issues with positioning or visibility if they are confined to the boundaries of a parent container. Portals allow them to be rendered at the root level, avoiding these issues.

3. **Event Bubbling**: Even though the portal is rendered outside the parent DOM, it maintains event bubbling with the parent. So, events like clicks and form submissions will behave as if the component is still within the parent component’s tree.

# HTTP and React

React is a library for building user interfaces so it is in no way concerened about HTTP. So how to make AJAX requests in react or API calls in React?

React itself does have a particular way to fetch or send a data to server, so we will have to make use of HTTP library. One of the popular library is **Axios**

**Fetch api** is also a good consideration as it is more standardized

```jsx
import React, { Component } from "react";
import axios from "axios";

export class PostList extends Component {
  constructor(props) {
    super(props);

    this.state = {
      posts: [],
      errorMsg: "",
    };
  }
  componentDidMount() {
    axios
      .get("https://jsonplaceholder.typicode.com/posts")
      .then((response) => {
        console.log(response);
        this.setState({
          posts: response.data,
        });
      })
      .catch((error) => {
        console.log(error);
        this.setState({ errorMsg: "Error retrieveing data" });
      });
  }
  render() {
    const { posts, errorMsg } = this.state;
    return (
      <div>
        <h1>List of posts:</h1>
        {posts.length ? posts.map((post) => <div>{post.title}</div>) : null}
        {errorMsg && <h2>{errorMsg}</h2>}
      </div>
    );
  }
}

export default PostList;
```

# HOOKS

Hooks are a new feature addition in React version 16.8. Hooks allow you to use React features without having to write a class. Hooks don't work inside classes.

# Why Hooks

- You had to understand how this keyword works in JS
- Remember to bind event handlers in class components
- Classes don't minify very well and make hot reloading very unreliable.

# Rules of Hooks

- **Only Call Hooks at the Top Level**

Don't call Hooks inside loops, conditions, or nested functions

- **Only Call Hooks from React Functions**

Call them from within React functional components and not just any regular JS functions

# FROM HERE ON IT MAY ONLY BE HEADING AND SMALL IMPORTANT NOTES

# 'useState' Hook

`const [state, setState] = useState(inititalStateValue);`

## Summary - useState

- The useState hook lets you add state to functional components
- In classes, the state is always an object.
- With the useState hook, the state doesn't have to be an object.
- The useState hook retrns an array with 2 elements.
- The first element is the current value of the state, and the second element is a state setter function
- New state value depends on the previous state value, and the second element is a state setter function
- When dealing with objects or arrays, always make sure to spread your state varialbe and then call the setter function

# 'useEffect' Hook

The `useEffect` Hook lets you perform side effects in functional components. It is a close replacement for `componentDidMount`, `componentDidUpdate` and `componentWillUnmount`.

`useEffect(functionThatGetsExecutedAfterEveryRender, [dependencyArray-ValuesTheEffectDependsOn])`

You can return a function that executes a code to unmount possible memory leaks (eg: event listeners )

Note - if you think dependecy array is a way to specify when you want to rerun the effect you're going to run into problems. Instead dependency array should be thought of as a way to let react know about everything that the effect must watch for changes

# 'useContext' Hook

Context provides a way to pass data through the component tree without having to pass props down manually at every level.

`const context = React.createContext()`

# 'useReducer' Hook

useReducer is a hook that is used for state management. It is an alternative to useState. useState is built using useReducer.

useReducer is like reduce function for arrays in JS.

`useReducer(reducerFunction, initialState)`

Here the reducer function accepts two values currentState and action and returns a newState.
The return value is a pair of values - newState and a dispatch method which is basically used to specify the action

eg:

```jsx
import Reac, { useReducer } from "react";

const inititalState = 0;
const reducer = (state, action) => {
  switch (action) {
    case "increment":
      return state + 1;
    case "decrement":
      return state - 1;
    case "reset":
      return inititalState;
    default:
      return state;
  }
};

function CounterOne() {
  const [count, dispatch] = useReducer(reducer, inititalState);
  return (
    <div>
      <div>Count - {count}</div>
      <button onClick={() => dispatch("increment")}>Increment</button>
      <button onClick={() => dispatch("decrement")}>Decrement</button>
      <button onClick={() => dispatch("reset")}>Reset</button>
    </div>
  );
}

export default CounterOne;
```

# React Router

React Router is a fully-featured client and server side routing library for React. It helps create and navigate between different URLs that make up your web application

- router, route, routes
- Links
- NavLinks
- const navigate = useNavigate(), navigate('path'), navigate(-1), navigate('path', {replace: true})
- Outlet
- No match route
- Index route
- Dynamic routes
- useParams
- useSearchParams (works like useState)
- Relative links - Link that does not start with a forward slash and will inherit the closest route in whhich they are rendered
- Lazy loading - A technique where components not required on the home page can be split into separate code bundles and download it only when the user navigates to that page. Think of it as incremently downloading the application. Reduces initital load time thereby improving performance

Eg:

```jsx
const LazyPage = React.lazy(() => import('./page'));
...
<Route
  path='page'
  element={
    <React.Suspense fallback="<h1>Loading..</h1>">
      <LazyPage />
    </React.Suspense>
  }
/>

```
