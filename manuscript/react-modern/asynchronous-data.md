## React Asynchronous Data

We have two interactions in our application: searching the list and removing items from the list. While the first interaction is a fluctuant interference through a third-party state (`searchTerm`) applied on the list, the second interaction is a non-reversible deletion of an item from the list. However, the list we are dealing with is still just sample data. What about preparing our application to deal with real data instead?

Usually, data from a remote backend/database arrives asynchronously for client-side applications like React. Thus it's often the case that we must render a component before we can initiate the data fetching. In the following, we will start by simulating this kind of asynchronous data with our sample data in the application. Later, we will replace the sample data with real data fetched from a real remote API. We start off with a function that returns a promise with data in its shorthand version once it resolves. The resolved object holds the previous list of stories:

{title="src/App.jsx",lang="javascript"}
~~~~~~~
const initialStories = [ ... ];

# leanpub-start-insert
const getAsyncStories = () =>
  Promise.resolve({ data: { stories: initialStories } });
# leanpub-end-insert
~~~~~~~

In the App component, instead of using the `initialStories`, use an empty array for the initial state. We want to start off with an empty list of stories and simulate fetching these stories asynchronously. In a new `useEffect` hook, call the function and resolve the returned promise as a side-effect. Due to the empty dependency array, the side-effect only runs once the component renders for the first time:

{title="src/App.jsx",lang="javascript"}
~~~~~~~
const App = () => {
  ...

# leanpub-start-insert
  const [stories, setStories] = React.useState([]);
# leanpub-end-insert

# leanpub-start-insert
  React.useEffect(() => {
    getAsyncStories().then(result => {
      setStories(result.data.stories);
    });
  }, []);
# leanpub-end-insert

  ...
};
~~~~~~~

Even though the data should arrive asynchronously when we start the application, it appears to arrive synchronously, because it's rendered immediately. Let's change this by giving it a bit of a realistic delay, because every network request to a remote API would come with a delay. First, remove the shorthand version for the promise:

{title="src/App.jsx",lang="javascript"}
~~~~~~~
const getAsyncStories = () =>
# leanpub-start-insert
  new Promise((resolve) =>
    resolve({ data: { stories: initialStories } })
  );
# leanpub-end-insert
~~~~~~~

And second, when resolving the promise, delay it for 2 seconds:

{title="src/App.jsx",lang="javascript"}
~~~~~~~
const getAsyncStories = () =>
  new Promise((resolve) =>
# leanpub-start-insert
    setTimeout(
      () => resolve({ data: { stories: initialStories } }),
      2000
    )
# leanpub-end-insert
  );
~~~~~~~

Once you start the application again, you should see a delayed rendering of the list. The initial state for the stories is an empty array and therefore nothing gets rendered in the List component. After the App component is rendered, the side-effect hook runs once to fetch the asynchronous data. After resolving the promise and setting the data in the component's state, the component renders again and displays the list of asynchronously loaded stories.

This section was only the first stepping stone to asynchronous data in React. Instead of having the data there from the beginning, we resolved the data asynchronously from a promise. However, we only moved our `stories` from being synchronous to asynchronous data. It's still sample data though and we will learn how to fetch real data eventually.

### Exercises:

* Compare your source code against the author's [source code](https://bit.ly/3R2obLU).
  * Recap all the [source code changes from this section](https://bit.ly/3QXCOjq).
  * Optional: If you are using TypeScript, check out the author's source code [here](https://bit.ly/3BNW79l).
* Optional: Read more about [JavaScript Promises](https://mzl.la/3aTGuQz).
* Read more about [faking a remote API with JavaScript](https://www.robinwieruch.de/javascript-fake-api/).
  * Read more about [using mock data in React](https://www.robinwieruch.de/react-mock-data/).
* Optional: [Leave feedback for this section](https://forms.gle/sfQcc477xmgGRLyB7).