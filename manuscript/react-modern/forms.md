## Forms in React

There is no modern application that doesn't use forms. A form is just a proper vehicle to submit data via a button from various input controls (e.g. input field, checkbox, radio button, slider). Earlier we introduced a new button to fetch data explicitly with a button click. We'll advance its use with a proper HTML form, which encapsulates the button and input field for the search term with its label.

Forms aren't much different in React's JSX than in HTML. We'll implement it in two refactoring steps with some HTML/JavaScript. First, wrap the input field and button into an HTML form element:

{title="src/App.jsx",lang="javascript"}
~~~~~~~
const App = () => {
  ...

  return (
    <div>
      <h1>My Hacker Stories</h1>

# leanpub-start-insert
      <form onSubmit={handleSearchSubmit}>
# leanpub-end-insert
        <InputWithLabel
          id="search"
          value={searchTerm}
          isFocused
          onInputChange={handleSearchInput}
        >
          <strong>Search:</strong>
        </InputWithLabel>

# leanpub-start-insert
        <button type="submit" disabled={!searchTerm}>
# leanpub-end-insert
          Submit
        </button>
# leanpub-start-insert
      </form>
# leanpub-end-insert

      <hr />

      ...
    </div>
  );
};
~~~~~~~

Instead of passing the `handleSearchSubmit()` handler to the button, it's used in the new form element's `onSubmit` attribute. The button receives a new `type` attribute called `submit`, which indicates that the form element's `onSubmit` handles the click and not the button. Next, since the handler is used for the form event, it executes `preventDefault()` additionally on React's synthetic event. This prevents the HTML form's native behavior which would lead to a browser reload:

{title="src/App.jsx",lang="javascript"}
~~~~~~~
const App = () => {
  ...

# leanpub-start-insert
  const handleSearchSubmit = (event) => {
# leanpub-end-insert
    setUrl(`${API_ENDPOINT}${searchTerm}`);

# leanpub-start-insert
    event.preventDefault();
# leanpub-end-insert
  };

  ...
};
~~~~~~~

Now we can execute the search feature with the keyboard's "Enter" key, because we are using a form instead of just a standalone button. In the next two steps, we will separate the whole form into a new SearchForm component. If you want to go ahead yourself, do not hesitate. Anyway, this is how the form can be extracted into its own component:

{title="src/App.jsx",lang="javascript"}
~~~~~~~
# leanpub-start-insert
const SearchForm = ({
  searchTerm,
  onSearchInput,
  onSearchSubmit,
}) => (
  <form onSubmit={onSearchSubmit}>
    <InputWithLabel
      id="search"
      value={searchTerm}
      isFocused
      onInputChange={onSearchInput}
    >
      <strong>Search:</strong>
    </InputWithLabel>

    <button type="submit" disabled={!searchTerm}>
      Submit
    </button>
  </form>
);
# leanpub-end-insert
~~~~~~~

The new component is instantiated in the App component. The App component still manages the state for the form though, because the state triggers the data request in the App component where the requested data will eventually get passed as props (here: `stories.data`) to the List component:

{title="src/App.jsx",lang="javascript"}
~~~~~~~
const App = () => {
  ...

  return (
    <div>
      <h1>My Hacker Stories</h1>

# leanpub-start-insert
      <SearchForm
        searchTerm={searchTerm}
        onSearchInput={handleSearchInput}
        onSearchSubmit={handleSearchSubmit}
      />
# leanpub-end-insert

      <hr />

      {stories.isError && <p>Something went wrong ...</p>}

      {stories.isLoading ? (
        <p>Loading ...</p>
      ) : (
        <List list={stories.data} onRemoveItem={handleRemoveStory} />
      )}
    </div>
  );
};
~~~~~~~

Forms aren't much different in React than in plain HTML. When we have input fields and a button to submit data from them, we can give our HTML more structure by wrapping it into a form element with a `onSubmit` attribute. The button that executes the submission therefore needs the "submit" `type` to refer the process to the form element's handler. After all, it makes it more accessible for keyboard users as well.

### Exercises:

