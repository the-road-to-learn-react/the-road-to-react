## Imperative React

Imperative programming involves providing explicit step-by-step instructions, detailing how a program should perform a task. In contrast, declarative programming focuses on specifying the desired outcome without specifying every procedural step. Declarative code is often considered more concise, readable, and maintainable.

React uses a declarative programming approach. Instead of manually manipulating the Document Object Model (DOM) for UI updates, developers declare the desired UI state, and React manages the rendering process. This declarative approach enhances code readability and scalability, abstracting away the complexities of DOM manipulation and enabling the creation of dynamic user interfaces with a higher level of abstraction.

When you implement JSX, you tell React what elements you want to see, not how to create these elements. When you implement a hook for state, you tell React what you want to manage as a stateful value and not how to manage it. And when you implement an event handler, you do not have to assign a listener imperatively:

{title="Code Playground",lang="javascript"}
~~~~~~~
// imperative JavaScript + DOM API
element.addEventListener('click', () => {
  // do something
});

// declarative React
const App = () => {
  const handleClick = () => {
    // do something
  };

  return (
    <button
      type="button"
      onClick={handleClick}
    >
      Click
    </button>
  );
};
~~~~~~~

However, there are cases when we will not want everything to be declarative. For example, sometimes you want to have imperative access to rendered elements, most often as a side-effect, in cases such as these:

* read/write access to elements via the DOM API:
  * reading (here: measuring) an element's width or height
  * writing (here: setting) an input field's focus state
* implementation of more complex animations:
  * setting transitions
  * orchestrating transitions
