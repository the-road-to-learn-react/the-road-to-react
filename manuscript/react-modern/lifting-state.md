## Lifting State in React

In this section, we are confronted with the following task: Use the `searchTerm` from the Search component to filter the `stories` by title in the App component before they reach the List component. In the last section, we established a callback handler to pass information from the Search component up to the App component. However, if you check the code, it seems somehow difficult to access the value coming from the callback handler's event in the App component in order to filter the list. The incoming value is only accessible in the callback handler's function. We need to figure out how to share the Search component's state across multiple components (App, List) who are interested in it. Therefore, we'll need to **lift state up** from Search to App component to share the state with more components:

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

We learned about the callback handler previously, because it helps us to keep an open communication channel from Search to App component. Now the Search component doesn't manage the state anymore, but only passes up the event to the App component via a callback handler after the text is entered into the HTML input field. You could also display the `searchTerm` again in the App component (from state, when using `searchTerm` directly) or Search component (from props, when passing the `searchTerm` state down as props).

Rule of thumb: Always manage state at a component level where every component that's interested in it is one that either manages the state (using information directly from state, e.g. App component) or a component below the managing component (using information from props, e.g. List or Search). If a component below needs to update the state (e.g. Search), pass a callback handler down to it which allows it to update it. If a component needs to use the state (e.g. displaying it), pass it down as props.

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
* Optional: [Rate this section](https://forms.gle/EqJGjxCM1Xzw9S6g7).