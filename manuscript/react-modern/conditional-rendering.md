## React Conditional Rendering

We will implement a new feature related to the just recently introduced asynchronous data. In a real application, a user would see some kind of feedback (e.g. loading spinner) when data gets loaded. In this section, we want to implement such feedback. Again you can try to implement it yourself first and later check out how the book implements this task.

**Task:** It takes some time to load the sample data from the promise. During this time, a user should be presented with a loading indicator in its simplest form (e.g. text which says "Loading ..."). Once the data arrived asynchronously, hide the loading indicator.

**Optional Hints:**

* In order to show a loading indicator, one would need to introduce a new stateful value. A boolean called `isLoading` may be the best solution.
* When the side-effect which loads the data kicks in, set the stateful boolean to `true`. Once the data loaded, set the stateful boolean to false again.
* In JSX, show a "Loading ..." text conditionally when the `isLoading` boolean is set to `true`.

A **conditional rendering** in React always happens if we have to render different JSX based on information (e.g. state, props). Dealing with asynchronous data is a good use case for making use of conditional rendering. For example, when the application initializes for the first time, there is no data to start with. Next, we are loading data and eventually, we have the data at our hands to display it. Sometimes the data fetching fails and we receive an error instead. So there are lots of things to cover for us as developers.

Fortunately, a few of these cases are already taken care of. For instance, because the initial state is an empty list `[]` rather than `null`, we don't have to worry that this breaks the application when we filter and map over this list. However, a few mentioned things are still missing. For example, what about a loading state which will show the user a loading indicator as feedback about the pending data request? We could introduce this as new stateful value and set the state accordingly when data gets fetched:

{title="src/App.jsx",lang="javascript"}
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

The boolean should be toggled properly now. What's missing is showing the user the loading indicator. A straightforward approach would be using an early return in the App component:

{title="src/App.jsx",lang="javascript"}
~~~~~~~
const App = () => {
  ...

  if (isLoading) {
    return <p>Loading ...</p>;
  }

  return (
    <div>
      ...
    </div>
  );
};
~~~~~~~

However, this way *only* the loading indicator would render and nothing else. Instead, we want to inline the loading indicator within the JSX to either show the loading indicator or the List component. Using an if-else statement inlined in JSX is not encouraged though due to JSX's limitations here. (You can try it as exercise though.) However, you can use a [ternary operator](https://mzl.la/3vAPKCL) instead and produce a **conditional rendering** in JSX this way:

{title="src/App.jsx",lang="javascript"}
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

That's already it. You are rendering conditionally a loading indicator or the List component based on a stateful boolean. Let's move on by implementing error handling for the asynchronous data too. An error doesn't happen in our simulated environment, but there could be errors if we start fetching data from a remote API. Therefore, introduce another state for error handling and handle it in the promise's `catch()` block when resolving the promise:

{title="src/App.jsx",lang="javascript"}
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

{title="src/App.jsx",lang="javascript"}
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

In JavaScript, a `true && 'Hello World'` always evaluates to 'Hello World'. A `false && 'Hello World'` always evaluates to false. In React, we can use this behaviour to our advantage. If the condition is true, the expression after the logical `&&` operator will be the output. If the condition is false, React ignores it and skips the expression. Using `expression && JSX` is more concise than using `expression ? JSX : null`.

Conditional rendering is not just for asynchronous data though. The simplest example of conditional rendering is a boolean state that's toggled with a button. If the boolean flag is true, render something, if it is false, don't render anything. Knowing about this feature in React can be quite powerful, because it gives you the ability to conditionally render JSX. It's yet another tool in React to make your UI more dynamic. And as we've discovered, it's often necessary for more complex control flows like asynchronous data.

### Exercises:

* Compare your source code against the author's [source code](https://bit.ly/3xKEdTD).
  * Recap all the [source code changes from this section](https://bit.ly/3fdJOf3).
  * Optional: If you are using TypeScript, check out the author's source code [here](https://bit.ly/3dQai5F).
* Read more about [conditional rendering in React](https://www.robinwieruch.de/conditional-rendering-react/).
* Question: Why didn't we need a conditional rendering for the empty `stories` before they get fetched from the fake API?
  * Answer: The `stories` are mapped as list in the List component by using the `map()` method. When mapping over a list, the `map()` method returns for every item a modified version (in our case JSX). If there are no items in the list, the `map()` method will return nothing. Therefore we do not need a conditional rendering here.
* Question: What would happen if the initial state of `stories` would be set to `null` instead of `[]`?
  * Answer: Then we would need a conditional rendering in the List component, because calling `map()` on `null` would throw an exception.
* Optional: [Leave feedback for this section](https://forms.gle/kHLAXtMaKsTFtWjY9).