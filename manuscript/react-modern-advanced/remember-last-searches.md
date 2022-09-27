## Remember Last Searches

**Task:** Remember the last five search terms which hit the API, and provide a button to move quickly between searches. When the buttons are clicked, stories for the search term should be fetched again.

**Optional Hints:**

* Don't use a new state for this feature. Instead, reuse the `url` state and `setUrl` state updater function to fetch stories from the API. Adapt them to multiple `urls` as state, and to set multiple `urls` with `setUrls`. The last URL from `urls` can be used to fetch the data, and the last five URLs from `urls` can be used to display the buttons.

![](images/last-searches.png)

Let's get to it. First, we will refactor all `url` to `urls` state and all `setUrl` to `setUrls` state updater functions. Instead of initializing the state with an `url` as a string, make it an array with the initial `url` as its only entry:

{title="src/App.jsx",lang="javascript"}
~~~~~~~
const App = () => {
  ...

# leanpub-start-insert
  const [urls, setUrls] = React.useState([
# leanpub-end-insert
    `${API_ENDPOINT}${searchTerm}`,
# leanpub-start-insert
  ]);
# leanpub-end-insert

  ...
};
~~~~~~~

Second, instead of using the current `url` state for data fetching, use the last `url` entry from the `urls` array. If another `url` is added to the list of `urls`, it is used to fetch data instead:

{title="src/App.jsx",lang="javascript"}
~~~~~~~
const App = () => {

  ...

  const handleFetchStories = React.useCallback(async () => {
    dispatchStories({ type: 'STORIES_FETCH_INIT' });

    try {
# leanpub-start-insert
      const lastUrl = urls[urls.length - 1];
      const result = await axios.get(lastUrl);
# leanpub-end-insert

      dispatchStories({
        type: 'STORIES_FETCH_SUCCESS',
        payload: result.data.hits,
      });
    } catch {
      dispatchStories({ type: 'STORIES_FETCH_FAILURE' });
    }
# leanpub-start-insert
  }, [urls]);
# leanpub-end-insert

  ...
};
~~~~~~~

And third, instead of storing the `url` string as state with the state updater function, concatenate the new `url` using the concat method with the previous `urls` in an array for the new state:

{title="src/App.jsx",lang="javascript"}
~~~~~~~
const App = () => {
  ...

  const handleSearchSubmit = (event) => {
# leanpub-start-insert
    const url = `${API_ENDPOINT}${searchTerm}`;
    setUrls(urls.concat(url));
# leanpub-end-insert

    event.preventDefault();
  };

  ...
};
~~~~~~~

With each search, another URL is stored in our state of `urls`. Next, render a button for each of the last five URLs. We'll include a new universal handler for these buttons, and each passes a specific `url` with a more specific inline handler:

{title="src/App.jsx",lang="javascript"}
~~~~~~~
# leanpub-start-insert
const getLastSearches = (urls) => urls.slice(-5);
# leanpub-end-insert

...

const App = () => {
  ...

# leanpub-start-insert
  const handleLastSearch = (url) => {
    // do something
  };
# leanpub-end-insert

# leanpub-start-insert
  const lastSearches = getLastSearches(urls);
# leanpub-end-insert

  return (
    <div>
      <h1>My Hacker Stories</h1>

      <SearchForm ... />

# leanpub-start-insert
      {lastSearches.map((url) => (
        <button
          key={url}
          type="button"
          onClick={() => handleLastSearch(url)}
        >
          {url}
        </button>
      ))}
# leanpub-end-insert

      ...
    </div>
  );
};
~~~~~~~

Next, instead of showing the whole URL of the last search in the button as button text, show only the search term by replacing the API's endpoint with an empty string:

{title="src/App.jsx",lang="javascript"}
~~~~~~~
# leanpub-start-insert
const extractSearchTerm = (url) => url.replace(API_ENDPOINT, '');

const getLastSearches = (urls) =>
  urls.slice(-5).map((url) => extractSearchTerm(url));
# leanpub-end-insert

...

