## Data Re-Fetching in React

So far, the App component fetches a list of stories once with a predefined query (`react`). After that, users can search for stories on the client-side. Now we'll move this feature from client-side to server-side searching, using the actual `searchTerm` as a dynamic query for the API request.

First, remove `searchedStories`, because we will receive the stories searched from the API. Pass only the regular stories to the List component:

{title="src/App.js",lang="javascript"}
~~~~~~~
const App = () => {
  ...

  return (
    <div>
      ...

      {stories.isLoading ? (
        <p>Loading ...</p>
      ) : (
# leanpub-start-insert
        <List list={stories.data} onRemoveItem={handleRemoveStory} />
# leanpub-end-insert
      )}
    </div>
  );
};
~~~~~~~

And second, instead of using a hardcoded search term like before, use the actual `searchTerm` from the component's state. If `searchTerm` is an empty string, do nothing:

{title="src/App.js",lang="javascript"}
~~~~~~~
const App = () => {
  ...

  React.useEffect(() => {
# leanpub-start-insert
    if (searchTerm === '') return;
# leanpub-end-insert

    dispatchStories({ type: 'STORIES_FETCH_INIT' });

# leanpub-start-insert
    fetch(`${API_ENDPOINT}${searchTerm}`)
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
  }, []);

  ...
};
~~~~~~~

The initial search respects the search term now, so we'll implement data refetching. If the `searchTerm` changes, run the side-effect for the data fetching again. If `searchTerm` is not present (e.g. null, empty string, undefined), do nothing (as a more generalized condition):

{title="src/App.js",lang="javascript"}
~~~~~~~
const App = () => {
  ...

  React.useEffect(() => {
# leanpub-start-insert
    if (!searchTerm) return;
# leanpub-end-insert

    dispatchStories({ type: 'STORIES_FETCH_INIT' });

    fetch(`${API_ENDPOINT}${searchTerm}`)
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
  }, [searchTerm]);
# leanpub-end-insert

  ...
};
~~~~~~~

We changed the feature from a client-side to server-side search. Instead of filtering a predefined list of stories on the client, the `searchTerm` is used to fetch a server-side filtered list. The server-side search happens for the initial data fetching, but also for data fetching if the `searchTerm` changes. The feature is fully server-side now.

Re-fetching data each time someone types into the input field isn't optimal, so we'll correct that soon. Because this implementation stresses the API, you might experience errors if you use requests too often.

### Exercises:

* Confirm your [source code for the last section](https://codesandbox.io/s/github/the-road-to-learn-react/hacker-stories/tree/hs/Data-Re-Fetching-in-React).
  * Confirm the [changes from the last section](https://github.com/the-road-to-learn-react/hacker-stories/compare/hs/Data-Fetching-with-React...hs/Data-Re-Fetching-in-React?expand=1).