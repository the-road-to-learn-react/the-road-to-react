## React Conditional Rendering

We are set to introduce a new feature associated with the recently introduced asynchronous data handling. In a genuine application, users typically receive feedback, such as a loading spinner, while data is being loaded. In this section, our goal is to implement this feedback mechanism. Feel free to attempt implementing it independently, and later, you can refer to the book to compare your solution with the provided implementation.

**Task:** It takes some time to load the sample data from the promise. During this time, a user should be presented with a loading indicator in its simplest form (e.g. text which says "Loading ..."). Once the data arrived asynchronously, hide the loading indicator.

**Optional Hints:**

* In order to show a loading indicator, one would need to introduce a new stateful value. A boolean called `isLoading` may be the best solution.
* When the side-effect which loads the data kicks in, set the stateful boolean to `true`. Once the data loaded, set the stateful boolean to false again.
* In JSX, show a "Loading ..." text conditionally when the `isLoading` boolean is set to `true`.

A **conditional rendering** in React always happens if we have to render different JSX based on information (e.g. state, props). Dealing with asynchronous data is a good use case for making use of conditional rendering. For example, when the application initializes for the first time, there is no data to start with. Next, we are loading data and eventually, we have the data at our hands to display it. Sometimes the data fetching fails and we receive an error instead. So there are lots of things to cover for us as developers.

Fortunately, some aspects are already handled. For example, because the initial state is an empty list `[]` instead of `null`, concerns about breaking the application when filtering or mapping over this list are alleviated. Yet, certain aspects still need attention. Consider the absence of a loading state to provide users with feedback on pending data requests. Introducing a new stateful value can address this, allowing us to set the state accordingly when data is being fetched:

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

* Compare your source code against the author's [source code](https://github.com/the-road-to-learn-react/hacker-stories/tree/2025_conditional-rendering).
  * Recap all the [source code changes](https://github.com/the-road-to-learn-react/hacker-stories/compare/2025_async-data...2025_conditional-rendering) from this section.
  * Optional: If you are using TypeScript, check out the author's source code [here](https://bit.ly/3Ss5RP1).
* Read more about [conditional rendering in React](https://www.robinwieruch.de/conditional-rendering-react/).

### Interview Questions:

* Question: Why didn't we need a conditional rendering for the empty `stories` before they get fetched from the fake API?
  * Answer: The `stories` are mapped as list in the List component by using the `map()` method. When mapping over a list, the `map()` method returns for every item a modified version (in our case JSX). If there are no items in the list, the `map()` method will return nothing. Therefore we do not need a conditional rendering here.
* Question: What would happen if the initial state of `stories` would be set to `null` instead of `[]`?
  * Answer: Then we would need a conditional rendering in the List component, because calling `map()` on `null` would throw an exception.
* Question: How can you conditionally render content based on state using useState?
  * Answer: Use conditional statements (e.g., if or ternary operator) in JSX based on the state value.
* Question: Can you use useState (or other hooks) inside conditional statements or loops?
  * Answer: No, hooks must be used at the top level of a function component, not within conditions or loops.
* Question: Why is conditional rendering often employed when handling asynchronous data in React?
  * Answer: Conditional rendering helps manage the UI display based on the state of asynchronous data, showing loading indicators or actual content as needed.
* Question: How do you handle loading states while waiting for asynchronous data in React?
  * Answer: Loading states are managed using conditional rendering or state variables, indicating to users that data is being fetched.
* Question: Can you handle errors during asynchronous data fetching in React?
  * Answer: Yes, error handling mechanisms, such as try...catch blocks or .catch with promises, can be implemented to manage errors during data fetching.