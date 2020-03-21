## Meet another React Component

So far we've only been using the App component to create our applications. We used the App component in the last section to express everything needed to render our list in JSX, and it should scale with your needs and eventually handle more complex tasks. To help with this, we'll split some of its responsibilities into a standalone List component:

{title="src/App.js",lang="javascript"}
~~~~~~~
const list = [ ... ];

function App() { ... }

# leanpub-start-insert
function List() {
  return list.map(function(item) {
    return (
      <div key={item.objectID}>
        <span>
          <a href={item.url}>{item.title}</a>
        </span>
        <span>{item.author}</span>
        <span>{item.num_comments}</span>
        <span>{item.points}</span>
      </div>
    );
  });
}
# leanpub-end-insert
~~~~~~~

Optional: If this component looks odd, because the outermost part of the returned JSX starts with JavaScript. We could use it with a wrapping HTML element as well, but we'll continue with the previous version.

{title="src/App.js",lang="javascript"}
~~~~~~~
function List() {
# leanpub-start-insert
  return (
    <div>
      {list.map(function(item) {
# leanpub-end-insert
        return (... );
# leanpub-start-insert
      })}
    </div>
  );
# leanpub-end-insert
}
~~~~~~~

Now the new List component can be used in the App component:

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

You've just created your first React component! With this example, we can see how components that encapsulate meaningful tasks can work for larger React applications.

Larger React applications have **component hierarchies** (also called **component trees**). There is usually one uppermost **entry point component** (e.g. App) that spans a tree of components below it. The App is the **parent component** of the List, so the List is a **child component** of the App. In a component tree, the App is the **root component**, and the components that don't render any other components are called **leaf components** (e.g. List). The App can have multiple children, as can the List. If the App has another child component, the additional child component is called a **sibling component** of the List.

### Exercises:

* Confirm your [source code for the last section](https://codesandbox.io/s/github/the-road-to-learn-react/hacker-stories/tree/hs/Meet-another-React-Component).
  * Confirm the [changes from the last section](https://github.com/the-road-to-learn-react/hacker-stories/compare/hs/Lists-in-React...hs/Meet-another-React-Component?expand=1).
* Draw your components -- the App component and List component -- as a component tree on a sheet of paper. Extend this component tree with other possible components (e.g. extracted Search component for the input field and label in the App component). Try to figure out which other parts can be extracted as standalone components.
* If a Search component is used in the App component, would the Search component be a sibling, parent, or child component for the List component?
* Ask yourself what problems could arise if we keep treating the `list` variable as global variable. We will cover how to handle these problems in the upcoming sections.