const App = () => {
  ...

  const lastSearches = getLastSearches(urls);

  return (
    <div>
      ...

# leanpub-start-insert
      {lastSearches.map((searchTerm) => (
  # leanpub-end-insert
        <button
# leanpub-start-insert
          key={searchTerm}
# leanpub-end-insert
          type="button"
# leanpub-start-insert
          onClick={() => handleLastSearch(searchTerm)}
# leanpub-end-insert
        >
# leanpub-start-insert
          {searchTerm}
# leanpub-end-insert
        </button>
      ))}

      ...
    </div>
  );
};
~~~~~~~

The `getLastSearches` function now returns search terms instead of URLs. The actual `searchTerm` is passed to the inline handler instead of the `url`. By mapping over the list of `urls` in `getLastSearches`, we can extract the search term for each `url` within the array's map method. Making it more concise, it can also look like this:

{title="src/App.jsx",lang="javascript"}
~~~~~~~
const getLastSearches = (urls) =>
# leanpub-start-insert
  urls.slice(-5).map(extractSearchTerm);
# leanpub-end-insert
~~~~~~~

Now we'll provide functionality for the new handler used by every button, since clicking one of these buttons should trigger another search. Since we use the `urls` state for fetching data, and since we know the last URL is always used for data fetching, concatenate a new `url` to the list of `urls` to trigger another search request:

{title="src/App.jsx",lang="javascript"}
~~~~~~~
const App = () => {
  ...

# leanpub-start-insert
  const handleLastSearch = (searchTerm) => {
    const url = `${API_ENDPOINT}${searchTerm}`;
    setUrls(urls.concat(url));
  };
# leanpub-end-insert

  ...
};
~~~~~~~

If you compare this new handler's implementation logic to the `handleSearchSubmit`, you may see some common functionality. Extract this common functionality to a new handler and a new extracted utility function:

{title="src/App.jsx",lang="javascript"}
~~~~~~~
# leanpub-start-insert
const getUrl = (searchTerm) => `${API_ENDPOINT}${searchTerm}`;
# leanpub-end-insert

...

const App = () => {
  ...

# leanpub-start-insert
  const handleSearch = (searchTerm) => {
    const url = getUrl(searchTerm);
    setUrls(urls.concat(url));
  };
# leanpub-end-insert

  const handleSearchSubmit = (event) => {
# leanpub-start-insert
    handleSearch(searchTerm);
# leanpub-end-insert

    event.preventDefault();
  };

  const handleLastSearch = (searchTerm) => {
# leanpub-start-insert
    handleSearch(searchTerm);
# leanpub-end-insert
  };

  ...
};
~~~~~~~

The new utility function can be used somewhere else in the App component. If you extract functionality that can be used by two parties, always check to see if it can be used by a third-party:

{title="src/App.jsx",lang="javascript"}
~~~~~~~
const App = () => {
  ...

  // important: still wraps the returned value in []
# leanpub-start-insert
  const [urls, setUrls] = React.useState([getUrl(searchTerm)]);
# leanpub-end-insert

  ...
};
~~~~~~~

The functionality should work, but it complains or breaks if the same search term is used more than once, because `searchTerm` is used for each button element as `key` attribute. Make the key more specific by concatenating it with the `index` of the mapped array.

{title="src/App.jsx",lang="javascript"}
~~~~~~~
const App = () => {
  ...

  return (
    <div>
      ...

# leanpub-start-insert
      {lastSearches.map((searchTerm, index) => (
# leanpub-end-insert
        <button
# leanpub-start-insert
          key={searchTerm + index}
# leanpub-end-insert
          type="button"
          onClick={() => handleLastSearch(searchTerm)}
        >
          {searchTerm}
        </button>
      ))}

      ...
    </div>
  );
};
~~~~~~~

It's not the perfect solution, because the `index` isn't a stable key (especially when adding items to the list); however, it doesn't break in this scenario. The feature works now, but you can add further UX improvements by following the tasks below.

**More Tasks:**

