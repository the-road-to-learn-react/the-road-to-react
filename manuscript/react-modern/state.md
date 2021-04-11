## React State

In contrast to React props, **React state** is used to make applications interactive. Both concepts, props and state, have clear defined purposes. Props are used to pass information down the component tree. State is used to alter information over time. Both can work hand in hand as well. We will see what this means in the following sections.

Let's start with state in React with the following use case: Whenever a user types something into an HTML input field, the user may want to see this typed information (state) displayed somewhere else in the application. Therefore we need some way to change information over time and, what's more important, to notify React to re-render its component(s) again. A naive (but wrong) approach would be the following:

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

When you try this in the browser, you will see that the output does not appear below the HTML input field after typing into it. However, this approach is not too far away from the actual solution. What's missing after all is the mechanisms to notify React to re-render the component with the new `searchTerm` state after the event handler updated it. In order to do so, we need to tell React that `searchTerm` is a state that changes over time and that whenever it changes React has to re-render its affected component(s). Fortunately, React offers us a utility function called `useState` for it:

{title="src/App.js",lang="javascript"}
~~~~~~~
const Search = () => {
# leanpub-start-insert
  const [searchTerm, setSearchTerm] = React.useState('');
# leanpub-end-insert

  ...
};
~~~~~~~

React's `useState` function takes an *initial state* as an argument -- where we will use an empty string. By providing this initial state to `useState`, we are telling React that this state will change over time. Furthermore, calling this function will return an array with two entries: The first entry (`searchTerm`) represents the *current state*; the second entry is a *function to update this state* (`setSearchTerm`). I will sometimes refer to this function as *state updater function*. Both entries are everything we need to display the current state and to alter the current state:

{title="src/App.js",lang="javascript"}
~~~~~~~
const Search = () => {
  const [searchTerm, setSearchTerm] = React.useState('');

  const handleChange = (event) => {
# leanpub-start-insert
    setSearchTerm(event.target.value);
# leanpub-end-insert
  };

  return (
    <div>
      <label htmlFor="search">Search: </label>
      <input id="search" type="text" onChange={handleChange} />

      <p>
        Searching for <strong>{searchTerm}</strong>.
      </p>
    </div>
  );
};
~~~~~~~

When the user types into the input field, the input field's change event is captured by the event handler. The handler's logic uses the event's value and the state updater function to set the updated state. After the updated state is set in a component, the component renders again, meaning the component function runs again. The updated state becomes the current state and is displayed in the component's JSX.

As an exercise, put a `console.log()` into each of your components. For example, the App component gets a `console.log('App renders')`, the List component gets a `console.log('List renders')` and so on. Now check your browser: For the first rendering, all loggings should appear, however, once you type into the HTML input field, only the Search component's logging should appear. That's because React only re-renders this component (and all of its child components), because its state changed.

It's important to note that the `useState` function is called a **React hook**. It's only one of many hooks provided by React and this section only scratched the surface. You will learn more about them throughout the next sections.

### Exercises:

* Confirm your [source code](https://codesandbox.io/s/github/the-road-to-learn-react/hacker-stories/tree/2021/React-State).
  * Confirm the [changes](https://github.com/the-road-to-learn-react/hacker-stories/compare/2021/React-Props...2021/React-State).
* Read more about [JavaScript array destructuring](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Destructuring_assignment#Array_destructuring).
* Read more about [React's useState Hook](https://www.robinwieruch.de/react-usestate-hook).
