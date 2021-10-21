## React Side-Effects

A React component's returned output is defined by its props and state. In contrast, side-effects don't change this output directly (but can change it indirectly). They are used to interact with APIs outside of the component (e.g. browser's localStorage API, remote APIs for data fetching), measuring HTML element's width and height, or setting timers in JavaScript. These are only a few examples of side-effects in React components and we will get to know one of them in this section.

Wouldn't it be great if our Search component could remember the most recent search, so that the application opens it in the browser whenever it restarts? Let's implement this feature by using a side-effect to store the recent search from the browser's local storage and load it upon component initialization. First, use the local storage to store the `searchTerm` accompanied by an identifier whenever a user types into the HTML input field:

{title="src/App.js",lang="javascript"}
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

Second, use the stored value, if a value exists, to set the initial state of the `searchTerm` in React's useState Hook. Otherwise, default to our initial state (here "React") as before:

{title="src/App.js",lang="javascript"}
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

[JavaScript's logical OR operator ||](https://mzl.la/3aXxryd) returns the truthy operand in this expression and is short-circuited if `localStorage.getItem('search')` returns a truthy value. It's used as a shorthand for the following implementation:

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

When using the input field and refreshing the browser tab, the browser should remember the latest search term now. The feature is complete, but there is one flaw that may introduce bugs in the long run: The handler function should mostly be concerned with updating the state, but now it has this side-effect. If we use the `setSearchTerm` function elsewhere in our application, we may break the feature we implemented because we cannot enforce that the local storage will also get updated. Let's fix this by handling the side-effect at a centralized place. We'll use **React's useEffect Hook** to trigger the side-effect each time the `searchTerm` changes:

{title="src/App.js",lang="javascript"}
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

React's useEffect Hook takes two arguments: The first argument is a function that runs our side-effect. In our case, the side-effect stores `searchTerm` into the browser's local storage. The second argument is a dependency array of variables. If one of these variables changes, the function for the side-effect is called. In our case, the function is called every time the `searchTerm` changes (e.g. when a user types into the HTML input field); and it's also called initially when the component renders for the first time.

Leaving out the second argument (the dependency array) would make the function for the side-effect run on every render (initial render and update renders) of the component. If the dependency array of React's useEffect is an empty array, the function for the side-effect is only called once, after the component renders for the first time. After all, the hook lets us opt into React's component lifecycle. It can be triggered when the component is first mounted, but also if one of its dependencies is updated.

In conclusion, using React `useEffect` Hook instead of managing the side-effect in the (event) handler has made the application more robust. *Whenever* and *wherever* the `searchTerm` state is updated via `setSearchTerm`, the browser's local storage will always be in sync with it.

### Exercises:

* Confirm your [source code](https://bit.ly/3jj9TbC).
  * Confirm the [changes](https://bit.ly/3E12iGK).
* Read more about [React's useEffect Hook](https://www.robinwieruch.de/react-useeffect-hook).
* Read more about [using local storage with React](https://www.robinwieruch.de/local-storage-react).
* Give the first argument's function a `console.log()` and experiment with React's useEffect Hook's dependency array. Check the logs for an empty dependency array too.
* Try the following scenario: In your browser, backspace the search term from the input field until nothing is left there. Internally, it should be set to an empty string now. Next, refresh the browser and check what it displays. You may be wondering why it does show "React" instead of "", because "" should be the recent search. That's because JavaScript's logical OR evaluates "" to false and thus takes "React" as the true value. If you want to prevent this and evaluate "" as true instead, you may want to exchange JavaScript's logical OR operator || with [JavaScript's nullish coalescing operator ??](https://mzl.la/2Z4bsU4).
* Optional: [Leave feedback for this section](https://forms.gle/iCtVZHYt2XRNfAcBA).