* integration of third-party libraries:
  * [D3](https://d3js.org) is a popular imperative chart library

Due to the verbosity and counterintuitive nature of imperative programming in React, we will only explore a brief example illustrating how to imperatively set the focus of an input field. Conversely, for the declarative approach, you can achieve the same outcome by setting the `autoFocus` attribute of the input field:

{title="src/App.jsx",lang="javascript"}
~~~~~~~
const InputWithLabel = ({ ... }) => (
  <>
    <label htmlFor={id}>{children}</label>
    &nbsp;
    <input
      id={id}
      type={type}
      value={value}
# leanpub-start-insert
      autoFocus
# leanpub-end-insert
      onChange={onInputChange}
    />
  </>
);

# leanpub-start-insert
// note that `autoFocus` is a shorthand for `autoFocus={true}`
// every attribute that is set to `true` can use this shorthand
# leanpub-end-insert
~~~~~~~

This works, but only if one of the reusable components is rendered. For example, if the App component renders two InputWithLabel components, only the last rendered component receives the `autoFocus` flag on its render. However, since we have a reusable React component here, we can pass a dedicated prop which lets the developer decide whether the input field should have an active `autoFocus`:

{title="src/App.jsx",lang="javascript"}
~~~~~~~
const App = () => {
  ...

  return (
    <div>
      <h1>My Hacker Stories</h1>

      <InputWithLabel
        id="search"
        value={searchTerm}
# leanpub-start-insert
        isFocused
# leanpub-end-insert
        onInputChange={handleSearch}
      >
        <strong>Search:</strong>
      </InputWithLabel>

      ...
    </div>
  );
};
~~~~~~~

Again, using just `isFocused` as an attribute is equivalent to `isFocused={true}`. Within the component, use the new prop for the input field's `autoFocus` attribute:

{title="src/App.jsx",lang="javascript"}
~~~~~~~
const InputWithLabel = ({
  id,
  value,
  type = 'text',
  onInputChange,
# leanpub-start-insert
  isFocused,
# leanpub-end-insert
  children,
}) => (
  <>
    <label htmlFor={id}>{children}</label>
    &nbsp;
    <input
      id={id}
      type={type}
      value={value}
# leanpub-start-insert
      autoFocus={isFocused}
# leanpub-end-insert
      onChange={onInputChange}
    />
  </>
);
~~~~~~~

The feature works, yet it's a declarative implementation. We are telling React *what* to do and not *how* to do it. Even though it's possible to do it with the declarative approach (which is the recommended way), let's refactor this scenario to an imperative approach. We want to execute the `focus()` method programmatically on the input field's element via the DOM API once it has been rendered:

{title="src/App.jsx",lang="javascript"}
~~~~~~~
const InputWithLabel = ({
  id,
  value,
  type = 'text',
  onInputChange,
  isFocused,
  children,
}) => {
# leanpub-start-insert
  // A
  const inputRef = React.useRef();
# leanpub-end-insert

# leanpub-start-insert
  // C
  React.useEffect(() => {
    if (isFocused && inputRef.current) {
      // D
      inputRef.current.focus();
    }
  }, [isFocused]);
# leanpub-end-insert

  return (
    <>
      <label htmlFor={id}>{children}</label>
      &nbsp;
# leanpub-start-insert
       {/* B */}
# leanpub-end-insert
      <input
# leanpub-start-insert
        ref={inputRef}
# leanpub-end-insert
        id={id}
        type={type}
        value={value}
        onChange={onInputChange}
      />
    </>
  );
};
~~~~~~~

All the essential steps are marked with comments that are explained step by step:

* (A) First, create a `ref` with **React's useRef Hook**. This `ref` object is a persistent value which stays intact over the lifetime of a React component. It comes with a property called `current`, which, in contrast to the `ref` object, can be changed.
* (B) Second, the `ref` is passed to the element's JSX-reserved `ref` attribute and thus element instance gets assigned to the changeable `current` property.
* (C) Third, opt into React's lifecycle with React's useEffect Hook, performing the focus on the element when the component renders (or its dependencies change).
* (D) And fourth, since the `ref` is passed to the element's `ref` attribute, its `current` property gives access to the element. Execute its focus programmatically as a side-effect, but only if `isFocused` is set and the `current` property is existent.

Essentially that's the whole example of how to move from declarative to imperative programming in React. In this case, it's possible to use either the declarative or imperative approach as you experienced first hand. However, it's not always possible to use the declarative approach, so the imperative approach can be performed whenever it's necessary. Since we didn't cover `ref` and `useRef` in much detail here, because it is a more rarely used feature in React, I suggest reading the additional article from the exercises for a more in-depth understanding.

### Exercises:

* Compare your source code against the author's [source code](https://tinyurl.com/4hz7a5rv).
  * Recap all the [source code changes](https://tinyurl.com/y8s3esu2) from this section.
  * Optional: If you are using TypeScript, check out the author's source code [here](https://bit.ly/494zUDw).
* Read more about [refs in React](https://www.robinwieruch.de/react-ref/) and optionally check out the following tutorials which are using refs:
  * [Create an image from a React component with a ref](https://www.robinwieruch.de/react-component-to-image/)
  * [Create a Slider component with a ref](https://www.robinwieruch.de/react-slider/)
  * [Create a custom hook with a ref](https://www.robinwieruch.de/react-custom-hook-check-if-overflow/)
* Read more about [why frameworks matter](https://www.robinwieruch.de/why-frameworks-matter/).

### Interview Questions:

* Question: What is useRef in React?
  * Answer: useRef is a hook in React that provides a mutable object called a ref, which can hold a mutable value and persists across renders.
* Question: How is useRef different from useState in React?
  * Answer: Unlike useState, useRef doesn't trigger a re-render when its value changes. It's often used for mutable values that don't affect the rendering.
* Question: Can useRef be used to hold a mutable value that persists across renders?
  * Answer: Yes, the primary purpose of useRef is to hold mutable values that persist across renders without causing re-renders.
* Question: What is the common use case for useRef in React?
  * Answer: A common use case is accessing and interacting with the DOM, as useRef can hold a reference to a DOM element.
* Question: Can useRef be used to trigger re-renders in React?
  * Answer: No, changing the value of a ref created with useRef does not trigger a re-render.
* Question: Can useRef be used to persist values between function calls?
  * Answer: Yes, useRef values persist across renders, making them suitable for persisting values between function calls without triggering re-renders.
* Question: How can you access the current value of a ref created with useRef?
  * Answer: Use myRef.current to access the current value of a ref created with useRef.