## Async/Await in React

There is no way around asynchronous data when working on real world applications. There will always be a remote API that gives you data, whether it is on the frontend or backend, so you need to understand how to work with this data asynchronously. In our React application, we have started to resolve promises with then/catch blocks. However, in modern JavaScript (and therefore React), a more popular solution is using async/await.

If you are already familiar with async/await or you want to explore [its usage](https://mzl.la/3AWyWaw) yourself, go ahead and change the code from using then/catch to async/await. If you have come this far, you could also consider compensating for the removal of the catch block for the error handling by using a try/catch block instead.

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

* Compare your source code against the author's [source code](https://github.com/the-road-to-learn-react/hacker-stories/tree/2025_async-await).
  * Recap all the [source code changes](https://github.com/the-road-to-learn-react/hacker-stories/compare/2025_third-party-libraries...2025_async-await) from this section.
  * Optional: If you are using TypeScript, check out the author's source code [here](https://bit.ly/3wdPFJq).
* Read more about [data fetching in React](https://www.robinwieruch.de/react-hooks-fetch-data/).

### Interview Questions:

* Question: What is async/await?
  * Answer: async and await are keywords in JavaScript used for handling asynchronous operations in a synchronous-like manner, making code more readable.
* Question: How do you use async/await with a function in React?
  * Answer: Declare the function with the async keyword and use await within the function to handle promises.
* Question: What is the purpose of async functions in React?
  * Answer: async functions allow you to work with asynchronous code in a more readable and sequential manner, enhancing the handling of promises.
* Question: How do you handle errors with async/await in React?
  * Answer: Use a try/catch block to catch and handle errors in an async function.
* Question: How does async/await differ from using .then() with Promises in React?
  * Answer: async/await provides a more concise syntax, making asynchronous code look similar to synchronous code, compared to chaining .then().
* Question: Can you use async/await with the Fetch API in React?
  * Answer: Yes, async/await is commonly used with the Fetch API for asynchronous data fetching in React.
* Question: How do you handle multiple asynchronous operations with async/await in React?
  * Answer: Use Promise.all() to handle multiple asynchronous operations concurrently in an async function.