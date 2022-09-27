## Explicit Data Fetching with React

Re-fetching all data each time someone types in the input field isn't optimal. Since we're using a third-party API to fetch the data, its internals are out of our reach. Eventually, we will be confronted with [rate limiting](https://bit.ly/2ZaJXI8) which returns an error instead of data. To solve this problem, we will change the implementation details from implicit to explicit data (re-)fetching. In other words, the application will refetch data only if someone clicks a confirmation button.

**Task:** The server-side search executes every time a user types into the input field. The new implementation should only execute a search when a user clicks a confirmation button. As long as the button is not clicked, the search term can change but isn't executed as API request.

**Optional Hints:**

* Add a button element to confirm the search request.
* Create a stateful value for the confirmed search.
* The button's event handler sets confirmed search as state by using the current search term.
* Only when the new confirmed search is set as state, execute the side-effect to perform a server-side search.

What's important with this feature is that we need a state for the fluctuating `searchTerm` and a new state for the confirmed search. But first of all, create a new button element which confirms the search and executes the data request eventually:

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
        isFocused
# leanpub-start-insert
        onInputChange={handleSearchInput}
# leanpub-end-insert
      >
        <strong>Search:</strong>
      </InputWithLabel>

# leanpub-start-insert
      <button
        type="button"
        disabled={!searchTerm}
        onClick={handleSearchSubmit}
      >
        Submit
      </button>
# leanpub-end-insert

      ...
    </div>
  );
};
~~~~~~~

Second, we distinguish between the handler of the input field and the button. While the renamed handler of the input field still sets the stateful `searchTerm`, the new handler of the button sets the new stateful value called `url` which is derived from the *current* `searchTerm` and the static API endpoint as a new state:

{title="src/App.jsx",lang="javascript"}
~~~~~~~
const App = () => {
  const [searchTerm, setSearchTerm] = useStorageState(
    'search',
    'React'
  );

# leanpub-start-insert
  const [url, setUrl] = React.useState(
    `${API_ENDPOINT}${searchTerm}`
  );
# leanpub-end-insert

  ...

# leanpub-start-insert
  const handleSearchInput = (event) => {
# leanpub-end-insert
    setSearchTerm(event.target.value);
  };

# leanpub-start-insert
  const handleSearchSubmit = () => {
    setUrl(`${API_ENDPOINT}${searchTerm}`);
  };
# leanpub-end-insert

  ...
};
~~~~~~~

Third, instead of running the data fetching side-effect on every `searchTerm` change (which happens each time the input field's value changes like we have seen before), the new stateful `url` is used whenever a user changes it by confirming a search request when clicking the button:

{title="src/App.jsx",lang="javascript"}
~~~~~~~
const App = () => {
  ...

  const handleFetchStories = React.useCallback(() => {
    dispatchStories({ type: 'STORIES_FETCH_INIT' });

# leanpub-start-insert
    fetch(url)
# leanpub-end-insert
      .then((response) => response.json())
      .then((result) => {
        dispatchStories({
          type: 'STORIES_FETCH_SUCCESS',
          payload: result.hits,
        });
      })
      .catch(() =>
        dispatchStories({ type: 'STORIES_FETCH_FAILURE' })
      );
# leanpub-start-insert
  }, [url]);
# leanpub-end-insert

  React.useEffect(() => {
    handleFetchStories();
  }, [handleFetchStories]);

  ...
};
~~~~~~~

Before the `searchTerm` was used for two cases: updating the input field's state and activating the side-effect for fetching data. Now it's only used for the former. A second state called `url` got introduced for triggering the side-effect that fetches the data which only happens when a user clicks the confirmation button.

### Exercises:

* Compare your source code against the author's [source code](https://bit.ly/3SkBAjX).
  * Recap all the [source code changes from this section](https://bit.ly/3qVR29V).
  * Optional: If you are using TypeScript, check out the author's source code [here](https://bit.ly/3Cf3ND9).
* Question: Why is `useState` instead of `useStorageState` used for the `url` state management?
  * Answer: We do not want to remember the `url` in the browser's local storage, because it's already derived from a static string (here: `API_ENDPOINT`) and the `searchTerm` which already comes from the browser's local storage.
* Question: Why is there no check for an empty `searchTerm` in the `handleFetchStories` function anymore?
  * Answer: Preventing a server-side search happens in the new button, because it gets disabled whenever there is no `searchTerm`.
* Optional: [Leave feedback for this section](https://forms.gle/HuJDuVNZmEDbhGzU9).