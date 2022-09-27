## Async/Await in React

There is no way around asynchronous data when working on real world applications. There will always be a remote API that gives your frontend data, so you need to work with this data asynchronously. In our React application, we have started to resolve promises with then/catch blocks. However, in modern JavaScript (and therefore React), a more popular solution is using async/await.

If you are already familiar with async/await or you want to explore [its usage](https://mzl.la/3AWyWaw) yourself, go ahead and change the code from using then/catch to async/await. If you have come this far, you could also consider compensating for the removal of the catch block for the error handling by using a try/catch blocks instead.

Let's continue with the task here. First, you would have to replace the then/catch syntax with the async/await syntax. The following refactoring of the `handleFetchStories()` function shows how to accomplish it without error handling:

{title="src/App.jsx",lang="javascript"}
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

To use async/await, our function requires the `async` keyword. Once you start using the `await` keyword on returned promises, everything reads like synchronous code. Actions after the `await` keyword are not executed until the promise resolves, meaning the code will wait. To include error handling as before, the `try` and `catch` blocks are there to help. If something goes wrong in the `try` block, the code will jump into the `catch` block to handle the error:

{title="src/App.jsx",lang="javascript"}
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

After all, using async/await with try/catch over then/catch makes it often more readable, because we avoid using callback functions and instead try to make our code more readable in a synchronous way. However, using then/catch is fine too. In the end, the whole team working on a project should agree on one syntax.

### Exercises:

* Compare your source code against the author's [source code](https://bit.ly/3Slryyv).
  * Recap all the [source code changes from this section](https://bit.ly/3xIbSNE).
  * Optional: If you are using TypeScript, check out the author's source code [here](https://bit.ly/3UASg86).
* Alternative: Check out this little [helper library](https://bit.ly/3SBwGOT) which simplifies error handling without using try/catch.
* Read more about [data fetching in React](https://www.robinwieruch.de/react-hooks-fetch-data/).
* Optional: [Leave feedback for this section](https://forms.gle/mtMmwrrsiwioZ8GH6).