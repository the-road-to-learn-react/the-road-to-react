## Meet another React Component

Components are the foundation of every React application. So far we've only been using the App component. This will not end up well, because components should scale with your application's size. So instead of making one component larger and more complex, we will split one component into multiple components eventually. We'll start with a new List component which extracts functionalities from the App component:

{title="src/App.js",lang="javascript"}
~~~~~~~
const list = [ ... ];

function App() { ... }

# leanpub-start-insert
function List() {
  return (
    <ul>
      {list.map(function (item) {
        return (
          <li key={item.objectID}>
            <span>
              <a href={item.url}>{item.title}</a>
            </span>
            <span>{item.author}</span>
            <span>{item.num_comments}</span>
            <span>{item.points}</span>
          </li>
        );
      })}
    </ul>
  );
}
# leanpub-end-insert
~~~~~~~

Then the new List component can be used in the App component where we have been using the inlined functionality previously:

{title="src/App.js",lang="javascript"}
~~~~~~~
function App() {
  return (
    <div>
      <h1>My Hacker Stories</h1>

      <label htmlFor="search">Search: </label>
      <input id="search" type="text" />

      <hr />

# leanpub-start-insert
      <List />
# leanpub-end-insert
    </div>
  );
}
~~~~~~~

You've just created your first React component! With this example in mind, we can see how components encapsulate meaningful tasks while contributing to the greater good of a larger React application. Extracting a component is a task that you will perform very often as a React developer, because it's always the case that a component will grow in size and complexity. Let's do this extraction of a component one more time for a so-called Search component:

{title="src/App.js",lang="javascript"}
~~~~~~~
function App() {
  return (
    <div>
      <h1>My Hacker Stories</h1>

# leanpub-start-insert
      <Search />
# leanpub-end-insert

      <hr />

      <List />
    </div>
  );
}

# leanpub-start-insert
function Search() {
  return (
    <div>
      <label htmlFor="search">Search: </label>
      <input id="search" type="text" />
    </div>
  );
}
# leanpub-end-insert
~~~~~~~

Finally, we have three components in our application: App, List, and Search. Generally speaking, a React application consists of many hierarchical components; which we can put into the following categories:

![](images/component-tree.png)

React applications have **component hierarchies** (also called **component trees**). There is usually one uppermost **entry point component** (e.g. App) that spans a tree of components below it. The App is the **parent component** of the List and Search, so the List and Search are **child components** of the App component and **sibling components** to each other. The illustration takes it one step further where the Item component is a child component of the List. In a component tree, there is always a **root component** (e.g. App) and the components that don't render any other components are called **leaf components** (e.g. List/Search component in our current source code or Item/Search component from the illustration). All components can have zero, one, or many child components.

### Exercises:

* Confirm your [source code](https://codesandbox.io/s/github/the-road-to-learn-react/hacker-stories/tree/2021/Meet-another-React-Component).
  * Confirm the [changes](https://github.com/the-road-to-learn-react/hacker-stories/compare/2021/Lists-in-React...2021/Meet-another-React-Component).
* Ask yourself:
  * What problem could arise if we keep treating the `list` variable as global variable. Think about a way how to prevent it.
  * We can't extract an Item component from the List component (like in the illustration) yet, because we don't know how to pass individual items from the list to each Item component. Think about a way to do it.
