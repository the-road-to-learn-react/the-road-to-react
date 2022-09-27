## React State

While it is not allowed to mutate React props as a developer, because they are only there to pass information from parent to child components, **React state** introduces a mutable data structure (read: stateful values). These stateful values get instantiated in a React component as so called state, can be passed with props as vehicle down to child components, but can also get mutated by using a function to modify the state. When a state gets mutated, the component with the state and all child components will re-render.

![](images/react-state.png)

Both concepts, props and state, have clear defined purposes: While props are used to pass information down the component hierarchy, state is used to change information over time. Let's start with state in React with the following use case: Whenever a user types text into our HTML input field element in the Search component, the user wants to see this information (state) displayed next to it. An intuitive (but not working) approach would be the following:

{title="src/App.jsx",lang="javascript"}
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

{title="src/App.jsx",lang="javascript"}
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

By using `useState`, we are telling React that we want to have a stateful value which changes over time. And whenever this stateful value changes, the affected components (here: Search component) will re-render to use it (here: to display the recent value).

React's `useState` method takes an *initial state* as an argument -- in our case it is an empty string. Furthermore, calling this method will return an array with two entries: The first entry (`searchTerm`) represents the *current state*. The second entry (`setSearchTerm`) is a function to update this state. The book will refer to this function as *state updater function*. Both entries are everything we need to display the current state (read) and to update it (write).

![](images/react-usestate.png)

When the user types into the input field, the input field's change event runs into the event handler. The handler's logic uses the event's value of the target and the state updater function to set the updated state. Afterward, the component re-renders (read: the component function runs). The updated state becomes the current state (here: `searchTerm`) and is displayed in the component's JSX.

As an exercise, put a `console.log()` into each of your components. For example, the App component gets a `console.log('App renders')`, the List component gets a `console.log('List renders')` and so on. Now check your browser: For the first rendering, all loggings should appear, however, once you type into the HTML input field, only the Search component's logging should appear. React only re-renders this component (and all of its potential descendant components) after its state has changed.

Now you have heard the terms rendering and re-rendering in a technical context as well. In essence every component in a React application has one initial rendering followed by potential re-renderings. Usually the initial rendering happens when a React component gets displayed in the browser. Then whenever a side-effect occurs, like a user interaction (e.g. typing into an input field), the change is captured in React's state which forces a re-rendering of all the components affected by this change; meaning the component which manages the state and all its descendant components.

![](images/react-lifecycle.png)

It's important to note that the `useState` function is called a **React hook**. It's only one of many hooks provided by React and this section only scratched the surface of hooks in React. You will learn more about them throughout the next sections. As for now, you should know that you can have as many `useState` hooks as you want in one or multiple components whereas state can be anything from a JavaScript string (like in this case) to a more complex data structure such as an array or object.

When the UI is rendered for the first time, every rendered component's `useState` hook gets initialized with an initial state which gets returned as current state. Whenever the UI is re-rendered because of a state change, the `useState` hook uses the most recent state from its internal [closure](https://www.robinwieruch.de/javascript-closure/). This might seem odd, as one would assume the `useState` gets declared from scratch every time a component's function runs. However, next to each component React allocates an object where information like state is stored in memory. Eventually the memory gets cleaned up once a component is not rendered anymore through JavaScript's garbage collection.

### Exercises:

* Compare your source code against the author's [source code](https://bit.ly/3R07WyW).
  * Recap all the [source code changes from this section](https://bit.ly/3f7mpMe).
  * Optional: If you are using TypeScript, check out the author's source code [here](https://bit.ly/3SwxFQu).
* Optional: Read more about [JavaScript array destructuring](https://mzl.la/3ncC7WI).
* Read more about [React's useState Hook](https://www.robinwieruch.de/react-usestate-hook/).
* Optional: [Leave feedback for this section](https://forms.gle/ZJNbQqq3Lw3RiD4H9).