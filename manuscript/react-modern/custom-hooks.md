## React Custom Hooks (Advanced)

Thus far we've covered the two most popular hooks in React: useState and useEffect. useState is used for variables that change over time; useEffect is used to opt into the lifecycle of your components to introduce side-effects. We'll eventually cover more hooks that come with React, but next, we'll tackle **React custom Hooks**; that is, building a hook yourself.

We will use the two hooks we already possess to create a new custom hook called `useSemiPersistentState`, named as such because it manages state yet synchronizes with the local storage. It's not fully persistent because clearing the local storage of the browser deletes relevant data for this application. We will start with how we want to use the hook in our App component:

{title="src/App.js",lang="javascript"}
~~~~~~~
const App = () => {
  const stories = [ ... ];

# leanpub-start-insert
  const [searchTerm, setSearchTerm] = useSemiPersistentState('React');
# leanpub-end-insert

  const handleSearch = (event) => {
    setSearchTerm(event.target.value);
  };

  const searchedStories = stories.filter((story) =>
    story.title.toLowerCase().includes(searchTerm.toLowerCase())
  );

  return (
    ...
  );
};
~~~~~~~

Instead of using React's built-in useState Hook, we want to use this custom hook now. Under the hood, we want that this hook synchronizes the state with the browser's local storage. If you look closely at the App component now, you can see that none of the previously introduced local storage features are there anymore. That's because we will copy this functionality over to our new custom hook:

{title="src/App.js",lang="javascript"}
~~~~~~~
# leanpub-start-insert
const useSemiPersistentState = () => {
  const [searchTerm, setSearchTerm] = React.useState(
    localStorage.getItem('search') || ''
  );

  React.useEffect(() => {
    localStorage.setItem('search', searchTerm);
  }, [searchTerm]);
};
# leanpub-end-insert

const App = () => {
...
};
~~~~~~~

So far, this custom hook is just a function around the `useState` and `useEffect` hooks that we've previously used in the App component. What's missing is providing an initial state and returning the values that are needed in our App component as an array:

{title="src/App.js",lang="javascript"}
~~~~~~~
# leanpub-start-insert
const useSemiPersistentState = (initialState) => {
# leanpub-end-insert
  const [searchTerm, setSearchTerm] = React.useState(
# leanpub-start-insert
    localStorage.getItem('search') || initialState
# leanpub-end-insert
  );

  React.useEffect(() => {
    localStorage.setItem('search', searchTerm);
  }, [searchTerm]);

# leanpub-start-insert
  return [searchTerm, setSearchTerm];
# leanpub-end-insert
};
~~~~~~~

We are following two conventions of React's built-in hooks here. First, the naming convention which puts the "use" prefix in front of every hook name; second, the returned values are returned as an array. Another goal of a custom hook should be reusability. All of this custom hook's internals are about a value of a certain search domain, but the hook should be for a generic value. Let's refactor the naming, therefore:

{title="src/App.js",lang="javascript"}
~~~~~~~
const useSemiPersistentState = (initialState) => {
# leanpub-start-insert
  const [value, setValue] = React.useState(
    localStorage.getItem('value') || initialState
# leanpub-end-insert
 );

  React.useEffect(() => {
# leanpub-start-insert
    localStorage.setItem('value', value);
  }, [value]);
# leanpub-end-insert

# leanpub-start-insert
  return [value, setValue];
# leanpub-end-insert
};
~~~~~~~

We handle an abstracted "value" within the custom hook. Using it in the App component, we can name the returned current state and state updater function anything domain-related (e.g. `searchTerm` and `setSearchTerm`) with array destructuring. There is still one problem with this custom hook. Using the custom hook more than once in a React application leads to an overwrite of the "value"-allocated item in the local storage, because it uses the same key in the local storage. To fix this, pass in an flexible key. Since the key comes from outside, the custom hook assumes that it could change, so it needs to be included in the dependency array of the `useEffect` hook as well. Without it, the side-effect may run with an outdated key (also called *stale*) if the key changed between renders:

{title="src/App.js",lang="javascript"}
~~~~~~~
# leanpub-start-insert
const useSemiPersistentState = (key, initialState) => {
# leanpub-end-insert
  const [value, setValue] = React.useState(
# leanpub-start-insert
    localStorage.getItem(key) || initialState
# leanpub-end-insert
 );

  React.useEffect(() => {
# leanpub-start-insert
    localStorage.setItem(key, value);
  }, [value, key]);
# leanpub-end-insert

  return [value, setValue];
};

const App = () => {
  ...

  const [searchTerm, setSearchTerm] = useSemiPersistentState(
# leanpub-start-insert
    'search',
# leanpub-end-insert
    'React'
  );

  ...
};
~~~~~~~

You've just created your first custom hook. If you're not comfortable with custom hooks, you can revert the changes and use the `useState` and `useEffect` hook as before, in the App component. However, knowing more about custom hooks gives you lots of new options. A custom hook can encapsulate non-trivial implementation details that should be kept away from a component; can be used in more than one React component; can be a composition of other hooks, and can even be open-sourced as an external library. Using your favorite search engine, you'll notice there are hundreds of React hooks that could be used in your application without worry over implementation details.

### Exercises:

* Confirm your [source code](https://bit.ly/30Koneb).
  * Confirm the [changes](https://bit.ly/2ZbkAGm).
* Read more about [React Hooks](https://www.robinwieruch.de/react-hooks) to get a good understanding of them, because they are the bread and butter in React function components.
* Optional: [Leave feedback for this section](https://forms.gle/5seN1Rv3ZwXmWmDR9).