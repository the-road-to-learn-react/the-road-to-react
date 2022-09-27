## React Custom Hooks (Advanced)

So far, we've covered the two most popular hooks in React: useState and useEffect. useState is used for values that change over time; useEffect is used to opt into the lifecycle of your components to introduce side-effects. We'll eventually cover more hooks that come with React, but next, we'll tackle **React custom Hooks** which means creating a hook yourself.

We will use the two hooks that we already know to create a new custom hook called `useStorageState` which will keep the component's state in sync with the browser's local storage. We will start with how we want to use the hook in our App component:

{title="src/App.jsx",lang="javascript"}
~~~~~~~
const App = () => {
  const stories = [ ... ];

# leanpub-start-insert
  const [searchTerm, setSearchTerm] = useStorageState('React');
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

This new custom hook allows us to use it the same way as React's built-in useState Hook. It returns a state and a state updater function and accepts an initial state as argument. Under the hood, we want that this hook synchronizes the state with the browser's local storage. If you look closely at the App component in the previous code snippet, you can see that none of the previously introduced local storage features are there anymore. Instead, we will copy and paste this functionality over to our new custom hook:

{title="src/App.jsx",lang="javascript"}
~~~~~~~
# leanpub-start-insert
const useStorageState = () => {
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

So far, this custom hook is just a function around the `useState` and `useEffect` hooks which we've previously used in the App component. What's missing is providing an initial state and returning the values that are needed in our App component as an array:

{title="src/App.jsx",lang="javascript"}
~~~~~~~
# leanpub-start-insert
const useStorageState = (initialState) => {
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

We are following two conventions of React's built-in hooks here. First, the naming convention which puts the "use" prefix in front of every hook name. And second, the returned values are returned as an array. Another goal of a custom hook should be reusability. All of this custom hook's internals are about a certain search domain, however, to make the custom hook reusable and therefore generic, we have to adjust the internal names:

{title="src/App.jsx",lang="javascript"}
~~~~~~~
const useStorageState = (initialState) => {
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

Now we handle an abstracted "value" within the custom hook. Using it in the App component, we can name the returned current state and state updater function anything domain-related (e.g. `searchTerm` and `setSearchTerm`) with array destructuring.

There is still one problem with this custom hook. Using the custom hook more than once in a React application leads to an overwrite of the "value"-allocated item in the local storage, because it uses the same key in the local storage. To fix this, pass in a flexible key. Since the key comes from outside, the custom hook assumes that it could change, so it needs to be included in the dependency array of the `useEffect` hook as well. Without it, the side-effect may run with an outdated key (also called *stale*) if the key changed between renders:

{title="src/App.jsx",lang="javascript"}
~~~~~~~
# leanpub-start-insert
const useStorageState = (key, initialState) => {
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

  const [searchTerm, setSearchTerm] = useStorageState(
# leanpub-start-insert
    'search',
# leanpub-end-insert
    'React'
  );

  ...
};
~~~~~~~

With the key in place, you can use this new custom hook more than once in your application. You only need to make sure that the first argument, the key you are passing in, is a unique identifier which allocates the state in the browser's local storage under a unique key. If you happen to use the same key more than once for multiple `useStorageState` hook usages, then all these hooks will work on the same local storage key/value pair.

![](images/use-storage-state.png)

You've just created your first custom hook. If you're not comfortable with custom hooks, you can revert the changes and use the `useState` and `useEffect` hook as before in the App component. However, knowing about custom hooks gives you lots of new options. A custom hook can encapsulate non-trivial implementation details that should be kept away from a component, can be used in more than one React component, can be a composition of other hooks, and can even be open-sourced as an external library. Using your favorite search engine, you'll notice there are hundreds of React hooks that could be used in your application without worry over implementation details.

### Exercises:

* Compare your source code against the author's [source code](https://bit.ly/3fcvkMp).
  * Recap all the [source code changes from this section](https://bit.ly/3UnqBr8).
  * Optional: If you are using TypeScript, check out the author's source code [here](https://bit.ly/3SD7dok).
* Read more about [React Hooks](https://www.robinwieruch.de/react-hooks/) and [custom React Hooks](https://www.robinwieruch.de/react-custom-hook/) to get a good understanding of them, because they are the bread and butter in React function components.
* Optional: [Leave feedback for this section](https://forms.gle/5seN1Rv3ZwXmWmDR9).