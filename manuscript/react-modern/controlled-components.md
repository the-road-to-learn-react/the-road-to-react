## React Controlled Components

When you type into your HTML input field and see the characters showing up, you may have noticed that the element itself holds an internal state, because we are not providing any external value to it. Let me show you where this behavior leads to unexpected results: After applying the following change -- giving the `searchTerm` an initial state of 'React' -- can you spot the mistake in your browser?

{title="src/App.js",lang="javascript"}
~~~~~~~
const App = () => {
  const stories = [ ... ];

# leanpub-start-insert
  const [searchTerm, setSearchTerm] = React.useState('React');
# leanpub-end-insert

 ...
};
~~~~~~~

While the list has been filtered respectively to this search term, the HTML input field doesn't show the value. Only when typing into the input field we see the change reflected in it. However, if we want to start properly with the initial state in the input field, we need to convert the Search component with its input field into a so-called **controlled component**. So far, the input field doesn't know anything about the `searchTerm`. It only uses the `onChange` handler to inform us of a change. Good for us that the input field has a `value` attribute which we can use as well:

{title="src/App.js",lang="javascript"}
~~~~~~~
const App = () => {
  const stories = [ ... ];

  const [searchTerm, setSearchTerm] = React.useState('React');

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

Now the input field uses the correct initial value when displaying it in the browser. When we use the `searchTerm` state from the App component via props, we force the input field to use this value over its internally managed element's state.

![](images/controlled-component.png)

We learned about controlled components in this section. Taking all the previous sections as learning steps into consideration, we discovered another concept called **unidirectional data flow**:

{title="Visualization",lang="javascript"}
~~~~~~~
UI -> Side-Effect -> State -> UI -> ...
~~~~~~~

A React application and its components start with an initial state, which may or may not be passed down as props to interested child components. It's rendered for the first time as a UI. Once a side-effect occurs, like user interaction (e.g. typing into an input field) or data loading from a remote API, the change is captured in React's state either in the component itself or by notifying parent components via a callback handler. Once state has been changed, all components below the component with the modified state are re-rendered (meaning: the component functions run again).

In the previous sections, we also learned about React's **component lifecycle**. At first, all components are instantiated from the top to the bottom of the component hierarchy. This includes all hooks (e.g. `useState`) that are instantiated with their initial values (e.g. initial state). From there, the UI awaits side-effects like user interactions. Once the state is changed (e.g. current state changed via state updater function from `useState`), all components below render again.

Every run through a component's function takes the *recent value* (e.g. current state) from React's useState Hook and *doesn't* reinitialize them again (e.g. initial state). This might seem odd, as one could assume the `useState` hooks function re-initializes again with its initial value, but it doesn't. Hooks initialize only once when the component renders for the first time, after which React tracks them internally with their most recent values.

### Exercises:

* Confirm your [source code](https://bit.ly/3aXr7GZ).
  * Confirm the [changes](https://bit.ly/3aV4XVO).
* Read more about [controlled components in React](https://www.robinwieruch.de/react-controlled-components/).
* Experiment with `console.log()` in your React components and observe how your changes render, both initially and after the input field changes.
* Optional: [Rate this section](https://forms.gle/7VYTww2EQiPkFnaR8).