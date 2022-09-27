## Forms in React

There will be no modern application that doesn't use forms. A form is just a proper vehicle to submit data via a button from various input controls (e.g. input field, checkbox, radio button, slider). Earlier we introduced a new button to fetch data explicitly with a button click. We'll advance its use with a proper HTML form, which encapsulates the button and input field for the search term with its label.

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

Now we can execute the search feature with the keyboard's "Enter" key, because we are using a form instead of just a standalone button. In the next two steps, we will separate the whole form into a new SearchForm component. If you want to go ahead yourself, do not hesitate. Anyway, that's how the form can gets extracted into its own component:

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

* Compare your source code against the author's [source code](https://bit.ly/3RXqiSl).
  * Recap all the [source code changes from this section](https://bit.ly/3fe5uaw).
  * Optional: If you are using TypeScript, check out the author's source code [here](https://bit.ly/3xVur1e).
* Read more about [forms in React](https://www.robinwieruch.de/react-form/).
* Try what happens without using `preventDefault`.
  * Read more about [preventDefault for events in React](https://www.robinwieruch.de/react-preventdefault/).
* Optional: [Leave feedback for this section](https://forms.gle/d14Mf7WzetP25jxq5).