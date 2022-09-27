## Meet another React Component

Components are the foundation of every React application. With a growing React project, you will get more and more components to manage. Each component encapsulates functionalities (e.g. rendering a list of items). So far we've only been using the App component. This will not end up well, because components should scale with your application's size. So instead of making one component larger and more complex over time, we'll split one component into multiple components eventually. Therefore, we'll start with a new List component which extracts functionalities from the App component:

{title="src/App.jsx",lang="javascript"}
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

Then the new List component can be used in the App component where we have been using the inlined HTML elements previously:

{title="src/App.jsx",lang="javascript"}
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

You've just created your first React component. With this example in mind, we can see how components encapsulate meaningful tasks while contributing to the greater good of a larger React project. Extracting a component is a task that you will perform very often as a React developer, because it's always the case that a component will grow in size and complexity. Go ahead and extract the label and input elements from the App component into their own Search component. The following code snippet shows how the book would solve this task:

{title="src/App.jsx",lang="javascript"}
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

Finally, we have three components in our application: App, List, and Search. Generally speaking, a React application consists of many hierarchical components; which we can put into the following categories. The following illustration assumes that we have split out an Item component from the List component as well -- which helps us to clarify the taxonomie.

![](images/component-tree.png)

React applications have **component hierarchies** (also called **component trees**). There is usually one uppermost **entry point component** (e.g. App) that spans a tree of components below it. The App is the **parent component** of the List and Search, so the List and Search are **child components** of the App component and **sibling components** to each other. The illustration takes it one step further where the Item component is a child component of the List. In a component tree, there is always a **root component** (e.g. App), and the components that don't render any other components are called **leaf components** (e.g. List/Search component in our current source code or Item/Search component from the illustration). All components can have zero, one, or many child components.

You can see how a React application grows in size by creating more components which are connected in one hierarchy. Usually you will start out with the App component from where you grow your component tree. Either you know the components you wanna create beforehand or you start with one component and extract components from it eventually. For a beginner, it may be difficult to know when to create a new component or when to extract a component from another component. Usually it happens naturally whenever a component gets too big in size/complexity or whenever you see natural boundaries in domains/functionality (e.g. List component renders a list of items, Search component renders a search form). In the end, each component represents a single unit in your application which makes the application maintainable and predictable.

### Exercises:

* Compare your source code against the author's [source code](https://bit.ly/3R0Aw35).
  * Recap all the [source code changes from this section](https://bit.ly/3f5pTi7).
  * Optional: If you are using TypeScript, check out the author's source code [here](https://bit.ly/3RlJKXM).
* We can't extract an Item component from the List component (like in the illustration) yet, because we don't know how to pass individual items from the list to each Item component. Think about a way to do it.
* Optional: [Leave feedback for this section](https://forms.gle/EZENmy48zvDP82NL7).