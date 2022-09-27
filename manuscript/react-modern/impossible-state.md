## React Impossible States

Perhaps you've noticed a disconnect between the single states in the App component when using multiple of React's useState Hooks. Technically, all states related to the asynchronous data belong together, which doesn't only include the stories as actual data, but also their loading and error states. That's where one reducer and React's useReducer Hook come into play to manage domain related states. But why should we care?

There is nothing wrong with multiple `useState` hooks in one React component. Be wary once you see multiple state updater functions in a row, however. These conditional states can lead to **impossible states** and undesired behavior in the UI. Try changing your pseudo data fetching function to the following implementation to simulate an error and thus our implementation of error handling:

{title="src/App.jsx",lang="javascript"}
~~~~~~~
const getAsyncStories = () =>
  new Promise((resolve, reject) => setTimeout(reject, 2000));
~~~~~~~

The impossible state happens when an error occurs for the asynchronous data. The state for the error is set, but the state for the loading indicator isn't revoked. In the UI, this would lead to an infinite loading indicator and an error message, though it may be better to show the error message only and hide the loading indicator. Impossible states are not easy to spot, which makes them infamous for causing bugs in the UI. You could go on and try yourself to fix this bug.

Fortunately, we can improve our chances of not dealing with such bugs by moving states that belong together from multiple `useState` (and `useReducer`) hooks into a single `useReducer` hook. Take the following hooks:

{title="src/App.jsx",lang="javascript"}
~~~~~~~
const App = () => {
  ...

  const [stories, dispatchStories] = React.useReducer(
    storiesReducer,
    []
  );
  const [isLoading, setIsLoading] = React.useState(false);
  const [isError, setIsError] = React.useState(false);

  ...
};
~~~~~~~

And merge them into one `useReducer` hook for a unified state management which encompasses a more complex state object and eventually more complex state transitions:

{title="src/App.jsx",lang="javascript"}
~~~~~~~
const App = () => {
  ...

  const [stories, dispatchStories] = React.useReducer(
    storiesReducer,
# leanpub-start-insert
    { data: [], isLoading: false, isError: false }
# leanpub-end-insert
  );

  ...
};
~~~~~~~

Since we cannot use the state updater functions from React's useState Hooks anymore, everything related to asynchronous data fetching must use the new dispatch function for the state transitions. The most straightforward approach is exchanging the state updater function with the dispatch function. Then the dispatch function receives a distinct `type` and a `payload`. The latter resembles the same payload that we would have used to update the state with a state updater function:

{title="src/App.jsx",lang="javascript"}
~~~~~~~
const App = () => {
  ...

  const [stories, dispatchStories] = React.useReducer(
    storiesReducer,
    { data: [], isLoading: false, isError: false }
  );

  React.useEffect(() => {
# leanpub-start-insert
    dispatchStories({ type: 'STORIES_FETCH_INIT' });
# leanpub-end-insert

    getAsyncStories()
      .then((result) => {
# leanpub-start-insert
        dispatchStories({
          type: 'STORIES_FETCH_SUCCESS',
          payload: result.data.stories,
        });
# leanpub-end-insert
      })
      .catch(() =>
# leanpub-start-insert
        dispatchStories({ type: 'STORIES_FETCH_FAILURE' })
# leanpub-end-insert
      );
  }, []);

  ...
};
~~~~~~~

We changed two things for the reducer function. First, we introduced new types when we called the dispatch function from the outside. Therefore we need to add new cases for state transitions. And second, we changed the state structure from an array to a complex object. Therefore we need to take the new complex object into account as incoming state and returned state:

{title="src/App.jsx",lang="javascript"}
~~~~~~~
const storiesReducer = (state, action) => {
  switch (action.type) {
# leanpub-start-insert
    case 'STORIES_FETCH_INIT':
      return {
        ...state,
        isLoading: true,
        isError: false,
      };
    case 'STORIES_FETCH_SUCCESS':
      return {
        ...state,
        isLoading: false,
        isError: false,
        data: action.payload,
      };
    case 'STORIES_FETCH_FAILURE':
      return {
        ...state,
        isLoading: false,
        isError: true,
      };
    case 'REMOVE_STORY':
      return {
        ...state,
        data: state.data.filter(
          (story) => action.payload.objectID !== story.objectID
        ),
      };
# leanpub-end-insert
    default:
      throw new Error();
  }
};
~~~~~~~

For every state transition, we return a *new state* object which contains all the key/value pairs from the *current state* object (via JavaScript's spread operator) and the new overwriting properties. For example, `STORIES_FETCH_FAILURE` sets the `isLoading` boolean to `false` and sets the `isError` boolean to `true`, while keeping all the the other state intact (e.g. `data` alias `stories`). That's how we get around the bug introduced earlier as impossible state since an error should set the loading boolean to `false`.

Observe how the `REMOVE_STORY` action changed as well. It operates on the `state.data`, and no longer just on the plain `state`. The state is a complex object with `data`, `isLoading`, and `error` states rather than just a list of stories. This has to be solved in the remaining code by addressing the state as object and not as array anymore:

{title="src/App.jsx",lang="javascript"}
~~~~~~~
const App = () => {
  ...

  const [stories, dispatchStories] = React.useReducer(
    storiesReducer,
    { data: [], isLoading: false, isError: false }
  );

  ...

# leanpub-start-insert
  const searchedStories = stories.data.filter((story) =>
# leanpub-end-insert
    story.title.toLowerCase().includes(searchTerm.toLowerCase())
  );

  return (
    <div>
      ...

# leanpub-start-insert
      {stories.isError && <p>Something went wrong ...</p>}
# leanpub-end-insert

# leanpub-start-insert
      {stories.isLoading ? (
# leanpub-end-insert
        <p>Loading ...</p>
      ) : (
        <List
          list={searchedStories}
          onRemoveItem={handleRemoveStory}
        />
      )}
    </div>
  );
};
~~~~~~~

Try to use the erroneous data fetching function again and check whether everything works as expected now:

{title="src/App.jsx",lang="javascript"}
~~~~~~~
const getAsyncStories = () =>
  new Promise((resolve, reject) => setTimeout(reject, 2000));
~~~~~~~

We moved from unreliable state transitions with multiple `useState` hooks to predictable state transitions with React's useReducer Hook. The state object managed by the reducer encapsulates everything related to the fetching of stories including loading and error states, but also implementation details like removing a story from the stories. We didn't get fully rid of impossible states, because it's still possible to leave out a crucial boolean flag like before, but we moved one step closer towards more predictable state management.

### Exercises:

* Compare your source code against the author's [source code](https://bit.ly/3R97YnT).
  * Recap all the [source code changes from this section](https://bit.ly/3xIh9oF).
  * Optional: If you are using TypeScript, check out the author's source code [here](https://bit.ly/3Ceq83w).
* Read more about [when to use useState or useReducer in React](https://www.robinwieruch.de/react-usereducer-vs-usestate/).
* Read more about [deriving state from props in React](https://www.robinwieruch.de/react-derive-state-props/).
* Optional: [Leave feedback for this section](https://forms.gle/XWTJS65iu6WkiZMCA).