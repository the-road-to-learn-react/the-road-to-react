## React Props

We are currently using the `list` variable as a global variable in the current application. We used it directly from the global scope in the App component, and again in the List component. This could work if you only had one variable, but it doesn't scale with multiple variables across multiple components from many different files.

Using so called props, we can pass variables as information from one component to another component. Before using props, we'll move the list from the global scope into the App component and rename it to its actual domain:

{title="src/App.js",lang="javascript"}
~~~~~~~
const App = () => {
# leanpub-start-insert
  const stories = [
    {
      title: 'React',
      url: 'https://reactjs.org/',
      author: 'Jordan Walke',
      num_comments: 3,
      points: 4,
      objectID: 0,
    },
    {
      title: 'Redux',
      url: 'https://redux.js.org/',
      author: 'Dan Abramov, Andrew Clark',
      num_comments: 2,
      points: 5,
      objectID: 1,
    },
  ];
# leanpub-end-insert

  const handleChange = event => { ... };

  return ( ... );
};
~~~~~~~

Next, we'll use **React props** to pass the array to the List component:

{title="src/App.js",lang="javascript"}
~~~~~~~
const App = () => {
  const stories = [ ... ];

  const handleChange = event => { ... };

  return (
    <div>
      <h1>My Hacker Stories</h1>

      <label htmlFor="search">Search: </label>
      <input id="search" type="text" onChange={handleChange} />

      <hr />

# leanpub-start-insert
      <List list={stories} />
# leanpub-end-insert
    </div>
  );
};
~~~~~~~

The variable is called `stories` in the App component, and we pass it under this name to the List component. In the List component's instantiation, however, it is assigned to the `list` attribute. We access it as `list` from the `props` object in the List component's function signature:

{title="src/App.js",lang="javascript"}
~~~~~~~
# leanpub-start-insert
const List = props =>
  props.list.map(item => (
# leanpub-end-insert
    <div key={item.objectID}>
      <span>
        <a href={item.url}>{item.title}</a>
      </span>
      <span>{item.author}</span>
      <span>{item.num_comments}</span>
      <span>{item.points}</span>
    </div>
  ));
~~~~~~~

Using this operation, we've prevented the list/stories variable from polluting the global scope in the App component. Since `stories` is not used in the App component directly, but in one of its child components, we passed them as props to the List component. There, we can access it through the first function signature's argument, called `props`.

### Exercises:

* Confirm your [source code for the last section](https://codesandbox.io/s/github/the-road-to-learn-react/hacker-stories/tree/hs/React-Props).
  * Confirm the [changes from the last section](https://github.com/the-road-to-learn-react/hacker-stories/compare/hs/Handler-Function-in-JSX...hs/React-Props?expand=1).
* Read more about [how to pass props to React components](https://www.robinwieruch.de/react-pass-props-to-component).