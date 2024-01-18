## React Asynchronous Data

We have two interactions in our application: searching the list and removing items from the list. While the first interaction is a fluctuant modification through a third-party state (`searchTerm`) applied on the list, the second interaction is a non-reversible deletion of an item from the list. However, the list we are dealing with is still just sample data. What about preparing our application to deal with real data instead?

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

* Compare your source code against the author's [source code](https://bit.ly/3tQ2nO3).
  * Recap all the [source code changes from this section](https://bit.ly/425WXuH).
  * Optional: If you are using TypeScript, check out Robin's source code [here](https://bit.ly/3BNW79l).
* Optional: Read more about [JavaScript Promises](https://mzl.la/3aTGuQz).
* Read more about [faking a remote API with JavaScript](https://www.robinwieruch.de/javascript-fake-api/).
  * Read more about [using mock data in React](https://www.robinwieruch.de/react-mock-data/).
* Optional: [Leave feedback for this section](https://forms.gle/sfQcc477xmgGRLyB7).

### Interview Questions:

* Question: Why is handling asynchronous data common in React applications?
  * Answer: Client-side React applications often fetch data from remote sources.
* Question: What is the typical approach for rendering components before data fetching in React?
  * Answer: Components are often rendered before initiating data fetching, and conditional rendering or placeholder content is used until the data arrives.
* Question: How can you simulate asynchronous data fetching in React using sample data?
  * Answer: Simulating asynchronous data involves using functions that return promises, resolving with sample data, and later replacing it with real data.
* Question: What is the purpose of promises in handling asynchronous data in React?
  * Answer: Promises are used to manage asynchronous operations, allowing components to wait for data resolution before rendering.
* Question: Why is asynchronous data fetching essential for responsive user interfaces in React?
  * Answer: Asynchronous data fetching prevents blocking the UI, ensuring responsiveness, and enabling the display of updated information when available.
* Question: Can you replace simulated sample data with real data fetched from a remote API in React?
  * Answer: Yes, after simulating asynchronous data with sample data, it can be replaced seamlessly with real data fetched from a remote API.
* Question: What is the significance of using the useState hook when dealing with asynchronous data in React?
  * Answer: useState allows components to manage state changes, including loading states and the updated data received asynchronously.
* Question: How does React ensure that components re-render when asynchronous data arrives?
  * Answer: React's state management ensures that when asynchronous data arrives and state is updated, components re-render to reflect the new data.
