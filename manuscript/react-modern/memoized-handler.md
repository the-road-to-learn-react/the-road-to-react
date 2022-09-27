## Memoized Functions in React (Advanced)

Functions that are defined in a React components are most of the time event handlers. However, because a React component is just a function itself, you can declare functions, function expressions, and arrow function expressions in a component too. In this section, we will introduce the concept of a **memoized function** by using React's useCallback Hook.

We will refactor the code upfront to use a memoized function and provide the explanations afterward. The refactoring consists of moving all the data fetching logic from the side-effect into a arrow function expression (A), wrapping this new function into React's `useCallback` hook (B), and invoking it in the `useEffect` hook (C):

{title="src/App.jsx",lang="javascript"}
~~~~~~~
const App = () => {
  ...

  // A
# leanpub-start-insert
  const handleFetchStories = React.useCallback(() => { // B
# leanpub-end-insert
    if (!searchTerm) return;

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
  }, [searchTerm]); // E
# leanpub-end-insert

  React.useEffect(() => {
# leanpub-start-insert
    handleFetchStories(); // C
  }, [handleFetchStories]); // D
# leanpub-end-insert

  ...
};
~~~~~~~

At its core, the application behaves the same, because we have only extracted a function from React's useEffect Hook. Instead of using the data fetching logic directly in the side-effect, we made it available as a function for the entire application. The benefit: reusability. The data fetching can be used by other parts of the application by calling this new function.

However, we have used React's useCallback Hook to wrap the extracted function, so let's explore why it's needed here. React's useCallback Hook creates a memoized function every time its dependency array (E) changes. As a result, the `useEffect` hook runs again (C), because it depends on the new function (D):

{title="Visualization",lang="javascript"}
~~~~~~~
1. change: searchTerm (cause: user interaction)
2. change: handleFetchStories
3. run: side-effect
~~~~~~~

If we didn't create a memoized function with React's `useCallback` Hook, a new `handleFetchStories` function would be created each time the App component re-renders, and would be executed in the `useEffect` hook to fetch data. The fetched data is then stored as state in the component. Because the state of the component changed, the component re-renders and creates a new `handleFetchStories` function. The side-effect would be triggered to fetch data, and we'd be stuck in an endless loop:

{title="Visualization",lang="javascript"}
~~~~~~~
1. define: handleFetchStories
2. run: side-effect
3. update: state
4. re-render: component
5. re-define: handleFetchStories
6. run: side-effect
...
~~~~~~~

React's `useCallback` hook changes the function only when one of its values in the dependency array changes. That's when we want to trigger a re-fetch of the data, because the input field has new input and we want to see the new data displayed in our list.

By moving the data fetching function outside the React's useEffect Hook, it becomes reusable for other parts of the application. We won't use it just yet, but it is a good use case to understand the memoized functions in React. Now the `useEffect` hook runs implicitly when the `searchTerm` changes, because the `handleFetchStories` is re-defined each time the `searchTerm` changes. Since the `useEffect` hook depends on the `handleFetchStories`, the side-effect for data fetching runs again.

### Exercises:

* Compare your source code against the author's [source code](https://bit.ly/3St2K7T).
  * Recap all the [source code changes from this section](https://bit.ly/3DIE4Uu).
  * Optional: If you are using TypeScript, check out the author's source code [here](https://bit.ly/3BShUNj).
* Read more about [React's useCallback Hook](https://www.robinwieruch.de/react-usecallback-hook/).
* Optional: [Leave feedback for this section](https://forms.gle/HSX9aurgsf5j76HR9).