## React Conditional Rendering

A **conditional rendering** in React always happens if we have to render different JSX based on state or props. Dealing with asynchronous data is a good use case for dealing with these conditional states. For example, when the application initializes for the first time, there is no data to start with. Next, we are loading data and eventually, we have the data at our disposal to display it. Sometimes the data fetching fails and we receive an error instead. So there are lots of things to cover for us as developers.

Fortunately, a few of these cases are already taken care of. For instance, because our initial state is an empty list rather than `null`, we don't have to worry that this breaks the application when we filter and map over this list. However, some things are still missing. For example, let's introduce one more state for a loading indicator which gives our users feedback about the pending data request:

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

    getAsyncStories().then((result) => {
      setStories(result.data.stories);
# leanpub-start-insert
      setIsLoading(false);
# leanpub-end-insert
    });
  }, []);

  ...
};
~~~~~~~

With [JavaScript's ternary operator](https://mzl.la/3vAPKCL), we can inline this conditional state as a **conditional rendering** in JSX:

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

Asynchronous data comes with error handling, too. It doesn't happen in our simulated environment, but there could be errors if we start fetching data from another third-party API. Introduce another state for error handling and handle it in the promise's `catch()` block when resolving the promise:

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
      .then((result) => {
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

Next, give the user feedback in case something goes wrong with another conditional rendering. This time, it's either rendering something or nothing. So instead of having a ternary operator where one side returns `null`, use the logical `&&` operator as shorthand:

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

Conditional rendering is not just for asynchronous data though. The simplest example of conditional rendering is a boolean flag state that's toggled with a button. If the boolean flag is true, render something, if it is false, don't render anything. Knowing about this feature in React can be quite powerful, because it gives you the ability to conditionally render JSX. It's yet another tool in React to make your UI more dynamic. And as we've discovered, it's often necessary for more complex control flows like asynchronous data.

### Exercises:

* Confirm your [source code](https://bit.ly/2ZfIJLM).
  * Confirm the [changes](https://bit.ly/3AYmneE).
* Read more about [conditional rendering in React](https://www.robinwieruch.de/conditional-rendering-react/).
* Optional: [Leave feedback for this section](https://forms.gle/kHLAXtMaKsTFtWjY9).