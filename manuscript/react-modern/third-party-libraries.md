## Third-Party Libraries in React

We previously introduced the native fetch API (which the browser provides) to perform requests to the Hacker News API. However, not all browsers support this, especially the older ones. Also, once you start testing your application in a [headless browser environment](https://bit.ly/3ncFfSs), issues can arise with the fetch API, because the actual browser is not there. There are a couple of ways to make fetch work in older browsers ([polyfills](https://bit.ly/3ASC86Y)) and in tests (isomorphic fetch), but these concepts are a bit off-task for the purpose of this learning experience.

One alternative is to substitute the native fetch API with a stable library like [axios](https://bit.ly/3jjEupg), which performs asynchronous requests to remote APIs. In this section, we will discover how to substitute a library -- a native API of the browser in this case -- with another library from the npm registry.

If you know how to install axios via npm and use it as replacement for the browser's fetch API, go ahead and implement it yourself. Otherwise, the book will guide you through the implementation. First, install axios from the command line:

{title="Command Line",lang="text"}
~~~~~~~
npm install axios
~~~~~~~

Second, import axios in your App component's file:

{title="src/App.jsx",lang="javascript"}
~~~~~~~
import * as React from 'react';
# leanpub-start-insert
import axios from 'axios';
# leanpub-end-insert

...
~~~~~~~

You can use `axios` instead of `fetch`. Its usage looks almost identical to the native fetch API: It takes the URL as an argument and returns a promise. You don't have to transform the returned response to JSON anymore, since axios wraps the result into a data object in JavaScript for you. Just make sure to adapt your code to the returned data structure:

{title="src/App.jsx",lang="javascript"}
~~~~~~~
const App = () => {
  ...

  const handleFetchStories = React.useCallback(() => {
    dispatchStories({ type: 'STORIES_FETCH_INIT' });

# leanpub-start-insert
    axios
      .get(url)
# leanpub-end-insert
      .then((result) => {
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

In this code, we call axios `axios.get()` for an explicit [HTTP GET request](https://mzl.la/3n5kUyi), which is the same HTTP method we used by default with the browser's native fetch API. You can use other HTTP methods such as HTTP POST with `axios.post()` as well. We can see with these examples that axios is a powerful library for performing requests to remote APIs. I recommend it over the native fetch API when requests become complex, working with older browsers, or for testing.

### Exercises:

* Compare your source code against the author's [source code](https://bit.ly/3O8sXbU).
  * Recap all the [source code changes from this section](https://bit.ly/3vHe3mC).
  * Optional: If you are using TypeScript, check out Robin's source code [here](https://bit.ly/3SzVPtl).
* Read more about [popular libraries in React](https://www.robinwieruch.de/react-libraries/).
* Optional: Read more about [axios](https://bit.ly/3jjEupg).
* Optional: [Leave feedback for this section](https://forms.gle/wfDb7r5K4az3TiWN9).

### Interview Questions:

* Question: What is Axios in the context of React?
  * Answer: Axios is a popular JavaScript library for making HTTP requests, commonly used in React applications for data fetching.
* Question: How does Axios differ from the Fetch API in React?
  * Answer: Axios is a third-party library providing additional features and a more convenient API compared to the native Fetch API.
* Question: Why might you choose Axios over Fetch in a React project?
  * Answer: Axios offers features like automatic JSON parsing, request/response interceptors, and better browser support, making it a preferred choice for many developers.
* Question: How do you make a GET request using Axios in React?
  * Answer: Use axios.get(url) to make a GET request in React with Axios.
* Question: How is error handling done in Axios requests in React?
  * Answer: Axios provides a .catch() method to handle errors in the request.
* Question: What is the main advantage of using Fetch API in React?
  * Answer: The Fetch API is built into modern browsers, eliminating the need for additional dependencies and making it a lightweight choice for simple scenarios.