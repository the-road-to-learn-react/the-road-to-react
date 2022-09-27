## React Side-Effects

A React component's returned output is defined by its props and state. Side-effects can affect this output too, because they are used to interact with third-party APIs (e.g. browser's localStorage API, remote APIs for data fetching), with HTML elements for width and height measurements, or with built-in JavaScript functions such as timers or intervals. These are only a few examples of side-effects in React components and we will get to apply one of these examples next.

At the moment, whenever you search for a term in our application you will get the result. However, once you close the browser and open it again, the search term isn't there anymore. Wouldn't it be a great user experience if our Search component could remember the most recent search, so that the application displays it in the browser whenever it restarts?

Let's implement this feature by using a side-effect to store the recent search from the browser's local storage and retrieve it upon the initial component initialization. First, use the local storage to store the `searchTerm` accompanied by an identifier whenever a user types into the HTML input field:

{title="src/App.jsx",lang="javascript"}
~~~~~~~
const App = () => {
  ...

  const handleSearch = (event) => {
    setSearchTerm(event.target.value);

# leanpub-start-insert
    localStorage.setItem('search', event.target.value);
# leanpub-end-insert
  };

  ...
);
~~~~~~~

Second, use the stored value, if a value exists, to set the initial state of the `searchTerm` in React's useState Hook. Otherwise, default to our initial state (here: "React") as before:

{title="src/App.jsx",lang="javascript"}
~~~~~~~
const App = () => {
  ...

  const [searchTerm, setSearchTerm] = React.useState(
# leanpub-start-insert
    localStorage.getItem('search') || 'React'
# leanpub-end-insert
  );

  ...
);
~~~~~~~

Good to know: [JavaScript's logical OR operator](https://mzl.la/3aXxryd) returns the truthy operand in this expression and is short-circuited if `localStorage.getItem('search')` returns a truthy value. It's used as a shorthand for the following implementation for setting default values:

{title="Code Playground",lang="javascript"}
~~~~~~~
let hasStored;
if (localStorage.getItem('search')) {
  hasStored = true;
} else {
  hasStored = false;
}

const initialState = hasStored
  ? localStorage.getItem('search')
  : 'React';
~~~~~~~

When using the input field and refreshing the browser tab, the browser should remember the latest search term now. Essentially we synchronized the browser's local storage with React's state: While we initialize the state with the browser's local storage's value (or a fallback), we write the new value  when the handler is called to the browser's storage and the component's state.

The feature is complete, but there is one flaw that may introduce bugs in the long run: The handler function should mostly be concerned with updating the state, but it has a side-effect now. The flaw: If we use the `setSearchTerm` state updater function somewhere else in our application, we break the feature because the local storage doesn't get updated. Let's fix this by handling the side-effect at a centralized place and not in a specific handler. We'll use **React's useEffect Hook** to trigger the desired side-effect each time the `searchTerm` changes:

{title="src/App.jsx",lang="javascript"}
~~~~~~~
const App = () => {
  ...

  const [searchTerm, setSearchTerm] = React.useState(
    localStorage.getItem('search') || 'React'
  );

# leanpub-start-insert
  React.useEffect(() => {
    localStorage.setItem('search', searchTerm);
  }, [searchTerm]);
# leanpub-end-insert

# leanpub-start-insert
  const handleSearch = (event) => {
    setSearchTerm(event.target.value);
  };
# leanpub-end-insert

  ...
);
~~~~~~~

React's useEffect Hook takes two arguments: The first argument is a function that runs our side-effect. In our case, the side-effect stores `searchTerm` into the browser's local storage. The second argument is a dependency array of variables. If one of these variables changes, the function for the side-effect is called. In our case, the function is called every time the `searchTerm` changes (e.g. when a user types into the HTML input field). In addition, it's also called initially when the component renders for the first time.

Leaving out the second argument (the dependency array) would make the function for the side-effect run on every render (initial render and update renders) of the component. If the dependency array of React's useEffect is an empty array, the function for the side-effect is only called once when the component renders for the first time. After all, the hook lets us opt into React's component lifecycle. It can be triggered when the component is first mounted, but also if one of its values (state, props, derived values from state/props) is updated.

In conclusion, using React `useEffect` Hook instead of managing the side-effect in the (event) handler has made the application more robust. *Whenever* and *wherever* the `searchTerm` state is updated via `setSearchTerm`, the browser's local storage will always be in sync with it.

### Exercises:

* Compare your source code against the author's [source code](https://bit.ly/3LvTSvT).
  * Recap all the [source code changes from this section](https://bit.ly/3BEcchA).
  * Optional: If you are using TypeScript, check out the author's source code [here](https://bit.ly/3DXpQzt).
* Read more about [React's useEffect Hook](https://www.robinwieruch.de/react-useeffect-hook/).
  * Give the first argument's function a `console.log()` and experiment with React's useEffect Hook's dependency array. Check the logs for an empty dependency array too.
* Read more about [using local storage with React](https://www.robinwieruch.de/local-storage-react/).
* Try the following scenario: In your browser, backspace the search term from the input field until nothing is left there. Internally, it should be set to an empty string now. Next, refresh the browser and check what it displays. You may be wondering why it does show "React" instead of "", because "" should be the recent search. That's because JavaScript's logical OR evaluates "" to false and thus takes "React" as the true value. If you want to prevent this and evaluate "" as true instead, you may want to exchange JavaScript's logical OR operator || with [JavaScript's nullish coalescing operator ??](https://mzl.la/2Z4bsU4).
* Optional: [Leave feedback for this section](https://forms.gle/iCtVZHYt2XRNfAcBA).