## React Conditional Rendering

Handling asynchronous data in React leaves us with conditional states: with data and without data. This case is already covered, though, because our initial state is an empty list rather than `null`. If it was null, we'd have to handle this issue in our JSX. However, since it's `[]`, we `filter()` over an empty array for the search feature, which leaves us with an empty array. This leads to rendering nothing in the List component's `map()` function.

In a real world application, there are more than two conditional states for asynchronous data, though. Consider showing users a loading indicator when data loading is delayed:

{title="src/App.js",lang="javascript"}
~~~~~~~
const App = () => {
  ...

  const [stories, setStories] = React.useState([]);
# leanpub-start-insert
  const [isLoading, setIsLoading] = React.useState(false);
# leanpub-end-insert

  React.useEffect(() => {
# leanpub-start-insert
    setIsLoading(true);
# leanpub-end-insert

    getAsyncStories().then(result => {
      setStories(result.data.stories);
# leanpub-start-insert
      setIsLoading(false);
# leanpub-end-insert
    });
  }, []);

  ...
};
~~~~~~~

With [JavaScript's ternary operator](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Conditional_Operator), we can inline this conditional state as a **conditional rendering** in JSX:

{title="src/App.js",lang="javascript"}
~~~~~~~
const App = () => {
  ...

  return (
    <div>
      ...

      <hr />

# leanpub-start-insert
      {isLoading ? (
        <p>Loading ...</p>
      ) : (
# leanpub-end-insert
        <List
          list={searchedStories}
          onRemoveItem={handleRemoveStory}
        />
# leanpub-start-insert
      )}
# leanpub-end-insert
    </div>
  );
};
~~~~~~~

Asynchronous data comes with error handling, too. It doesn't happen in our simulated environment, but there could be errors if we start fetching data from another third-party API. Introduce another state for error handling and solve it in the promise's `catch()` block when resolving the promise:

{title="src/App.js",lang="javascript"}
~~~~~~~
const App = () => {
  ...

  const [stories, setStories] = React.useState([]);
  const [isLoading, setIsLoading] = React.useState(false);
# leanpub-start-insert
  const [isError, setIsError] = React.useState(false);
# leanpub-end-insert

  React.useEffect(() => {
    setIsLoading(true);

    getAsyncStories()
      .then(result => {
        setStories(result.data.stories);
        setIsLoading(false);
      })
# leanpub-start-insert
      .catch(() => setIsError(true));
# leanpub-end-insert
  }, []);

  ...
};
~~~~~~~

Next, give the user feedback in case something went wrong with another conditional rendering. This time, it's either rendering something or nothing. So instead of having a ternary operator where one side returns `null`, use the logical `&&` operator as shorthand:

{title="src/App.js",lang="javascript"}
~~~~~~~
const App = () => {
  ...

  return (
    <div>
      ...

      <hr />

# leanpub-start-insert
      {isError && <p>Something went wrong ...</p>}
# leanpub-end-insert

      {isLoading ? (
        <p>Loading ...</p>
      ) : (
        ...
      )}
    </div>
  );
};
~~~~~~~

In JavaScript, a `true && 'Hello World'` always evaluates to 'Hello World'. A `false && 'Hello World'` always evaluates to false. In React, we can use this behaviour to our advantage. If the condition is true, the expression after the logical `&&` operator will be the output. If the condition is false, React ignores it and skips the expression.

Conditional rendering is not just for asynchronous data though. The simplest example of conditional rendering is a boolean flag state that's toggled with a button. If the boolean flag is true, render something, if it is false, don't render anything.

This feature can be quite powerful, because it gives you the ability to conditionally render JSX. It's yet another tool in React to make your UI more dynamic. And as we've discovered, it's often necessary for more complex control flows like asynchronous data.

### Exercises:

* Confirm your [source code for the last section](https://codesandbox.io/s/github/the-road-to-learn-react/hacker-stories/tree/hs/React-Conditional-Rendering).
  * Confirm the [changes from the last section](https://github.com/the-road-to-learn-react/hacker-stories/compare/hs/React-Asynchronous-Data...hs/React-Conditional-Rendering?expand=1).
* Read more about [conditional rendering in React](https://www.robinwieruch.de/conditional-rendering-react/).