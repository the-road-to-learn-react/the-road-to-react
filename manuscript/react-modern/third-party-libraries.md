## Third-Party Libraries in React

We previously introduced the native fetch API to complete requests to the Hacker News API, which the browser provides. Not all browsers support this, however, especially the older ones. Also, once you start testing your application in a [headless browser environment](https://en.wikipedia.org/wiki/Headless_browser), issues can arise with the fetch API. There are a couple of ways to make fetch work in older browsers ([polyfills](https://en.wikipedia.org/wiki/Polyfill_(programming))) and in tests (isomorphic fetch), but these concepts are a bit off-task for the purpose of this learning experience.

One alternative is to substitute the native fetch API with a stable library like [axios](https://github.com/axios/axios), which performs asynchronous requests to remote APIs. In this section, we will discover how to substitute a library--a native API of the browser in this case--with another library from the npm registry. First, install axios on the command line:

{title="Command Line",lang="text"}
~~~~~~~
npm install axios
~~~~~~~

Second, import axios in your App component's file:

{title="src/App.js",lang="javascript"}
~~~~~~~
import React from 'react';
# leanpub-start-insert
import axios from 'axios';
# leanpub-end-insert

...
~~~~~~~

You can use `axios` instead of `fetch`, and its usage looks almost identical to the native fetch API. It takes the URL as an argument and returns a promise. You don't have to transform the returned response to JSON anymore, since axios wraps the result into a data object in JavaScript. Just make sure to adapt your code to the returned data structure:

{title="src/App.js",lang="javascript"}
~~~~~~~
const App = () => {
  ...

  const handleFetchStories = React.useCallback(() => {
    dispatchStories({ type: 'STORIES_FETCH_INIT' });

# leanpub-start-insert
    axios
      .get(url)
# leanpub-end-insert
      .then(result => {
        dispatchStories({
          type: 'STORIES_FETCH_SUCCESS',
# leanpub-start-insert
          payload: result.data.hits,
# leanpub-end-insert
        });
      })
      .catch(() =>
        dispatchStories({ type: 'STORIES_FETCH_FAILURE' })
      );
  }, [url]);

  ...
};
~~~~~~~

In this code, we call axios `axios.get()` for an explicit [HTTP GET request](https://developer.mozilla.org/en-US/docs/Web/HTTP/Methods/GET), which is the same HTTP method we used by default with the browser's native fetch API. You can use other HTTP methods such as HTTP POST with `axios.post()`as well.

We can see with these examples that axios is a powerful library for performing requests to remote APIs. I recommend over the native fetch API when requests become complex, working with older browser, or for testing.

### Exercises:

* Confirm your [source code for the last section](https://codesandbox.io/s/github/the-road-to-learn-react/hacker-stories/tree/hs/Third-Party-Libraries-in-React).
  * Confirm the [changes from the last section](https://github.com/the-road-to-learn-react/hacker-stories/compare/hs/Explicit-Data-Fetching-with-React...hs/Third-Party-Libraries-in-React?expand=1).
* Read more about [popular libraries in React](https://www.robinwieruch.de/react-libraries).
* Read more about [why frameworks matter](https://www.robinwieruch.de/why-frameworks-matter).
* Read more about [axios](https://github.com/axios/axios).