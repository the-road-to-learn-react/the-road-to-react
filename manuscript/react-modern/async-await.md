## Async/Await in React (Advanced)

You'll work with asynchronous data often in React, so it's good to know alternative syntax for handling promises: async/await. The following refactoring of the `handleFetchStories` function without error handling shows how:

{title="src/App.js",lang="javascript"}
~~~~~~~
const App = () => {
  ...

# leanpub-start-insert
  const handleFetchStories = React.useCallback(async () => {
# leanpub-end-insert
    dispatchStories({ type: 'STORIES_FETCH_INIT' });

# leanpub-start-insert
    const result = await axios.get(url);
# leanpub-end-insert

# leanpub-start-insert
    dispatchStories({
      type: 'STORIES_FETCH_SUCCESS',
      payload: result.data.hits,
    });
# leanpub-end-insert
  }, [url]);

  ...
};
~~~~~~~

To use async/await, our function requires the `async` keyword. Once you start using the `await` keyword, everything reads like synchronous code. Actions after the `await` keyword are not executed until promise resolves, meaning the code will wait.

{title="src/App.js",lang="javascript"}
~~~~~~~
const App = () => {
  ...

  const handleFetchStories = React.useCallback(async () => {
    dispatchStories({ type: 'STORIES_FETCH_INIT' });

# leanpub-start-insert
    try {
# leanpub-end-insert
      const result = await axios.get(url);

      dispatchStories({
        type: 'STORIES_FETCH_SUCCESS',
        payload: result.data.hits,
      });
# leanpub-start-insert
    } catch {
      dispatchStories({ type: 'STORIES_FETCH_FAILURE' });
    }
# leanpub-end-insert
  }, [url]);

  ...
};
~~~~~~~

To include error handling as before, the `try` and `catch` blocks are there to help. If something goes wrong in the `try` block,  the code will jump into the `catch` block to handle the error. `then`/`catch` blocks and async/await with `try`/`catch` blocks are both valid for handling asynchronous data in JavaScript and React.

### Exercises:

* Confirm your [source code for the last section](https://codesandbox.io/s/github/the-road-to-learn-react/hacker-stories/tree/hs/Async-Await-in-React).
  * Confirm the [changes from the last section](https://github.com/the-road-to-learn-react/hacker-stories/compare/hs/Third-Party-Libraries-in-React...hs/Async-Await-in-React?expand=1).
* Read more about [data fetching in React](https://www.robinwieruch.de/react-hooks-fetch-data).
* Read more about [async/await in JavaScript](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/async_function).