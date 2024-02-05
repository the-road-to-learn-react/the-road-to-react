## React Component Composition

Essentially a React application is a bunch of React components arranged in the shape of a tree. When you learned about initializing components as elements in JSX, you have seen how they are used like any other HTML element in JSX. However, until now we have only used them as self-closing tags. What if there could be an opening and closing tag instead for React elements too? Entering the concept of component composition:

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
        onInputChange={handleSearch}
# leanpub-start-insert
      >
        Search:
      </InputWithLabel>
# leanpub-end-insert

      ...
    </div>
  );
};
~~~~~~~

Component composition is one of React's more powerful features. Essentially we'll discover how to use a React element in the same fashion as an HTML element by leveraging its opening and closing tag. In the previous example, instead of using the `label` prop from before, we inserted the text "Search:" between the component's element's tags. In the InputWithLabel component, you have access to this information via **React's children** prop now. Instead of using the `label` prop, use the `children` prop to render everything that has been rendered in between the `<InputWithLabel>` opening and closing tag:

{title="src/App.jsx",lang="javascript"}
~~~~~~~
const InputWithLabel = ({
  id,
  value,
  type = 'text',
  onInputChange,
# leanpub-start-insert
  children,
# leanpub-end-insert
}) => (
  <>
# leanpub-start-insert
    <label htmlFor={id}>{children}</label>
# leanpub-end-insert
    &nbsp;
    <input
      id={id}
      type={type}
      value={value}
      onChange={onInputChange}
    />
  </>
);
~~~~~~~

Now the React component's elements behave similarly to native HTML. Everything that's passed between a component's elements can be accessed as `children` in the component and be rendered. Sometimes when using a React component, you want to have more freedom from the outside regarding what to render on the inside of a component:

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
        onInputChange={handleSearch}
      >
# leanpub-start-insert
        <strong>Search:</strong>
# leanpub-end-insert
      </InputWithLabel>

      ...
    </div>
  );
};
~~~~~~~

With the React children prop, we can compose React components into each other. We've used it with a string and with a string wrapped in an HTML `<strong>` element, but it doesn't end here. You can pass React elements via React children as well -- which you should definitely explore more as an exercise.

### Exercises:

* Compare your source code against the author's [source code](https://bit.ly/492oks5).
  * Recap all the [source code changes from this section](https://bit.ly/3u4KkDB).
  * Optional: If you are using TypeScript, check out the author's source code [here](https://bit.ly/3uvSg0S).
* Read more about [Component Composition in React](https://www.robinwieruch.de/react-component-composition/).
* Optional: [Leave feedback for this section](https://forms.gle/L2GgfHVjAAwbqudq8).

### Interview Questions:

* Question: What does "children" refer to in React components?
  * Answer: "Children" in React refers to the content placed between the opening and closing tags of a component.
* Question: How can you access and render the "children" of a React component?
  * Answer: Use the `props.children` property to access and render the content placed within a component.
* Question: Can a React component have multiple children?
  * Answer: Yes, but would use attributes instead, like `<MyComponent slotOne={<em>1</em>} slotTwo={<em>2</em>} />` which can then be accessed via `props.slotOne` and `props.slotTwo` in the component.
* Question: Can you pass React components as "children" to another component?
  * Answer: Yes, React components can be passed as "children" to other components, allowing for composability.
* Question: What is the purpose of the React.Children utility in React?
  * Answer: The React.Children utility provides methods for working with and manipulating the "children" of a React component.
* Question: How do you iterate over and manipulate each child in a React component?
  * Answer: Use React.Children.map or React.Children.forEach to iterate over and perform operations on each child of a component.
* Question: Can you have a component without any "children" in React?
  * Answer: Yes, a component can exist without any "children" by not placing content between its opening and closing tags.
* Question: What is the difference between "children" and other props in React?
  * Answer: "Children" refers specifically to the content between tags, while other props are key-value pairs passed to a component.
* Question: How do you check if a React component has "children"?
  * Answer: Use the React.Children.count or React.Children.toArray methods to check if a component has "children."