## Reusable React Component

Examine the Search component more closely: Every intricate detail of its implementation is closely linked to the search feature. However, internally, the component is composed of merely a label and an input. Why should it be so tightly bound to a singular domain? This narrow association makes the component less adaptable for other functionalities within the application. Consequently, the Search component becomes impractical for tasks unrelated to searching.

Moreover, the Search component poses a risk of introducing bugs. If multiple instances of this Search component are rendered on the same page, their `htmlFor`/`id` combination is duplicated. This duplication disrupts the focus when a user clicks on one of the labels. To rectify these issues, let's enhance the Search component's reusability.

Given that the Search component lacks actual "search" functionality, making it reusable for various application features involves minimal effort. We can achieve this by introducing dynamic `id` and `label` props to the Search component, renaming the specific value and callback handler to more generic terms, and consequently, renaming the component itself:

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

While it is entirely reusable, its applicability is limited to using an input with `text`. To broaden its scope and support additional input types, such as numbers (`number`) or phone numbers (`tel`), the `type` attribute of the input field needs to be accessible from the outside too:

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

* Compare your source code against the author's [source code](https://tinyurl.com/2y5xk95h).
  * Recap all the [source code changes](https://tinyurl.com/5n6zzbys) from this section.
  * Optional: If you are using TypeScript, check out the author's source code [here](https://bit.ly/3Oun6hn).
* Read more about [Reusable React Components](https://www.robinwieruch.de/react-reusable-components/) and create some of these components yourself:
  * [Button in React](https://www.robinwieruch.de/react-button/), [Radio Button in React](https://www.robinwieruch.de/react-radio-button/), [Checkbox in React](https://www.robinwieruch.de/react-checkbox/), [Dropdown in React](https://www.robinwieruch.de/react-dropdown/), [Drag-and-Drop List in React (Advanced)](https://www.robinwieruch.de/react-drag-and-drop/), ...
* Before we used the text "Search:" with a ":". How would you deal with it now? Would you pass it with `label="Search:"` as prop to the InputWithLabel component or hardcode it after the `<label htmlFor={id}>{label}:</label>` usage in the InputWithLabel component? We will see how to cope with this later.

### Interview Questions:

* Question: Why is reusability important in React?
  * Answer: Reusability promotes code efficiency, maintainability, and consistency by allowing components to be used across various parts of an application.
* Question: How can you make a React component reusable?
  * Answer: Make components more generic by using props for customizable behavior and ensuring they are not tightly coupled to specific functionalities.
* Question: How do React props contribute to reusability?
  * Answer: Props make components adaptable and reusable by allowing dynamic customization.
* Question: Can a reusable component have internal state?
  * Answer: Yes, a reusable component can have internal state by using a hook like useState.
* Question: Why is component abstraction important for reusability?
  * Answer: Abstraction hides unnecessary details, making components more versatile and easier to reuse without exposing their internal complexities.
* Question: Is it advisable to make all components in a React application reusable?
  * Answer: While reusability is beneficial, not all components need to be reusable. Often there are components which have only a single purpose and are only used once in a React application.