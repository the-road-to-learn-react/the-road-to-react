## Callback Handlers in JSX

While props are passed down as information from parent to child components, state can be used to change information over time. However, we don't have all the pieces yet to make our components talk to each other. When using props as vehicle to transport information, we can only talk to descendant components. When using state, we can make information stateful, but this information can also only be passed down by using props as container.

For example, at the moment, the Search component does not share its state with other components, so it's only used (here: displayed) and updated by the Search component. That's fine for displaying the most recent state in the Search component, however, at the end we want to use this state somewhere else. In this section for example, we want to use the state in the App component to filter the `stories` by `searchTerm` before they get passed to the List component. So we know that we could communicate to a child component via props, but do not know how to communicate the state up to a parent component (here: from Search to App component).

![](images/callback-handler-1.png)

There is no way to pass information up the component tree, since props are naturally only passed downwards. However, we can introduce a **callback handler** instead: A callback handler gets introduced as event handler (A), is passed as function in props to another component (B), is executed there as callback handler (C), and *calls back* to the place it was introduced (D):

{title="src/App.jsx",lang="javascript"}
~~~~~~~
const App = () => {
  const stories = [ ... ];

# leanpub-start-insert
  // A
  const handleSearch = (event) => {
    // D
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

# leanpub-start-insert
const Search = (props) => {
# leanpub-end-insert
  const [searchTerm, setSearchTerm] = React.useState('');

  const handleChange = (event) => {
    setSearchTerm(event.target.value);

# leanpub-start-insert
    // C
    props.onSearch(event);
# leanpub-end-insert
  };

  return ( ... );
};
~~~~~~~

Whenever a user types into the input field now, the function that is passed down from the App component to the Search component runs. This way, we can notify the App component when a user types into the input field in the Search component. Essentially a callback handler, which is just a more specific type of an event handler, becomes our implicit vehicle to communicate upwards the component tree.

![](images/callback-handler-2.png)

The concept of the callback handler in a nutshell: We pass a function from a parent component (App) to a child component (Search) via props; we call this function in the child component (Search), but have the actual implementation of the called function in the parent component (App). In other words, when an (event) handler is passed as props from a parent component to its child component, it becomes a callback handler. React props are always passed down the component tree and therefore functions that are passed down as callback handlers in props can be used to communicate up the component tree.

### Exercises:

* Compare your source code against the author's [source code](https://bit.ly/3BVIZjQ).
  * Recap all the [source code changes from this section](https://bit.ly/3S97S1e).
  * Optional: If you are using TypeScript, check out the author's source code [here](https://bit.ly/3LKjtBa).
* Revisit the concepts of [(event) handler and callback handler](https://www.robinwieruch.de/react-event-handler/) as many times as you need.
* Optional: [Leave feedback for this section](https://forms.gle/3LoBoWKCMNT2YpnA7).