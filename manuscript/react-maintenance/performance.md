# React Maintenance

Once a React application grows, maintenance becomes a priority. To prepare for this eventuality, we'll cover performance optimization, type safety, testing, and project structure. Each of these topics will strengthen your app to take on more functionality without losing quality.

Performance optimization prevents applications from slowing down by assuring efficient use of available resource. Typed programming languages like TypeScript detect bugs earlier in the feedback loop. Testing gives us more explicit feedback than typed programming, and provides a way to understand which actions can break the application. Lastly, a project structure supports the organized management of assets into folders and files, which is especially useful in scenarios where team members work in different domains.

## Performance in React (Advanced)

This section is just here for the sake of learning about performance improvements in React. We wouldn't need optimizations in most React applications, as React is fast out of the box. While more sophisticated tools exist for performance measurements in JavaScript and React, we will stick to a simple `console.log()` and our browser's developer tools for the logging output.

### Strict Mode

Before we can learn about performance in React, we will briefly look at React's Strict Mode which gets enabled in the *src/main.jsx* file:

{title="src/main.jsx",lang="javascript"}
~~~~~~~
ReactDOM.createRoot(document.getElementById('root')).render(
  <React.StrictMode>
    <App />
  </React.StrictMode>
);
~~~~~~~

[React's Strict Mode](https://bit.ly/3SufTxx) is a helper component which notifies developers in the case of something being wrong in our implementation. For example, using a [deprecated](https://bit.ly/3R8ycam) React API (e.g. using a legacy React hook) would give us a warning in the browser's developer tools. However, it also ensures that state and side-effects are implemented well by a developer. Let's experience what this means in our code.

The App component fetches initially data from a remote API which gets displayed as a list. We are using React's useEffect hook for initializing the data fetching. Now I encourage you to add a `console.log()` which logs whenever this hook runs:

{title="src/main.jsx",lang="javascript"}
~~~~~~~
const App = () => {
  ...

  React.useEffect(() => {
# leanpub-start-insert
    console.log('How many times do I log?');
# leanpub-end-insert
    handleFetchStories();
  }, [handleFetchStories]);

  ...
};
~~~~~~~

Many would expect seeing the logging only once in the browser's developer tools, because this side-effect should only run once (or if the `handleFetchStories` function changes). However, you will see the logging twice for the App component's initial render. To be honest, this is a highly unexpected behavior (even for seasoned React developers), which makes it difficult to understand for React beginners. However, the React core team decided that this behavior is needed for surfacing bugs related to misused side-effects in the application.

So React's Strict Mode runs React's useEffect Hooks twice for the initial render. Because this results in fetching the *same* data twice, this is not a problem for us. The operation is called idempotent, which means that the result of a successfully performed request is independent of the number of times it is executed. After all, it's *only* a performance problem, because there are two network requests, but it doesn't result in a buggy behavior of the application. In addition to all of this uncertainty, the Strict Mode is only applied for the development environment, so whenever this application gets build for production, the Strict Mode gets removed automatically.

Both of these behaviors, running React's useEffect Hook twice for the initial render and having different outcomes between development and production, surface many warranted discussions around React's Strict Mode.

For the following performance sections, I encourage you to disable the Strict Mode by simply removing it. This way, we can follow the logging that would happen for this application once it is build for a production environment:

{title="src/main.jsx",lang="javascript"}
~~~~~~~
ReactDOM.createRoot(document.getElementById('root')).render(
  <App />
);
~~~~~~~

However, at the end of the performance sections, I encourage you to add the Strict Mode back again, because it is there to help you after all.

### Don't run on first render

Previously, we have covered React's useEffect Hook, which is used for side-effects. It runs the first time a component renders (mounting), and then every re-render (updating). By passing an empty dependency array to it as a second argument, we can tell the hook to run on the first render only. Out of the box, there is no way to tell the hook to run only on every re-render (update) and not on the first render (mount). For example, examine our custom hook for state management with React's useState Hook and its semi-persistent state with local storage using React's useEffect Hook:

{title="src/App.jsx",lang="javascript"}
~~~~~~~
const useStorageState = (key, initialState) => {
  const [value, setValue] = React.useState(
    localStorage.getItem(key) || initialState
  );

  React.useEffect(() => {
# leanpub-start-insert
    console.log('A');
# leanpub-end-insert
    localStorage.setItem(key, value);
  }, [value, key]);

  return [value, setValue];
};
~~~~~~~

With a closer look at the developer's tools, we can see the log for the first time when the component renders using this custom hook. It doesn't make sense to run the side-effect for the initial rendering of the component though, because there is nothing to store in the local storage except the initial value. It's a redundant function invocation, and should only run for every update (re-rendering) of the component.

As mentioned, there is no React Hook that runs on every re-render, and there is no way to tell the `useEffect` hook in a React idiomatic way to call its function only on every re-render. However, by using React's useRef Hook which keeps its `ref.current` property intact over re-renders, we can keep a *made up state* (without re-rendering the component on state updates) with an instance variable of our component's lifecycle:

{title="src/App.jsx",lang="javascript"}
~~~~~~~
const useStorageState = (key, initialState) => {
# leanpub-start-insert
  const isMounted = React.useRef(false);
# leanpub-end-insert

  const [value, setValue] = React.useState(
    localStorage.getItem(key) || initialState
  );

  React.useEffect(() => {
# leanpub-start-insert
    if (!isMounted.current) {
      isMounted.current = true;
    } else {
# leanpub-end-insert
      console.log('A');
      localStorage.setItem(key, value);
# leanpub-start-insert
    }
# leanpub-end-insert
  }, [value, key]);

  return [value, setValue];
};
~~~~~~~

We are exploiting the `ref` and its mutable `current` property for imperative state management that doesn't trigger a re-render. Once the hook is called from its component for the first time (component render), the ref's `current` property is initialized with a `false` boolean called `isMounted`. As a result, the side-effect function in `useEffect` isn't called; only the boolean flag for `isMounted` is toggled to `true` in the side-effect. Whenever the hook runs again (component re-render), the boolean flag is evaluated in the side-effect. Since it's `true`, the side-effect function runs. Over the lifetime of the component, the `isMounted` boolean will remain `true`. It was there to avoid calling the side-effect function for the first time render that uses our custom hook.

The above was only about preventing the invocation of one simple function for a component rendering for the first time. But imagine you have an expensive computation in your side-effect, or the custom hook is used frequently in the application. It's more practical to deploy this technique to avoid unnecessary function invocations.

### Exercises:

* Read more about [running useEffect only on update](https://www.robinwieruch.de/react-useeffect-only-on-update/).
* Read more about [running useEffect only once](https://www.robinwieruch.de/react-useeffect-only-once/).

### Don't re-render if not needed

Earlier, we have explored React's re-rendering mechanism. We'll repeat this exercise for the App and List components. For both components, add a logging statement:

{title="src/App.jsx",lang="javascript"}
~~~~~~~
const App = () => {
  ...

# leanpub-start-insert
  console.log('B:App');
# leanpub-end-insert

  return ( ... );
};

const List = ({ list, onRemoveItem }) =>
# leanpub-start-insert
  console.log('B:List') || (
# leanpub-end-insert
    <ul>
      {list.map((item) => (
        <Item
          key={item.objectID}
          item={item}
          onRemoveItem={onRemoveItem}
        />
      ))}
    </ul>
  );
~~~~~~~

Because the List component has no function body, and developers are lazy folks who don't want to refactor the component for a simple logging statement, the List component uses the `||` operator instead. This is a neat trick for adding a logging statement to a function component without a function body. Since the `console.log()` on the left-hand side of the operator always evaluates to false, the right-hand side of the operator gets always executed.

{title="Code Playground",lang="javascript"}
~~~~~~~
function getTheTruth() {
  if (console.log('B:List')) {
    return true;
  } else {
    return false;
  }
}

console.log(getTheTruth());
// B:List
// false
~~~~~~~

Let's focus on the actual logging in the browser's developer tools when refreshing the page. You should see a similar output. First, the App component renders, followed by its child components (e.g. List component).

{title="Visualization",lang="text"}
~~~~~~~
B:App
B:List
B:App
B:App
B:List
~~~~~~~

*Again: If you are seeing more than these loggings, check whether your *src/main.jsx* file uses `<React.StrictMode>` as a wrapper for your App component. If it's the case, remove the Strict Mode and check your logging again. Explanation: In development mode, React's Strict Mode renders a component twice to detect problems with your implementation in order to warn you about these. This Strict Mode is automatically excluded for applications in production. However, if you don't want to be confused by the multiple renders, remove Strict Mode from the *src/main.jsx* file.*

Since a side-effect triggers data fetching after the first render, only the App component renders, because the List component is replaced by a loading indicator in a conditional rendering. Once the data arrives, both components render again.

{title="Visualization",lang="text"}
~~~~~~~
// initial render
B:App
B:List

// data fetching with loading instead of List component
B:App

// re-rendering with data
B:App
B:List
~~~~~~~

So far, this behavior is acceptable, since everything renders on time. Now we'll take this experiment a step further, by typing into the SearchForm component's input field. You should see the changes with every character entered into the element:

{title="Visualization",lang="text"}
~~~~~~~
B:App
B:List
~~~~~~~

What's striking is that the List component shouldn't re-render, but it does. The search feature isn't executed via its button, so the `list` passed to the List component via the App component remains the same for every keystroke. This is React's default behavior, re-rendering everything below a component (here: the `App` component) with a state change, which surprises many people. In other words, if a parent component re-renders, its descendent components re-render as well. React does this by default, because preventing a re-render of child components could lead to bugs. Because the re-rendering mechanism of React is often fast enough by default, the automatic re-rendering of descendent components is encouraged by React.

Sometimes we want to prevent re-rendering, however. For example, huge data sets displayed in a table (e.g. List component) shouldn't re-render if they are not affected by an update (e.g. Search component). It's more efficient to perform an equality check if something changed for the component. Therefore, we can use React's memo API to make this equality check for the props:

{title="src/App.jsx",lang="javascript"}
~~~~~~~
# leanpub-start-insert
const List = React.memo(
# leanpub-end-insert
  ({ list, onRemoveItem }) =>
    console.log('B:List') || (
      <ul>
        {list.map((item) => (
          <Item
            key={item.objectID}
            item={item}
            onRemoveItem={onRemoveItem}
          />
        ))}
      </ul>
    )
# leanpub-start-insert
);
# leanpub-end-insert
~~~~~~~

React's memo API checks whether the props of a component have changed. If not, it does not re-render even though its parent component re-rendered. However, the output stays the same when typing into the SearchForm's input field:

{title="Visualization",lang="text"}
~~~~~~~
B:App
B:List
~~~~~~~

The `list` passed to the List component is the same, but the `onRemoveItem` callback handler isn't. If the App component re-renders, it always creates a new version of this callback handler as a new function. Earlier, we used React's useCallback Hook to prevent this behavior, by creating a function only on the initial render (or if one of its dependencies has changed):

{title="src/App.jsx",lang="javascript"}
~~~~~~~
const App = () => {
  ...

# leanpub-start-insert
  const handleRemoveStory = React.useCallback((item) => {
# leanpub-end-insert
    dispatchStories({
      type: 'REMOVE_STORY',
      payload: item,
    });
# leanpub-start-insert
  }, []);
# leanpub-end-insert

  ...

  console.log('B:App');

  return (... );
};
~~~~~~~

Since the callback handler gets the `item` passed as an argument in its function signature, it doesn't have any dependencies and is declared only once when the App component initially renders. None of the props passed to the List component should change now. Try it with the combination of React `memo` and `useCallback`, to search via the SearchForm's input field. The "B:List" logging disappears, and only the App component re-renders with its "B:App" logging.

![](images/memo.png)

While all props passed to a component stay the same, the component renders again if its parent component is forced to re-render. That's React's default behavior, which works most of the time because the re-rendering mechanism is pretty fast. However, if re-rendering decreases the performance of a React application, React's `memo` API helps prevent re-rendering. As we have seen, sometimes `memo` alone doesn't help, though. Callback handlers are re-defined each time in the parent component and passed as *changed* props to the component, which causes another re-render. In that case, `useCallback` is used for making the callback handler only change when its dependencies change.

### Exercises:

* Read more about [React's memo API](https://www.robinwieruch.de/react-memo/).
* Read more about [React's useCallback Hook](https://www.robinwieruch.de/react-usecallback-hook/).

### Don't rerun expensive computations

Sometimes we'll have performance-intensive computations in our React components -- between a component's function signature and return block -- which run on every render. For this scenario, we must create a use case in our current application first:

{title="src/App.jsx",lang="javascript"}
~~~~~~~
# leanpub-start-insert
const getSumComments = (stories) => {
  console.log('C');

  return stories.data.reduce(
    (result, value) => result + value.num_comments,
    0
  );
};
# leanpub-end-insert

const App = () => {
  ...

# leanpub-start-insert
  const sumComments = getSumComments(stories);
# leanpub-end-insert

  return (
    <div>
# leanpub-start-insert
      <h1>My Hacker Stories with {sumComments} comments.</h1>
# leanpub-end-insert

      ...
    </div>
  );
};
~~~~~~~

If all arguments are passed to a function, it's acceptable to have it outside the component, because it does not have any further dependency needed from within the component. This prevents creating the function on every render, so the `useCallback` hook becomes unnecessary. However, the function still computes the value of summed comments on every render, which becomes a problem for more expensive computations.

![](images/usememo-1.png)

Each time text is typed in the input field of the SearchForm component, this computation runs again with an output of "C". This may be fine for a non-heavy computation like this one, but imagine this computation would take more than 500ms. It would give the re-rendering a delay, because everything in the component has to wait for this computation. We can tell React to only run a function if one of its dependencies has changed. If no dependency changed, the result of the function stays the same. React's useMemo Hook helps us here:

{title="src/App.jsx",lang="javascript"}
~~~~~~~
const App = () => {
  ...

# leanpub-start-insert
  const sumComments = React.useMemo(
    () => getSumComments(stories),
    [stories]
  );
# leanpub-end-insert

  return ( ... );
};
~~~~~~~

For every time someone types in the SearchForm, the computation shouldn't run again. It only runs if the dependency array, here `stories`, has changed. After all, this should only be used for cost expensive computations which could lead to a delay of a (re-)rendering of a component.

![](images/usememo-2.png)

Now, after we went through these scenarios for `useMemo`, `useCallback`, and `memo`, remember that these shouldn't necessarily be used by default. Apply these performance optimizations only if you run into performance bottlenecks. Most of the time this shouldn't happen, because React's rendering mechanism is pretty efficient by default. Sometimes the check for utilities like `memo` can be more expensive than the re-rendering itself.

### Exercises:

* Compare your source code against the author's [source code](https://bit.ly/3R2xISV).
  * Recap all the [source code changes from this section](https://bit.ly/3R8BYk2).
* Read more about [React's useMemo Hook](https://www.robinwieruch.de/react-usememo-hook/).
* Download *React Developer Tools* as an extension for your browser. Open it for your application in the browser via the browser's developer tools and try its various features. For example, you can use it to visualize React's component tree and its updating components.
* Does the SearchForm re-render when removing an item from the List with the "Dismiss"-button? If it's the case, apply performance optimization techniques (using `useCallback` and `memo`) to prevent re-rendering.
* Does each Item re-render when removing an item from the List with the "Dismiss"-button? If it's the case, apply performance optimization techniques to prevent re-rendering.
* Remove all performance optimizations to keep the application simple. Our current application doesn't suffer from any performance bottlenecks. Try to avoid [premature optimzations](https://bit.ly/3AYktL8). Use this section and its further reading material as a reference, in case you run into performance problems.
* Optional: [Leave feedback for this section](https://forms.gle/FwNrJSdLikquVzsB6).