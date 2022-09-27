## Reusable React Component

Have a closer look at the Search component: Every implementation detail is tied to the search feature. However, internally the component is only a label and an input, so why should it be tied so strictly to one domain? Being tied to one feature makes a component less reusable for the rest of the application. In this case, the Search component cannot be used for non-search-related tasks.

In addition, the Search component risks introducing bugs, because if two of these Search components are rendered on the same page, their `htmlFor`/`id` combination is duplicated and therefore breaking the focus when one of the labels is clicked by the user. Let's fix these underlying issues by making the Search component reusable.

Since the Search component doesn't have any actual "search" functionality, it takes little effort to generalize the search domain-specific properties to make the component reusable for the rest of the application. Let's pass a dynamic `id` and `label` prop to the Search component, rename the actual value and callback handler to something more generic, and rename the component too:

{title="src/App.jsx",lang="javascript"}
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

It's fully reusable, but only when using an input with a text. If we would want to support numbers (`number`) or phone numbers (`tel`) too, the `type` attribute of the input field needs to be accessible from the outside too:

{title="src/App.jsx",lang="javascript"}
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

Because we don't pass a `type` prop from the App component to the InputWithLabel component, the [default parameter](https://mzl.la/3aUefkN) from the function signature takes over for the type. Thus, every time the InputWithLabel component is used without a `type` prop, the default type will be `"text"`.

With just a few changes we turned a specialized Search component into a more reusable InputWithLabel component. We generalized the naming of the internal implementation details and gave the new component a larger API surface to provide all the necessary information from the outside. We aren't using the component elsewhere yet, but we increased its ability to handle the task if we do.

It's always a trade-off between generalization and specialization of components. In this case, we turned a highly specialized component into a generalized component. While a generalized component has a better chance of getting reused in the application, a specialized component would implement business logic for one specific use case and therefore isn't reusable at all.

### Exercises:

* Compare your source code against the author's [source code](https://bit.ly/3dCtfJe).
  * Recap all the [source code changes from this section](https://bit.ly/3S9O8KH).
  * Optional: If you are using TypeScript, check out the author's source code [here](https://bit.ly/3Ce3312).
* Read more about [Reusable React Components](https://www.robinwieruch.de/react-reusable-components/) and build some of these components yourself:
  * [Button in React](https://www.robinwieruch.de/react-button/)
  * [Radio Button in React](https://www.robinwieruch.de/react-radio-button/)
  * [Checkbox in React](https://www.robinwieruch.de/react-checkbox/)
  * [Dropdown in React](https://www.robinwieruch.de/react-dropdown/)
* Before we used the text "Search:" with a ":". How would you deal with it now? Would you pass it with `label="Search:"` as prop to the InputWithLabel component or hardcode it after the `<label htmlFor={id}>{label}:</label>` usage in the InputWithLabel component? We will see how to cope with this later.
* Optional: [Leave feedback for this section](https://forms.gle/76C3LvW3kHHwdhgq5).