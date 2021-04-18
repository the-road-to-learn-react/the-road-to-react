## Callback Handlers in JSX

The last sections taught us important lessons about props and state in React. While props are passed down as information from parent to child components, state can be used to change information over time and to make React show this changed information. However, we don't have all the pieces yet to make our components talk to each other. At the moment, the Search component does not share its state with other components, so it's only used and updated by the Search component and thus becomes useless for the other components.

![](images/callback-handler.png)

There is no way to pass information up the component tree since props are naturally only passed downwards. However, we can introduce a **callback handler**: A callback function gets introduced (A), is used elsewhere (B), but "calls back" to the place it was introduced (C):

{title="src/App.js",lang="javascript"}
~~~~~~~
const App = () => {
  const stories = [ ... ];

# leanpub-start-insert
  // A
  const handleSearch = (event) => {
    // C
    console.log(event.target.value);
  };
# leanpub-end-insert

  return (
    <div>
      <h1>My Hacker Stories</h1>

# leanpub-start-insert
      {/* // B */}
      <Search onSearch={handleSearch} />
# leanpub-end-insert

      <hr />

      <List list={stories} />
    </div>
  );
};
~~~~~~~

Now our Search component can use this callback handler from its incoming props to call it whenever a user types into the HTML input field:

{title="src/App.js",lang="javascript"}
~~~~~~~
# leanpub-start-insert
const Search = (props) => {
# leanpub-end-insert
  const [searchTerm, setSearchTerm] = React.useState('');

  const handleChange = (event) => {
    setSearchTerm(event.target.value);

# leanpub-start-insert
    // B
    props.onSearch(event);
# leanpub-end-insert
  };

  return ( ... );
};
~~~~~~~

The concept of the callback handler in a nutshell: We pass a function from a parent component (App) to a child component (Search) via props; we call this function in the child component (Search), but have the actual implementation of the called function in the parent component (App). Essentially when an (event) handler is passed as props from a parent component to its child component, it becomes a callback handler. React props are always passed down the component tree, and callback handlers passed as functions in props can be used to communicate up the component hierarchy.

### Exercises:

* Confirm your [source code](https://codesandbox.io/s/github/the-road-to-learn-react/hacker-stories/tree/2021/Callback-Handler-in-JSX).
  * Confirm the [changes](https://github.com/the-road-to-learn-react/hacker-stories/compare/2021/React-State...2021/Callback-Handler-in-JSX).
* Revisit the concepts of handler and callback handler as many times as you need.