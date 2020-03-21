## React Custom Hooks (Advanced)

Thus far we've covered the two most popular hooks in React: useState and useEffect. useState is used to make your application interactive; useEffect is used to opt into the lifecycle of your components.

We'll eventually cover more hooks that come with React -- in this volume and in other resources -- though we won't cover all of them here. Next we'll tackle **React custom Hooks**; that is, building a hook yourself.

We will use the two hooks we already possess to create a new custom hook called `useSemiPersistentState`, named as such because it manages state yet synchronizes with the local storage. It's not fully persistent because clearing the local storage of the browser deletes relevant data for this application. Start by extracting all relevant implementation details from the App component into this new custom hook:

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

So far, it's just a function around our previously in the App component used `useState` and `useEffect` hooks. Before we can use it, let's return the values that are needed in our App component from this custom hook:

{title="src/App.js",lang="javascript"}
~~~~~~~
const useSemiPersistentState = () => {
 const [searchTerm, setSearchTerm] = React.useState(
   localStorage.getItem('search') || ''
 );

 React.useEffect(() => {
   localStorage.setItem('search', searchTerm);
 }, [searchTerm]);

# leanpub-start-insert
 return [searchTerm, setSearchTerm];
# leanpub-end-insert
};
~~~~~~~

We are following two conventions of React's built-in hooks here. First, the naming convention which puts the "use" prefix in front of every hook name; second, the returned values are returned as an array. Now we can use the custom hook with its returned values in the App component with the usual array destructuring:

{title="src/App.js",lang="javascript"}
~~~~~~~
const App = () => {
 const stories = [ ... ];

# leanpub-start-insert
 const [searchTerm, setSearchTerm] = useSemiPersistentState();
# leanpub-end-insert

 const handleSearch = event => {
   setSearchTerm(event.target.value);
 };

 const searchedStories = stories.filter(story =>
   story.title.toLowerCase().includes(searchTerm.toLowerCase())
 );

 return (
   ...
 );
};
~~~~~~~

Another goal of a custom hook should be reusability. All of this custom hook's internals are about the search domain, but the hook should be for a value that's set in state and synchronized in local storage. Let's adjust the naming therefore:

{title="src/App.js",lang="javascript"}
~~~~~~~
const useSemiPersistentState = () => {
# leanpub-start-insert
 const [value, setValue] = React.useState(
   localStorage.getItem('value') || ''
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

We handle an abstracted "value" within the custom hook. Using it in the App component, we can name the returned current state and state updater function anything domain-related (e.g. `searchTerm` and `setSearchTerm`) with array destructuring.

There is still one problem with this custom hook. Using the custom hook more than once in a React application leads to an overwrite of the "value"-allocated item in the local storage. To fix this, pass in a key:

{title="src/App.js",lang="javascript"}
~~~~~~~
# leanpub-start-insert
const useSemiPersistentState = key => {
# leanpub-end-insert
 const [value, setValue] = React.useState(
# leanpub-start-insert
   localStorage.getItem(key) || ''
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
   'search'
# leanpub-end-insert
 );

 ...
};
~~~~~~~

Since the key comes from outside, the custom hook assumes that it could change, so it needs to be included in the dependency array of the `useEffect` hook. Without it, the side-effect may run with an outdated key (also called *stale*) if the key changed between renders.

Another improvement is to give the custom hook the initial state we had from the outside:

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

 ...
};

const App = () => {
 ...

 const [searchTerm, setSearchTerm] = useSemiPersistentState(
# leanpub-start-insert
   'search',
   'React'
# leanpub-end-insert
 );

 ...
};
~~~~~~~

You've just created your first custom hook. If you're not comfortable with custom hooks, you can revert the changes and use the `useState` and `useEffect` hook as before, in the App component.

However, knowing more about custom hooks gives you lots of new options. A custom hook can encapsulate non-trivial implementation details that should be kept away from a component; can be used in more than one React component; and can even be open-sourced as an external library. Using your favorite search engine, you'll notice there are hundreds of React hooks that could be used in your application without worry over implementation details.

### Exercises:

* Confirm your [source code for the last section](https://codesandbox.io/s/github/the-road-to-learn-react/hacker-stories/tree/hs/React-Custom-Hooks).
 * Confirm the [changes from the last section](https://github.com/the-road-to-learn-react/hacker-stories/compare/hs/React-Side-Effects...hs/React-Custom-Hooks?expand=1).
* Read more about [React Hooks](https://www.robinwieruch.de/react-hooks) to get a good understanding of them. They are the bread and butter in React function components, so it's important to really understand them ([0](https://reactjs.org/docs/hooks-overview.html), [1](https://reactjs.org/docs/hooks-custom.html)).