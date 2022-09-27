## Data Re-Fetching in React

Finally we have data from a remote API. That's more encouraging to play around with in contrast to sample data. When you started your application after the last section, you may have noticed that it doesn't feel complete yet. Because we are fetching data with a predefined query (here: `'react'`), we always see "React"-related stories. Even though we have a search feature, we can filter only the stories that are already there. Hence the search feature is called a client-side search, because it does not interact with the remote API, but only with the data that's already there.

In this section, we want to change the client-side search to a server-side search. While a client-side search only filters the stories that are available on the client (after the initial data fetching), a server-side search would allow us to get data from the remote API based on the search term. Try to tackle the following task yourself again before continuing to read the book.

**Task:** The search feature is a client-side search, because it filters only the data that's already there. Instead it should be possible to use the search to fetch data related to the search term.

**Optional Hints:**

* The derived value `searchedStories` can be removed, because we expect the data to come filtered from the API.
* For the data fetching, the hardcoded `'react'` needs to get replaced by the `searchTerm`.
* Handle the edge case when `searchTerm` is an empty string.

There are not many steps involved to migrate the application from a client-side to a server-side search. First, remove `searchedStories` because we will receive the stories filtered by search term from the API. Pass only the regular stories to the List component:

{title="src/App.jsx",lang="javascript"}
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

And second, instead of using the hardcoded search term (here: `'react'`), use the actual `searchTerm` from the component's state. Afterward, every time a user searches for something via the input field, the `searchTerm` will be used to request these kind of stories from the remote API. In addition, you need to deal with the edge case if `searchTerm` is an empty string, which means preventing a request from being fired:

{title="src/App.jsx",lang="javascript"}
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
  }, []);

  ...
};
~~~~~~~

There is one crucial piece missing now. While the initial data fetching respects the `searchTerm` (here: `'React'` which is set as initial state), the `searchTerm` is not respected when it is changed via a user typing into the input field. If you inspect the dependency array of our `useEffect` hook, you will see that it's empty. This means the side-effect only renders for the initial rendering of the App component. If we would want to run the side-effect also when the `searchTerm` changes, we would have to include it in the dependency array:

{title="src/App.jsx",lang="javascript"}
~~~~~~~
const App = () => {
  ...

  React.useEffect(() => {
# leanpub-start-insert
    // if `searchTerm` is not present
    // e.g. null, empty string, undefined
    // do nothing
    // more generalized condition than searchTerm === ''

    if (!searchTerm) return;
# leanpub-end-insert

    dispatchStories({ type: 'STORIES_FETCH_INIT' });

    fetch(`${API_ENDPOINT}${searchTerm}`)
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
  }, [searchTerm]);
# leanpub-end-insert

  ...
};
~~~~~~~

We changed the feature from a client-side to server-side search. Instead of filtering a predefined list of stories on the client, the `searchTerm` is used to fetch a server-side filtered list. The server-side search happens not only for the initial data fetching, but also for data fetching if the `searchTerm` changes. The search feature is fully server-side now.

Caveat: Re-fetching data each time someone types into the input field isn't optimal though, because this implementation stresses the API with every keystroke. Hence you might experience API errors if you trigger requests too often, because most APIs protect themselves against the flood of request by using so called rate limiting (e.g. allowing only X requests in 1 minute). We will fix this soonish.

### Exercises:

* Compare your source code against the author's [source code](https://bit.ly/3DJRZd3).
  * Recap all the [source code changes from this section](https://bit.ly/3S2P1og).
  * Optional: If you are using TypeScript, check out the author's source code [here](https://bit.ly/3C9TFvb).
* Optional: [Leave feedback for this section](https://forms.gle/ywE4bFy6D2HSG8Rd7).