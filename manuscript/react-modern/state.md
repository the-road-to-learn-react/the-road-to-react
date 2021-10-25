## React State

While it is not allowed to mutate React props as a developer, because they are only there to pass information from parent to child components, **React state** introduces mutable values (read: stateful values). These stateful values get instantiated in a React component as so called state, can be passed with props as vehicle down to child components, but can also get changed in the component where they got instantiated. When a state gets changed, the component with the state and all child components will re-render (read: run their component's function again).

![](images/react-state.png)

Both concepts, props and state, have clear defined purposes: While props are used to pass information down the component hierarchy, state is used to change information over time. Let's start with state in React with the following use case: Whenever a user types text into our HTML input field element in the Search component, the user wants to see this information (state) displayed next to it. An intuitive (but not working) approach would be the following:

{title="src/App.js",lang="javascript"}
~~~~~~~
const Search = () => {
# leanpub-start-insert
  let searchTerm = '';
# leanpub-end-insert

  const handleChange = (event) => {
# leanpub-start-insert
    searchTerm = event.target.value;
# leanpub-end-insert
  };

  return (
    <div>
      <label htmlFor="search">Search: </label>
      <input id="search" type="text" onChange={handleChange} />

# leanpub-start-insert
      <p>
        Searching for <strong>{searchTerm}</strong>.
      </p>
# leanpub-end-insert
    </div>
  );
};
~~~~~~~

When you try this in the browser, you will see that the output does not appear below the HTML input field after typing into it. However, this approach is not too far away from the actual solution. What's missing is telling React that `searchTerm` is a stateful value. Fortunately, React offers us a method called `useState` for it:

{title="src/App.js",lang="javascript"}
~~~~~~~
const Search = () => {
# leanpub-start-insert
  const [searchTerm, setSearchTerm] = React.useState('');
# leanpub-end-insert

  const handleChange = (event) => {
# leanpub-start-insert
    setSearchTerm(event.target.value);
# leanpub-end-insert
  };

  ...
};
~~~~~~~

By using `useState`, we are telling React that we want to have a stateful value which changes over time. And whenever this stateful value changes, the affected components (here: Search component) will re-render to use (here: display) their recent values.

![](images/react-usestate.png)

React's `useState` method takes an *initial state* as an argument -- in our case it is an empty string. Furthermore, calling this method will return an array with two entries: The first entry (`searchTerm`) represents the *current state*. The second entry (`setSearchTerm`) is a *function to update this state*. The book will refer to this function as *state updater function*. Both entries are everything we need to display the current state and to mutate it.

When the user types into the input field, the input field's change event is captured by the event handler. The handler's logic uses the event's value of the target and the state updater function to set the updated state. After the updated state is set in a component, the component renders again (meaning the component function runs again). The updated state becomes the current state (here: `searchTerm`) and is displayed in the component's JSX.

As an exercise, put a `console.log()` into each of your components. For example, the App component gets a `console.log('App renders')`, the List component gets a `console.log('List renders')` and so on. Now check your browser: For the first rendering, all loggings should appear, however, once you type into the HTML input field, only the Search component's logging should appear. React only re-renders this component (and all of its potential child components) after its state has changed.

It's important to note that the `useState` function is called a **React hook**. It's only one of many hooks provided by React and this section only scratched the surface of hooks in React. You will learn more about them throughout the next sections. You can have as many `useState` hooks as you want in one or multiple components whereas state can be anything from a JavaScript string (like in our case) to a more complex data structure such as an array or object.

A hook like React's `useState` hook has its own underlying "magic" to it. Previously a React component received optional props and rendered its JSX. Based on the props, the React component always rendered the same JSX (based on an input, it renders the same output, also called pure component). However, now we have introduced a stateful value inside a component which can influence the returned JSX of the component. Even though the component's function runs again for every re-render, React remembers the most recent state from its `useState` hooks all the time. Under the hood, React allocates an invisible container for each component where information like state is stored in memory.

### Exercises:

* Confirm your [source code](https://bit.ly/3prVjSO).
  * Confirm the [changes](https://bit.ly/30ISOBv).
* Read more about [JavaScript array destructuring](https://mzl.la/3ncC7WI).
* Read more about [React's useState Hook](https://www.robinwieruch.de/react-usestate-hook).
* Optional: [Leave feedback for this section](https://forms.gle/ZJNbQqq3Lw3RiD4H9).