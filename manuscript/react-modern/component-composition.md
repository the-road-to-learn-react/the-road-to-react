## React Component Composition

The concept of component composition is one of React's more powerful features. Essentially we'll discover how to use a React element in the same fashion as an HTML element, with an opening and closing tag:

{title="src/App.js",lang="javascript"}
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

Instead of using the `label` prop from before, we inserted the text "Search:" between the component's element's tags. In the InputWithLabel component, you have access to this information via **React's children** prop now. Instead of using the `label` prop, use the `children` prop to render everything that has been passed down from above where you want it:

{title="src/App.js",lang="javascript"}
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

{title="src/App.js",lang="javascript"}
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

With this React feature, we can compose React components into each other. We've used it with a JavaScript string and with a string wrapped in an HTML `<strong>` element, but it doesn't end here. You can pass components via React children as well; which you should definitely explore more as an exercise.

### Exercises:

* Confirm your [source code](https://bit.ly/3lUZqVA).
  * Confirm the [changes](https://bit.ly/3BU66J3).
* Read more about [Component Composition in React](https://www.robinwieruch.de/react-component-composition).
* Optional: [Leave feedback for this section](https://forms.gle/L2GgfHVjAAwbqudq8).