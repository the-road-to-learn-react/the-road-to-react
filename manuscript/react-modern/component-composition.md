## React Component Composition

Now we'll discover how to use a React element in the same fashion as an HTML element, with an opening and closing tag:

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
        Search
      </InputWithLabel>
# leanpub-end-insert

      ...
    </div>
  );
};
~~~~~~~

Instead of using the `label` prop from before, we inserted the text "Search:" between the component's element's tags. In the InputWithLabel component, you have access to this information via **React's children** prop. Instead of using the`label` prop, use the children`prop to render everything that has been passed down from above where you want it:

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

Now the React component's elements behave similar to native HTML. Everything that's passed between a component's elements can be accessed as children`in the component and be rendered somewhere. Sometimes when using a React component, you want to have more freedom from the outside what to render in the inside of a component:

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

With this React feature, we can compose React components into each other. We've used it with a JavaScript string and with a string wrapped in an HTML `<strong>` element, but it doesn't end here. You can pass components via React children as well.

### Exercises:

* Confirm your [source code for the last section](https://codesandbox.io/s/github/the-road-to-learn-react/hacker-stories/tree/hs/React-Component-Composition).
  * Confirm the [changes from the last section](https://github.com/the-road-to-learn-react/hacker-stories/compare/hs/Reusable-React-Component...hs/React-Component-Composition?expand=1).
* Read more about React Component Composition ([0](https://www.robinwieruch.de/react-component-composition), [1](https://reactjs.org/docs/composition-vs-inheritance.html)).
* Create a simple text component that renders a string and passes it as `children` to the InputWithLabel component.