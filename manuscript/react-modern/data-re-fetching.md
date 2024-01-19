## Data Re-Fetching in React

Now, we have data retrieved from a remote API, providing a more engaging environment compared to sample data. If you launched your application after the previous section, you might have sensed that it lacks completeness. Since we fetch data with a predefined query (in this case: `'react'`), we consistently see stories related to "React." Despite having a search feature, it can only filter existing stories. Therefore, the search feature is termed a client-side search because it operates solely on the available data, without interacting with the remote API.

While a client-side search only filters the stories that are available on the client (after the initial data fetching), a server-side search would allow us to get data from the remote API based on the search term. Essentially client-side and server-side searching differ in where the search operation takes place. Client-side searching occurs on the user's device, providing fast responses but may be less suitable for large datasets. Server-side searching happens on the server, better suited for large datasets but may result in slower user response times due to server round-trips. The choice depends on factors like dataset size, search complexity, and performance considerations. In this section, we want to change the client-side search to a server-side search. Try to tackle the following task yourself again before continuing to read the book.

**Task:** The search feature is a client-side search, because it filters only the data that's already there. Instead it should be possible to use the search to fetch data related to the search term.

**Optional Hints:**

* The calculated value `searchedStories` can be omitted as we anticipate filtered data directly from the API.
* In the data retrieval process, replace the hardcoded `'react'` with the dynamic `searchTerm`.
* Address the edge case where `searchTerm` is an empty string.

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

We've transitioned the feature from a client-side search to a server-side search. Instead of filtering a predefined list of stories on the client, the `searchTerm` is now utilized to retrieve a server-side filtered list. Server-side searching occurs not only during the initial data fetch but also when the `searchTerm` undergoes changes. The search feature is now entirely server-side.

Note: However, re-fetching data with every keystroke isn't optimal, as this implementation puts strain on the API with frequent requests. Excessive requests may lead to API errors due to rate limiting, a measure many APIs employ to protect against a high volume of requests (e.g., allowing only X requests in 1 minute). We plan to address this issue soon.

### Exercises:

* Compare your source code against the author's [source code](https://bit.ly/3vF23lz).
  * Recap all the [source code changes from this section](https://bit.ly/3U4PB8Q).
  * Optional: If you are using TypeScript, check out Robin's source code [here](https://bit.ly/3C9TFvb).
* Optional: [Leave feedback for this section](https://forms.gle/ywE4bFy6D2HSG8Rd7).

### Interview Questions:

* Question: What is client-side searching?
  * Answer: Client-side searching involves filtering and manipulating data on the user's device or browser.
* Question: How does client-side searching impact performance?
  * Answer: It can offer fast response times but may be limited by the amount of data that needs to be loaded from the server.
* Question: What is server-side searching?
  * Answer: Server-side searching entails sending search queries to the server, where the data is filtered, and the results are returned to the client.
* Question: When is server-side searching preferred?
  * Answer: Server-side searching is preferable for large datasets or when complex search logic and data reside on the server.
* Question: What are potential drawbacks of client-side searching?
  * Answer: Limitations may arise with large datasets, slower initial page loads, and the need to load extensive data to the client.
* Question: What is the impact of frequent API requests in client-side searching?
  * Answer: Frequent requests can stress the API, potentially leading to errors, especially if the API employs rate limiting measures.
* Question: How can the performance issue of frequent API requests be addressed?
  * Answer: Implementing debouncing or throttling techniques can mitigate the impact of frequent API requests and prevent overloading the server.