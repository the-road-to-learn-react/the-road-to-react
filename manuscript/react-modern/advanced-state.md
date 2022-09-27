## React Advanced State

All state management in this application makes heavy use of React's useState Hook. On the other hand, React's **useReducer Hook** enables one to use more sophisticated state management for complex state structures and transitions. Since the knowledge about reducers in JavaScript splits the community in half, we won't cover the basics here. However, if you haven't heard about reducers before, check out this [guide about reducers in JavaScript](https://www.robinwieruch.de/javascript-reducer/).

In this section, we will move the stateful `stories` from React's `useState` hook to React's `useReducer` hook. Using `useReducer` over `useState` makes sense as soon as multiple stateful values are dependent on each other or related to one domain. For example, `stories`, `isLoading`, and `error` are all related to the data fetching. In a more abstract version, all three could be properties in a complex object (e.g. `data`, `isLoading`, `error`) managed by a reducer instead. We will cover this in a later section. In this section, we will start to manage the `stories` and its state transitions in a reducer.

First, introduce a reducer function outside of your components. A reducer function always receives a `state` and an `action`. Based on these two arguments, a reducer always returns a new state:

{title="src/App.jsx",lang="javascript"}
~~~~~~~
const getAsyncStories = () =>
  new Promise((resolve) => ... );

# leanpub-start-insert
const storiesReducer = (state, action) => {
  if (action.type === 'SET_STORIES') {
    return action.payload;
  } else {
    throw new Error();
  }
};
# leanpub-end-insert
~~~~~~~

A reducer `action` is always associated with a `type` and as a best practice with a `payload`. If the `type` matches a condition in the reducer, return a new state based on incoming state and action. If it isn't covered by the reducer, throw an error to remind yourself that the implementation isn't covered. The `storiesReducer` function covers one `type` and then returns the `payload` of the incoming action without using the current state to compute the new state. The new state is therefore simply the `payload`.

In the App component, exchange `useState` for `useReducer` for managing the `stories`. The new hook receives a reducer function and an initial state as arguments and returns an array with two items. The first item is the *current state* and the second item is the *state updater function* (also called *dispatch function*):

{title="src/App.jsx",lang="javascript"}
~~~~~~~
const App = () => {
  ...

# leanpub-start-insert
  const [stories, dispatchStories] = React.useReducer(
    storiesReducer,
    []
  );
# leanpub-end-insert

  ...
};
~~~~~~~

The new dispatch function can be used instead of the `setStories` function, which was previously returned from `useState`. Instead of setting the state explicitly with the state updater function from `useState`, the `useReducer` state updater function sets the state implicitly by dispatching an action for the reducer. The action comes with a `type` and an optional `payload`:

{title="src/App.jsx",lang="javascript"}
~~~~~~~
const App = () => {
  ...

  React.useEffect(() => {
    setIsLoading(true);

    getAsyncStories()
      .then((result) => {
# leanpub-start-insert
        dispatchStories({
          type: 'SET_STORIES',
          payload: result.data.stories,
        });
# leanpub-end-insert
        setIsLoading(false);
      })
      .catch(() => setIsError(true));
  }, []);

  const handleRemoveStory = (item) => {
    const newStories = stories.filter(
      (story) => item.objectID !== story.objectID
    );

# leanpub-start-insert
    dispatchStories({
      type: 'SET_STORIES',
      payload: newStories,
    });
# leanpub-end-insert
  };

  ...
};
~~~~~~~

The application appears the same in the browser, though a reducer and React's `useReducer` hook are managing the state for the stories now. Let's bring the concept of a reducer to a minimal version by handling more than one state transition. If there is only one state transition, a reducer wouldn't make sense.

So far, the `handleRemoveStory` handler computes the new stories. It's valid to move this logic into the reducer function and manage the reducer with an action, which is another case for moving from imperative to declarative programming. Instead of doing it ourselves by saying *how it should be done*, we are telling the reducer *what to do*. Everything else is hidden in the reducer:

{title="src/App.jsx",lang="javascript"}
~~~~~~~
const App = () => {
  ...

  const handleRemoveStory = (item) => {
# leanpub-start-insert
    dispatchStories({
      type: 'REMOVE_STORY',
      payload: item,
    });
# leanpub-end-insert
  };

  ...
};
~~~~~~~

Now the reducer function has to cover this new case in a new conditional state transition. If the condition for removing a story is met, the reducer has all the implementation details needed to remove the story. The action gives all the necessary information (here an item's identifier) to remove the story from the current state and return a new list of filtered stories as state:

{title="src/App.jsx",lang="javascript"}
~~~~~~~
const storiesReducer = (state, action) => {
  if (action.type === 'SET_STORIES') {
    return action.payload;
# leanpub-start-insert
  } else if (action.type === 'REMOVE_STORY') {
    return state.filter(
      (story) => action.payload.objectID !== story.objectID
    );
  } else {
# leanpub-end-insert
    throw new Error();
  }
};
~~~~~~~

All these if-else statements will eventually clutter when adding more state transitions into one reducer function. Refactoring it to a switch statement for all the state transitions makes it more readable and is seen as a best practice in the React community:

{title="src/App.jsx",lang="javascript"}
~~~~~~~
const storiesReducer = (state, action) => {
# leanpub-start-insert
  switch (action.type) {
    case 'SET_STORIES':
      return action.payload;
    case 'REMOVE_STORY':
      return state.filter(
        (story) => action.payload.objectID !== story.objectID
      );
    default:
      throw new Error();
  }
# leanpub-end-insert
};
~~~~~~~

What we've covered is a minimal version of a reducer in JavaScript and its usage in React with the help of React's useReducer Hook. The reducer covers two state transitions, shows how to compute the current state and action into a new state, and uses some business logic (removal of a story) for a state transition. Now we can set a list of stories as state for the asynchronously arriving data and remove a story from the list of stories with just one state managing reducer and its associated `useReducer` hook. To fully grasp the concept of reducers in JavaScript and the usage of React's useReducer Hook, visit the linked resources in the exercises. We will continue expanding our implementation of a reducer in the next section.

### Exercises:

* Compare your source code against the author's [source code](https://bit.ly/3Usx6sT).
  * Recap all the [source code changes from this section](https://bit.ly/3Lzbzus).
  * Optional: If you are using TypeScript, check out the author's source code [here](https://bit.ly/3RijH3Q).
* Read more about [reducers and useReducer in React](https://www.robinwieruch.de/react-usereducer-hook/).
* Extract the action types `'SET_STORIES'` and `'REMOVE_STORY'` as variables and reuse them in the reducer and the dispatch functions. This way, you will avoid introducing typos in your action types.
* Optional: [Leave feedback for this section](https://forms.gle/tNqqVynwQV9Ym9u68).