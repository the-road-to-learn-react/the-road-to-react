## Explicit Data Fetching with React

Re-fetching all data each time someone types in the input field isn't optimal. Since we're using a third-party API to fetch the data, its internals are out of our reach. Eventually, we will incur [rate limiting](https://en.wikipedia.org/wiki/Rate_limiting), which returns an error instead of data.

To solve this problem, change the implementation details from implicit to explicit data (re-)fetching. In other words, the application will refetch data only if someone clicks a confirmation button. First, add a button element for the confirmation to the JSX:

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

Second, the handler, input, and button handler receive implementation logic to update the component's state. The input field handler still updates the `searchTerm`; the button handler sets the `url` derived from the *current* `searchTerm` and the static API URL as a new state:

{title="src/App.js",lang="javascript"}
~~~~~~~
const App = () => {
  const [searchTerm, setSearchTerm] = useSemiPersistentState(
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
  const handleSearchInput = event => {
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

Third, instead of running the data fetching side-effect on every `searchTerm` change -- which would happen each time the input field's value changes -- the `url` is used. The `url` is set explicitly by the user when the search is confirmed via our new button:

{title="src/App.js",lang="javascript"}
~~~~~~~
const App = () => {
  ...

  const handleFetchStories = React.useCallback(() => {
    dispatchStories({ type: 'STORIES_FETCH_INIT' });

# leanpub-start-insert
    fetch(url)
# leanpub-end-insert
      .then(response => response.json())
      .then(result => {
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

Before the `searchTerm` was used for two cases: updating the input field's state and activating the side-effect for fetching data. Too many responsibilities one may would have said. Now it's only used for the former. A second state called `url` got introduced for triggering the side-effect for fetching data which only happens when a user clicks the confirmation button.

### Exercises:

* Confirm your [source code for the last section](https://codesandbox.io/s/github/the-road-to-learn-react/hacker-stories/tree/hs/Explicit-Data-Fetching-with-React).
  * Confirm the [changes from the last section](https://github.com/the-road-to-learn-react/hacker-stories/compare/hs/Memoized-Handler-in-React...hs/Explicit-Data-Fetching-with-React?expand=1).
* Why is `useState` instead of `useSemiPersistentState` used for the `url` state management?
* Why is there no check for an empty `searchTerm` in the `handleFetchStories` function anymore?