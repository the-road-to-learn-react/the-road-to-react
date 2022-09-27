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

Component composition is one of React's more powerful features. Essentially we'll discover how to use a React element in the same fashion as an HTML element by leveraging its opening and closing tag.

In the previous example, instead of using the `label` prop from before, we inserted the text "Search:" between the component's element's tags. In the InputWithLabel component, you have access to this information via **React's children** prop now. Instead of using the `label` prop, use the `children` prop to render everything that has been rendered in between the `<InputWithLabel>` opening and closing tag:

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

Now the React component's elements behave similarly to native HTML. Everything that's passed between a component's elements can be accessed as `children` in the component and be rendered somewhere. Sometimes when using a React component, you want to have more freedom from the outside regarding what to render on the inside of a component:

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

* Compare your source code against the author's [source code](https://bit.ly/3qTV3fb).
  * Recap all the [source code changes from this section](https://bit.ly/3DKLDu6).
  * Optional: If you are using TypeScript, check out the author's source code [here](https://bit.ly/3Eebr23).
* Read more about [Component Composition in React](https://www.robinwieruch.de/react-component-composition/).
* Optional: [Leave feedback for this section](https://forms.gle/L2GgfHVjAAwbqudq8).