* Compare your source code against the author's [source code](https://tinyurl.com/mra85b3s).
  * Recap all the [source code changes](https://tinyurl.com/3ecdcpje) from this section.
  * Optional: If you are using TypeScript, check out the author's source code [here](https://bit.ly/42qnJ18).
* Read more about [forms in React](https://www.robinwieruch.de/react-form/).
* Try what happens without using `preventDefault`.
  * Read more about [preventDefault for events in React](https://www.robinwieruch.de/react-preventdefault/).

### Interview Questions:

* Questions: How do you handle form input in React?
  * Answers: In React, form input is typically managed using state. Each input field has a corresponding state variable, and the value of the input is set to the state.
* Questions: What is the purpose of the onChange event in React forms?
  * Answers: The onChange event is used to capture user input in real-time and update the state accordingly, ensuring the form reflects the latest input.
* Questions: How can you prevent the default form submission behavior in React?
  * Answers: Use the e.preventDefault() method within the form's submit handler to prevent the default form submission behavior.
* Questions: What is controlled and uncontrolled form input in React?
  * Answers: Controlled form input is when React state manages the input value. Uncontrolled input is when the DOM handles the input, and React does not track its state.
* Questions: How do you perform form validation in React?
  * Answers: Form validation in React is typically done by checking the input values against certain conditions or using validation libraries. The onSubmit handler is a common place to implement validation.
* Questions: What is the purpose of the value attribute in form inputs?
  * Answers: The value attribute sets the initial value of a form input and ensures that the input is controlled by React state.
* Questions: How can you handle multiple form inputs with a single onChange handler in React?
  * Answers: Use the name attribute on each input field and access the corresponding value using event.target.name in the onChange handler.
* Questions: What is the role of the onSubmit event in React forms?
  * Answers: The onSubmit event is triggered when the form is submitted. It's where you handle form validation, data processing, or any other actions related to the form submission.

## Forms with Actions

React forms are a powerful tool to submit data. In the previous section, we introduced a form to submit a search term to fetch data from an API. We used the `onSubmit` event handler to trigger the data fetching process. In this section, we'll introduce a new concept to the form: the `action` attribute. The `action` attribute is a standard HTML attribute that specifies the URL where the form data should be submitted. When using React, you can pass in an action function to the form component, which will be executed when the form is submitted:

{title="src/App.jsx",lang="javascript"}
~~~~~~~
# leanpub-start-insert
const SearchForm = ({ searchTerm, onSearchInput, searchAction }) => (
  <form action={searchAction}>
# leanpub-end-insert
    <InputWithLabel
      id="search"
      value={searchTerm}
      isFocused
      onInputChange={onSearchInput}
    >
      <strong>Search:</strong>
    </InputWithLabel>

    <button type="submit" disabled={!searchTerm}>
      Submit
    </button>
  </form>
);
~~~~~~~

Instead of passing the submit handler to the form's `onSubmit` attribute, we pass a new `searchAction` function to the form's `action` attribute:

{title="src/App.jsx",lang="javascript"}
~~~~~~~
<SearchForm
  searchTerm={searchTerm}
  onSearchInput={handleSearchInput}
# leanpub-start-insert
  searchAction={searchAction}
# leanpub-end-insert
/>
~~~~~~~

Since we are closer to the native form behavior, we can remove the `preventDefault()` call from the submit handler:

{title="src/App.jsx",lang="javascript"}
~~~~~~~
# leanpub-start-insert
const searchAction = () => {
# leanpub-end-insert
  setUrl(`${API_ENDPOINT}${searchTerm}`);

# leanpub-start-insert
  // event.preventDefault(); <--- we don't need this anymore
# leanpub-end-insert
};
~~~~~~~

Moving forward with React 19, the `action` attribute will be used over the `onSubmit` attribute to submit form data, because it takes more advantage of the native form behavior. Optionally the form action's function signature would give you access to the [form data](https://tinyurl.com/bddjd59s), which can be useful for form validation or other form-related tasks.

### Exercises:

* Compare your source code against the author's [source code](https://tinyurl.com/4nchwt46).
  * Recap all the [source code changes](https://tinyurl.com/2vnwf7ty) from this section.
* Read more about [React and FormData](https://www.robinwieruch.de/react-form-data/).
* Read more about [Forms and their Loading State](https://www.robinwieruch.de/react-form-loading-pending-action/).