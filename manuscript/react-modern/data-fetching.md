## Data Fetching with React

We set everything up for asynchronous data fetching React. However, we are still using pseudo data coming from a promise we set up ourselves for a fake API. Still, all lessons up to now about asynchronous React and advanced state management were preparing us to fetch data from a real remote third-party API. In this section, we will use the informative [Hacker News API](https://hn.algolia.com/api) to request popular tech stories.

If you are familiar how to fetch data in JavaScript, you can try to accomplish the following task yourself and check later the implementation from the book. However, do not hesitate to continue with the book, because this is a tough task.

**Task:** The application uses asynchronous yet pseudo data from a promise (fake API). Instead of using the `getAsyncStories()` function, use the Hacker News API to fetch the data.

**Optional Hints:**

* Use this `https://hn.algolia.com/api/v1/search?query=React` API endpoint of the Hacker News API.
* Remove the `initialStories` variable, because this data will come from the API.
* Use the browser's [native fetch API](https://mzl.la/2Z1kyjU) to perform the request.
* Note: A successful or erroneous request uses the same implementation logic that we already have in place.

We start with a great foundation for fetching asynchronous data, because everything is already in place. The only thing that keeps us away from the solution is using sample data instead of real world data. Therefore, the next code snippet shows everything you need to change to connect to a remote API. Instead of using the `initialStories` array and `getAsyncStories` function, which can be removed now, we will fetch the data directly from the API:

{title="src/App.jsx",lang="javascript"}
~~~~~~~
// A
# leanpub-start-insert
const API_ENDPOINT = 'https://hn.algolia.com/api/v1/search?query=';
# leanpub-end-insert

const App = () => {
  ...

  React.useEffect(() => {
    dispatchStories({ type: 'STORIES_FETCH_INIT' });

# leanpub-start-insert
    fetch(`${API_ENDPOINT}react`) // B
      .then((response) => response.json()) // C
# leanpub-end-insert
      .then((result) => {
        dispatchStories({
          type: 'STORIES_FETCH_SUCCESS',
# leanpub-start-insert
          payload: result.hits, // D
# leanpub-end-insert
        });
      })
      .catch(() =>
        dispatchStories({ type: 'STORIES_FETCH_FAILURE' })
      );
  }, []);

  ...
};
~~~~~~~

First, the `API_ENDPOINT` (A) is used to fetch popular tech stories for a certain query (a search term). In this case, we fetch stories about React (B). Second, the native browser's [fetch API](https://mzl.la/2Z1kyjU) is used to make this request (B). For the fetch API, the response needs to be translated into JSON (C). Finally, the returned result has a different data structure (D), which we send as payload to our component's state reducer.

In the previous code example, we used [JavaScript's Template Literals](https://mzl.la/3jlcVfn) for a string interpolation. When this feature wasn't available in JavaScript, we'd have used the + operator on strings instead:

{title="Code Playground",lang="javascript"}
~~~~~~~
const greeting = 'Hello';

// + operator
const welcome = greeting + ' React';
console.log(welcome);
// Hello React

// template literals
const anotherWelcome = `${greeting} React`;
console.log(anotherWelcome);
// Hello React
~~~~~~~

Check your browser to see stories related to the initial query fetched from the Hacker News API. Since we used the same data structure for the sample stories, we didn't need to change anything in the Item component. It's still possible to filter the stories after fetching them with the search feature, because they still have a `title` property. We will change this behavior in one of the next sections though.

### Exercises:

* Compare your source code against the author's [source code](https://bit.ly/3DICIsS).
  * Recap all the [source code changes from this section](https://bit.ly/3R4EaJ2).
  * Optional: If you are using TypeScript, check out the author's source code [here](https://bit.ly/3LK6OhQ).
* Read through [Hacker News](https://news.ycombinator.com) and its [API](https://hn.algolia.com/api).
* Optional: Read more about [JavaScript's Template Literals](https://mzl.la/3jlcVfn).
* Optional: [Leave feedback for this section](https://forms.gle/hoJxjjpoZQGCS7Vp9).