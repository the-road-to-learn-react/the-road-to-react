## Forms in React

Earlier we introduced a new button to fetch data explicitly with a button click. We'll advance its use with a proper HTML form, which encapsulates the button and input field for the search term with its label.

Forms aren't much different in React's JSX than in HTML. We'll implement it in two refactoring steps with some HTML/JavaScript. First, wrap the input field and button into an HTML form element:

{title="src/App.js",lang="javascript"}
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

Instead of passing the `handleSearchSubmit` handler to the button, it's used in the new form element. The button receives a new `type` attribute called `submit`, which indicates that the form element handles the click and not the button.

Since the handler is used for the form event, it executes `preventDefault` in React's synthetic event. This prevents the HTML form's native behavior, which leads to a browser reload.

{title="src/App.js",lang="javascript"}
~~~~~~~
const App = () => {
  ...

# leanpub-start-insert
  const handleSearchSubmit = event => {
# leanpub-end-insert
    setUrl(`${API_ENDPOINT}${searchTerm}`);

# leanpub-start-insert
    event.preventDefault();
# leanpub-end-insert
  };

  ...
};
~~~~~~~

Now we can execute the search feature with the keyboard's `Enter` key. In the next two steps, we will only separate the component into its standalone SearchForm component:

{title="src/App.js",lang="javascript"}
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

The new component is used by the App component. The App component still manages the state for the form, because the state is used in the App component to fetch data passed as props (`stories.data`) to the List component:

{title="src/App.js",lang="javascript"}
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

Forms aren't much different in React than HTML. When we have input fields and a button to submit data from them, we can give our HTML more structure by wrapping it into a form element with a `onSubmit` handler. The button that executes the submission needs only the "submit" `type`.

### Exercises:

* Confirm your [source code for the last section](https://codesandbox.io/s/github/the-road-to-learn-react/hacker-stories/tree/hs/Forms-in-React).
  * Confirm the [changes from the last section](https://github.com/the-road-to-learn-react/hacker-stories/compare/hs/Async-Await-in-React...hs/Forms-in-React?expand=1).
* Try the code without `preventDefault`.
* Read more about [preventDefault for Events in React](https://www.robinwieruch.de/react-preventdefault).
* Read more about [React Component Composition](https://www.robinwieruch.de/react-component-composition).