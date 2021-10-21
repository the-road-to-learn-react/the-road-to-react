## Forms in React

Earlier we introduced a new button to fetch data explicitly with a button click. We'll advance its use with a proper HTML form, which encapsulates the button and input field for the search term with its label. Forms aren't much different in React's JSX than in HTML. We'll implement it in two refactoring steps with some HTML/JavaScript. First, wrap the input field and button into an HTML form element:

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

Instead of passing the `handleSearchSubmit` handler to the button, it's used in the new form element. The button receives a new `type` attribute called `submit`, which indicates that the form element handles the click and not the button. Since the handler is used for the form event, it executes `preventDefault` in React's synthetic event. This prevents the HTML form's native behavior, which leads to a browser reload:

{title="src/App.js",lang="javascript"}
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

Now we can execute the search feature with the keyboard's "Enter" key, because we are using a form instead of just a standalone button. In the next two steps, we will only separate the component into its own SearchForm component:

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

The new component is used by the App component. However, the App component still manages the state for the form, because the state is used in the App component to fetch data passed as props (`stories.data`) to the List component:

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

Forms aren't much different in React than in plain HTML. When we have input fields and a button to submit data from them, we can give our HTML more structure by wrapping it into a form element with a `onSubmit` handler. The button that executes the submission needs only the "submit" `type`. After all, it makes it more accessible for keyboard users as well.

### Exercises:

* Confirm your [source code](https://bit.ly/3jfEcjd).
  * Confirm the [changes](https://bit.ly/3G43M4I).
* Try what happens without using `preventDefault`.
  * Read more about [preventDefault for events in React](https://www.robinwieruch.de/react-preventdefault).
* Optional: [Leave feedback for this section](https://forms.gle/d14Mf7WzetP25jxq5).