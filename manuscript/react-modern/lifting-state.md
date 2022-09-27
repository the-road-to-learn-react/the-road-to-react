## Lifting State in React

In this section, we are confronted with the following task: Use the stateful `searchTerm` from the Search component to filter the `stories` by their `title` property in the App component before they are passed as props to the List component. So far, we have learned about how to pass information down explicitly with props and how to pass information up implicitly with callback handlers. However, if you look at the callback handler in the App component, it doesn't come natural to one on how to apply the `searchTerm` from the App component's `handleSearch()` handler as filter to the `stories`.

One solution could be establishing another state in the App component which captures the arriving `searchTerm` in the App component and then uses it for filtering the `stories` before they are passed to the List component as props. However, this adds duplication as a bad practice, because the `searchTerm` would have a state in the Search and App components then. So think about it another way: If the App component is interested in the `searchTerm` state to filter the `stories`, why not instantiate the state in the App component instead of in the Search component in the first place?

Try it yourself: Move the state from the Search component to the App component, pass the state updater function to the Search component as callback handler and use it to update the state when a user types into the input field. Then use the new state in the App component to `filter()` the `stories` before they are passed to the List component. The following implementation demonstrates the first part of the solution:

{title="src/App.jsx",lang="javascript"}
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

We learned about the callback handler previously, because it helps us to keep an open communication channel from child component (here: Search component) to parent component (here: App component). Now, the Search component doesn't manage the state anymore, but only passes up the event to the App component via a callback handler after the text is entered into the HTML input field. From there, the App component updates its state. The process of moving state from one component to another, like we did in the last code snippet, is called **lifting state**. Next, you could still display the `searchTerm` again in the App component (from state, when using `searchTerm`) or Search component (from props, when passing the `searchTerm` state down as props).

![](images/component-communication.png)

Rule of thumb: Always manage state at a component level where every component that's interested in it is one that either manages the state (using information directly from state, e.g. App component) or a component below the state managing component (using information from props, e.g. List or Search components). If a component below needs to update the state (e.g. Search), pass a callback handler down to it which allows this particular component to update the state above in the parent component. If a component below needs to use the state (e.g. displaying it), pass it down as props.

Finally, by managing the search state in the App component, we can filter the `stories` with the stateful `searchTerm` before passing them as `list` prop to the List component. Try it yourself by using the array's built-in `filter()` method in combination with the `stories` and the `searchTerm` before consulting the following implementation:

{title="src/App.jsx",lang="javascript"}
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

Here, the [JavaScript array's built-in filter method](https://mzl.la/3BYFAOR) is used to create a new filtered array. The `filter()` method takes a function as an argument, which accesses each item in the array and returns `true` or `false`. If the function returns `true`, meaning the condition is met, the item stays in the newly created array; if the function returns `false`, it's removed:

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

The `filter()` method could be made more concise by using an arrow function with an immediate return:

{title="src/App.jsx",lang="javascript"}
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

That's all to the refactoring steps of the inlined function for the `filter()` method. There are many variations to it -- and it's not always simple to keep a good balance between readability and conciseness -- however, I feel like keeping it concise whenever possible keeps it most of the time readable as well.

What's not working very well yet: The `filter()` method checks whether the `searchTerm` is present as string in the `title` property of each story, but it's case sensitive. If we search for "react", there is no filtered "React" story in your rendered list. Try to fix this problem yourself by making the `filter()` method's condition case insensitive. The following code snippet shows you how to achieve it by lower casing the `searchTerm` and the `title` of the story:

{title="src/App.jsx",lang="javascript"}
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

![](images/state-where.png)

After all, knowing where to instantiate state in React turns out to be an important skill in every React developer's career. The state should always be there where all components which depend on the state can read (via props) and update (via callback handler) it. These are all descendant components of the component which instantiates the state.

### Exercises:

* Compare your source code against the author's [source code](https://bit.ly/3dCKU3f).
  * Recap all the [source code changes from this section](https://bit.ly/3DG8Op8).
  * Optional: If you are using TypeScript, check out the author's source code [here](https://bit.ly/3SEqxl1).
* Read more about [lifting state in React](https://www.robinwieruch.de/react-lift-state/).
* Optional: [Leave feedback for this section](https://forms.gle/EqJGjxCM1Xzw9S6g7).