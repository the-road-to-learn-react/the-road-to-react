## Lifting State in React

In this section, we are confronted with the following task: Use the `searchTerm` from the Search component to filter the `stories` by their `title` property in the App component before they are passed as props to the List component. So far, we have learned about how to pass information down explicilty with props and how to pass information up implictly with callback handlers. However, if you look at callback handler in the App component, it doesn't come natural to one on how to apply the `searchTerm` from the App component's `handleSearch()` event handler as filter for the `stories`.

One solution could be establishing another state in the App component which captures the arriving `searchTerm` in the App component and then uses it for filtering the `stories` before they are passed to the List component as props. However, this adds duplication, because the `searchTerm` would have a state in the Search and App components then. So think about it another way: If the App component is interested in the `searchTerm` state to filter the `stories`, why not instantiate the state in the App component in the first place?

Try it yourself: **Lift the state** from the Search component to the App component, pass the state updater function to the Search component and use it to update the state when a user types into the input field, and use the new state in the App component to `filter()` the `stories` before they are passed to the List component. The following implementation will demonstrate this solution:

{title="src/App.js",lang="javascript"}
~~~~~~~
const App = () => {
  const stories = [ ... ];

# leanpub-start-insert
  const [searchTerm, setSearchTerm] = React.useState('');
# leanpub-end-insert

  const handleSearch = (event) => {
# leanpub-start-insert
    setSearchTerm(event.target.value);
# leanpub-end-insert
  };

  return (
    <div>
      <h1>My Hacker Stories</h1>

      <Search onSearch={handleSearch} />

      <hr />

      <List list={stories} />
    </div>
  );
};

const Search = (props) => (
  <div>
    <label htmlFor="search">Search: </label>
# leanpub-start-insert
    <input id="search" type="text" onChange={props.onSearch} />
# leanpub-end-insert
  </div>
);
~~~~~~~

We learned about the callback handler previously, because it helps us to keep an open communication channel from child component (here: Search component) to parent component (here: App component). In the recent version of the code, the Search component doesn't manage the state anymore, but only passes up the event to the App component via a callback handler after the text is entered into the HTML input field. From here, you still could also display the `searchTerm` again in the App component (from state, when using `searchTerm` directly) or Search component (from props, when passing the `searchTerm` state down as props).

Rule of thumb: Always manage state at a component level where every component that's interested in it is one that either manages the state (using information directly from state, e.g. App component) or a component below the state managing component (using information from props, e.g. List or Search components). If a component below needs to update the state (e.g. Search), pass a callback handler down which allows this particular component to update the state above in the parent component. If a component needs to use the state (e.g. displaying it), you can pass it down as props too.

![](images/component-communication.png)

Finally, by managing the search feature's state in the App component, we can filter the `stories` with the stateful `searchTerm` before passing them as `list` to the List component:

{title="src/App.js",lang="javascript"}
~~~~~~~
const App = () => {
  const stories = [ ... ];

  const [searchTerm, setSearchTerm] = React.useState('');

  const handleSearch = (event) => {
    setSearchTerm(event.target.value);
  };

# leanpub-start-insert
  const searchedStories = stories.filter(function (story) {
    return story.title.includes(searchTerm);
  });
# leanpub-end-insert

  return (
    <div>
      <h1>My Hacker Stories</h1>

      <Search onSearch={handleSearch} />

      <hr />

# leanpub-start-insert
      <List list={searchedStories} />
# leanpub-end-insert
    </div>
  );
};
~~~~~~~

Here, the [JavaScript array's built-in filter function](https://mzl.la/3BYFAOR) is used to create a new filtered array. The filter function takes a function as an argument, which accesses each item in the array and returns true or false. If the function returns true, meaning the condition is met, the item stays in the newly created array; if the function returns false, it's removed:

{title="Code Playground",lang="javascript"}
~~~~~~~
const words = [
  'spray',
  'limit',
  'elite',
  'exuberant',
  'destruction',
  'present'
];

const filteredWords = words.filter(function (word) {
  return word.length > 6;
});

console.log(filteredWords);
// ["exuberant", "destruction", "present"]
~~~~~~~

The filter function can be made more concise by using an arrow function with an immediate return:

{title="src/App.js",lang="javascript"}
~~~~~~~
const App = () => {
  ...

# leanpub-start-insert
  const searchedStories = stories.filter((story) =>
    story.title.includes(searchTerm)
  );
# leanpub-end-insert

  ...
};
~~~~~~~

That's all to the refactoring steps of the inlined function for the filter function. There are many variations to it -- and it's not always simple to keep a good balance between readable and conciseness -- however, I feel like keeping it concise whenever possible keeps it most of the time readable as well.

What's not working very well yet: The filter function checks whether the `searchTerm` is present in our story item's title, but it's still too opinionated about the letter case. If we search for "react", there is no filtered "React" story in your rendered list. To fix this problem, we have to lower case the story's title and the `searchTerm` to make them equal.

{title="src/App.js",lang="javascript"}
~~~~~~~
const App = () => {
  ...

  const searchedStories = stories.filter((story) =>
# leanpub-start-insert
    story.title.toLowerCase().includes(searchTerm.toLowerCase())
# leanpub-end-insert
  );

  ...
};
~~~~~~~

Now you should be able to search for "eact", "React", or "react" and see one of two displayed stories. Congratulations, you have just added your first real interactive feature to your application by leveraging state -- to derive a filtered list of stories -- and props -- by passing a callback handler to the Search component.

### Exercises:

* Confirm your [source code](https://bit.ly/3vtfBwo).
  * Confirm the [changes](https://bit.ly/3DSiuK6).
* Read more about [lifting state in React](https://www.robinwieruch.de/react-lift-state).
* Optional: [Leave feedback for this section](https://forms.gle/EqJGjxCM1Xzw9S6g7).