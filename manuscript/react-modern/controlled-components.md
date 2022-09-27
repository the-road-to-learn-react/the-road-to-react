## React Controlled Components

HTML elements come with their internal state which is not coupled to React. Confirm this thesis yourself by checking how your HTML input field is implemented in your Search component. While we provide essential attributes like `id` and `type` in addition to a handler (here: `onChange`), we do not tell the element its value. However, it does show the correct value when a user types into it.

Now try the following: When initializing the `searchTerm` state in the App component, use `'React'` as initial state instead of an empty string. Afterward, open the application in the browser. Can you spot the problem? Spend some time on your own figuring out what happens here and how to fix this problem.

While the `stories` have been filtered respectively to the new initial `searchTerm`, the HTML input field doesn't show the value in the browser. Only when we start typing into the input field do we see the changes reflected in it. That's because the input field doesn't know anything about React's state (here: `searchTerm`), it only uses its handler to communicate (see `handleSearch()`) its internal state to React state. And once a user starts typing into the input field, the HTML element keeps track of these changes itself. However, if we want to get things right, the HTML should know about the React state. Therefore, we need to provide the current state as `value` to it:

{title="src/App.jsx",lang="javascript"}
~~~~~~~
const App = () => {
  const stories = [ ... ];

# leanpub-start-insert
  const [searchTerm, setSearchTerm] = React.useState('React');
# leanpub-end-insert

  ...

  return (
    <div>
      <h1>My Hacker Stories</h1>

# leanpub-start-insert
      <Search search={searchTerm} onSearch={handleSearch} />
# leanpub-end-insert

      ...
    </div>
  );
};

const Search = (props) => (
  <div>
    <label htmlFor="search">Search: </label>
    <input
      id="search"
      type="text"
# leanpub-start-insert
      value={props.search}
# leanpub-end-insert
      onChange={props.onSearch}
    />
  </div>
);
~~~~~~~

Now both states are synchronized. Instead of giving the HTML element the freedom of keeping track of its internal state, it uses React's state by leveraging the element's `value` attribute instead. Whenever the HTML element emits a change event, the new value is written to Reacts state and re-renders the components. Then the HTML element uses the recent state as `value` again.

![](images/controlled-component.png)

Earlier the HTML element did its own thing, but now we are in control of it by feeding React's state into it. Now, while the input field became explicitly a **controlled element**, the Search component became implicitly a **controlled component**. As a React beginner, using controlled components is important, because you want to enforce a predictable behavior. Later though, there will be cases for uncontrolled components too.

### Exercises:

* Compare your source code against the author's [source code](https://bit.ly/3dzwFwb).
  * Recap all the [source code changes from this section](https://bit.ly/3BXGMEk).
  * Optional: If you are using TypeScript, check out the author's source code [here](https://bit.ly/3SfJBGP).
* Read more about [controlled components in React](https://www.robinwieruch.de/react-controlled-components/).
* Optional: [Leave feedback for this section](https://forms.gle/7VYTww2EQiPkFnaR8).