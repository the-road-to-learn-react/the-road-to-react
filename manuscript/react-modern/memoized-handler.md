## Memoized Functions in React (Advanced)

Most often, functions defined in React components serve as event handlers. However, given that a React component is essentially a function itself, you can also declare functions, function expressions, and arrow function expressions within a component. This section introduces the concept of a memoized function using React's useCallback Hook.

To begin, we'll proactively refactor the code to incorporate a memoized function, followed by detailed explanations. The refactoring involves transferring all data fetching logic from the side-effect into an arrow function expression (A). This new function is then encapsulated within React's `useCallback` hook (B) and subsequently invoked within the `useEffect` hook (C):

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

At its core, the application behaves the same, because we have only extracted a new function from React's useEffect Hook. Instead of using the data fetching logic directly in the side-effect, we made it available as a function for the entire application. The benefit: reusability. The data fetching can be used by other parts of the application by calling this new function. However, we have used React's useCallback Hook to wrap the extracted function, so let's explore why it's needed here. React's useCallback Hook creates a memoized function every time its dependency array (E) changes. As a result, the `useEffect` hook runs again (C), because it depends on the new function (D):

{title="Visualization",lang="javascript"}
~~~~~~~
1. change: searchTerm (cause: user interaction)
2. change: handleFetchStories (cause: changed searchTerm)
3. run: side-effect (cause: changed handleFetchStories)
~~~~~~~

If we would leave out React's `useCallback` Hook and only define the new `handleFetchStories` event handler without it, a new `handleFetchStories` function would be created each time the App component re-renders, and would be executed in the `useEffect` hook to fetch data. The fetched data is then stored as state in the component. Then, because the state of the component changed, the component re-renders and creates a new `handleFetchStories` function. The side-effect would be triggered to fetch data, and we'd be stuck in an endless loop:

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

You could try this infinite loop yourself by removing React's useCallback Hook, but be prepared for a crashing browser. After all, React's `useCallback` hook changes the function only when one of its values in the dependency array changes. That's when we want to trigger a re-fetch of the data, because the input field has new input and we want to see the new data displayed in our list.

By moving the data fetching function outside the React's useEffect Hook, it becomes reusable for other parts of the application. We won't use it just yet, but it is a good use case to understand the memoized functions in React. Now the `useEffect` hook runs implicitly when the `searchTerm` changes, because the `handleFetchStories` is re-defined each time the `searchTerm` changes. Since the `useEffect` hook depends on the `handleFetchStories`, the side-effect for data fetching runs again.

### Exercises:

* Compare your source code against the author's [source code](https://bit.ly/422ZA0k).
  * Recap all the [source code changes from this section](https://bit.ly/3vFwrMT).
  * Optional: If you are using TypeScript, check out the author's source code [here](https://bit.ly/49Fo6Yj).
* Read more about [React's useCallback Hook](https://www.robinwieruch.de/react-usecallback-hook/).
* Optional: [Leave feedback for this section](https://forms.gle/HSX9aurgsf5j76HR9).

### Interview Questions:

* Question: What is the purpose of the useCallback hook in React?
  * Answer: useCallback is used to memoize functions in React, preventing unnecessary re-creations of functions on re-renders.
* Question: When should you use useCallback in a React component?
  * Answer: Use useCallback when you want to memoize a function to optimize performance, especially in scenarios involving callback functions passed to child components.
* Question: What are the arguments of the useCallback hook?
  * Answer: The first argument is the function to be memoized, and the second argument is an array of dependencies that, when changed, trigger the creation of a new memoized function.
* Question: What happens if the dependencies array in useCallback is empty?
  * Answer: If the dependencies array is empty, the memoized function is created only once and remains the same throughout the component's lifecycle.