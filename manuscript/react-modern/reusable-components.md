## Reusable React Component

Have a closer look at the Search component. The label element has the text "Search: "; the id/htmlFor attributes have the `search` identifier; the value is called `search`, and the callback handler is called `onSearch`. The component is very much tied to the search feature, which makes it less reusable for the rest of the application and non-search-related tasks which would need the same label and input field. Also, it risks introducing bugs if two of these Search components are rendered side by side, because the htmlFor/id combination is duplicated, breaking the focus when one of the labels is clicked by the user. Let's fix these underlying issues by making the Search component reusable.

Since the Search component doesn't have any actual "search" functionality, it takes little effort to generalize the search domain-specific properties to make the component reusable for the rest of the application. Let's pass a dynamic `id` and `label` prop to the Search component, rename the actual value and callback handler to something more generic, and rename the component accordingly:

{title="src/App.js",lang="javascript"}
~~~~~~~
const App = () => {
  ...

  return (
    <div>
      <h1>My Hacker Stories</h1>

# leanpub-start-insert
      <InputWithLabel
        id="search"
        label="Search"
        value={searchTerm}
        onInputChange={handleSearch}
# leanpub-end-insert
      />

      ...
    </div>
  );
};

# leanpub-start-insert
const InputWithLabel = ({ id, label, value, onInputChange }) => (
# leanpub-end-insert
  <>
# leanpub-start-insert
    <label htmlFor={id}>{label}</label>
    &nbsp;
# leanpub-end-insert
    <input
# leanpub-start-insert
      id={id}
# leanpub-end-insert
      type="text"
# leanpub-start-insert
      value={value}
      onChange={onInputChange}
# leanpub-end-insert
    />
  </>
);
~~~~~~~

It's not fully reusable yet. If we want an input field for data like a number (`number`) or phone number (`tel`), the `type` attribute of the input field needs to be accessible from the outside too:

{title="src/App.js",lang="javascript"}
~~~~~~~
const InputWithLabel = ({
  id,
  label,
  value,
# leanpub-start-insert
  type = 'text',
# leanpub-end-insert
  onInputChange,
}) => (
  <>
    <label htmlFor={id}>{label}</label>
    &nbsp;
    <input
      id={id}
# leanpub-start-insert
      type={type}
# leanpub-end-insert
      value={value}
      onChange={onInputChange}
    />
  </>
);
~~~~~~~

Because we don't pass a `type` prop from the App component to the InputWithLabel component, the [default parameter](https://mzl.la/3aUefkN) from the function signature takes over for the type. Thus, every time the InputWithLabel component is used without a `type` prop, the default type will be `text`.

In conclusion, with just a few changes we turned a specialized Search component into a more reusable component. We generalized the naming of the internal implementation details and gave the new component a larger API surface to provide all the necessary information from the outside. We aren't using the component elsewhere, but we increased its ability to handle the task if we do.

### Exercises:

* Confirm your [source code](https://bit.ly/3B0roTU).
  * Confirm the [changes](https://bit.ly/3C2DzAY).
* Read more about [Reusable React Components](https://www.robinwieruch.de/react-reusable-components).
* Before we used the text "Search:" with a ":". How would you deal with it now? Would you pass it with `label="Search:"` as prop to the InputWithLabel component or hardcode it after the `<label htmlFor={id}>{label}:</label>` usage in the InputWithLabel component? We will see how to cope with this later.
* Optional: [Leave feedback for this section](https://forms.gle/76C3LvW3kHHwdhgq5).