* (1) Do not show the current search as a button, only the five preceding searches. Hint: Adapt the `getLastSearches` function.
* (2) Don't show duplicated searches. Searching twice for "React" shouldn't create two different buttons. Hint: Adapt the `getLastSearches` function.
* (3) Set the SearchForm component's input field value with the last search term if one of the buttons is clicked.

The source of the five rendered buttons is the `getLastSearches` function. There, we take the array of `urls` and return the last five entries from it. Now we'll change this utility function to return the last six entries instead of five by removing the last one, in order to not show the current search as a button. Afterward, only the five *previous* searches are displayed as buttons:

{title="src/App.jsx",lang="javascript"}
~~~~~~~
const getLastSearches = (urls) =>
  urls
# leanpub-start-insert
    .slice(-6)
    .slice(0, -1)
# leanpub-end-insert
    .map(extractSearchTerm);
~~~~~~~

If the same search is executed two or more times in a row, duplicate buttons appear, which is likely not your desired behavior. It would be acceptable to group identical searches into one button if they followed each other. We will solve this problem in the utility function as well. Before separating the array into the five previous searches, group the identical searches:

{title="src/App.jsx",lang="javascript"}
~~~~~~~
const getLastSearches = (urls) =>
  urls
# leanpub-start-insert
    .reduce((result, url, index) => {
      const searchTerm = extractSearchTerm(url);

      if (index === 0) {
        return result.concat(searchTerm);
      }

      const previousSearchTerm = result[result.length - 1];

      if (searchTerm === previousSearchTerm) {
        return result;
      } else {
        return result.concat(searchTerm);
      }
    }, [])
# leanpub-end-insert
    .slice(-6)
    .slice(0, -1);
~~~~~~~

The reduce function starts with an empty array as its `result`. The first iteration concatenates the `searchTerm` we extracted from the first `url` into the `result`. Every extracted `searchTerm` is compared to the one before it. If the previous search term is different from the current, concatenate the `searchTerm` to the result. If the search terms are identical, return the result without adding anything.

The SearchForm component's input field should be set with the new `searchTerm` if one of the last search buttons is clicked. We can solve this using the state updater function for the specific value used in the SearchForm component.

{title="src/App.jsx",lang="javascript"}
~~~~~~~
const App = () => {
  ...

  const handleLastSearch = (searchTerm) => {
# leanpub-start-insert
    setSearchTerm(searchTerm);
# leanpub-end-insert

    handleSearch(searchTerm);
  };

  ...
};
~~~~~~~

Lastly, extract the feature's new rendered content from this section as a standalone component, to keep the App component lightweight:

{title="src/App.jsx",lang="javascript"}
~~~~~~~
const App = () => {
  ...

  const lastSearches = getLastSearches(urls);

  return (
    <div>
      ...

      <SearchForm ... />

# leanpub-start-insert
      <LastSearches
        lastSearches={lastSearches}
        onLastSearch={handleLastSearch}
      />
# leanpub-end-insert

      <hr />

      ...
    </div>
  );
};

# leanpub-start-insert
const LastSearches = ({ lastSearches, onLastSearch }) => (
  <>
    {lastSearches.map((searchTerm, index) => (
      <button
        key={searchTerm + index}
        type="button"
        onClick={() => onLastSearch(searchTerm)}
      >
        {searchTerm}
      </button>
    ))}
  </>
);
# leanpub-end-insert
~~~~~~~

This feature wasn't an easy one. Lots of fundamental React but also JavaScript knowledge was needed to accomplish it. If you had no problems implementing it yourself or in following the instructions, you are very well set. If you had one or the other issue, don't worry too much about it. Maybe you even figured out another way to solve this task and it may have turned out simpler than the one I showed here.

### Exercises:

* Compare your source code against the author's [source code](https://bit.ly/3fqttDT).
  * Recap all the [source code changes from this section](https://bit.ly/3rgcDda).
* Read more about [grouping in JavaScript](https://www.robinwieruch.de/javascript-groupby/).
* Optional: [Leave feedback for this section](https://forms.gle/LhNVodZgu8qTqHhN6).