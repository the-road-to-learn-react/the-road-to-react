## React Props

Currently we are using the `list` as a global variable in our project. At the beginning, we used it directly from the global scope in the App component and later in the List component. This could work if you only had one global variable, but it isn't maintainable with multiple variables across multiple components (within multiple folders/files). By using so-called props in React, we can pass variables as information from one component to another component. Let's explore how this works.

Before using props for the first time, we'll move the `list` from the global scope into the App component and give it a self-descriptive name. Don't forget to refactor the App component's function from concise to block body in order to declare the `list` prior to the return statement:

{title="src/App.jsx",lang="javascript"}
~~~~~~~
# leanpub-start-insert
const App = () => {
  const stories = [
    {
      title: 'React',
      url: 'https://reactjs.org/',
      author: 'Jordan Walke',
      ...
    },
    {
      title: 'Redux',
      url: 'https://redux.js.org/',
      author: 'Dan Abramov, Andrew Clark',
      ...
    },
  ];

  return ( ... );
};
# leanpub-end-insert
~~~~~~~

Next, we'll use **React props** to pass the list of items to the List component. The variable is called `stories` in the App component and we pass it under this name to the List component. However, in the List component's instantiation, it is assigned to the `list` HTML attribute:

{title="src/App.jsx",lang="javascript"}
~~~~~~~
const App = () => {
  const stories = [ ... ];

  return (
    <div>
      ...

# leanpub-start-insert
      <List list={stories} />
# leanpub-end-insert
    </div>
  );
};
~~~~~~~

Now try yourself to retrieve the `list` from the List component's function signature by introducing a parameter. If you find the solution yourself, congratulations for passing your first information from one component to another. If not, the following code snippet shows how it works:

{title="src/App.jsx",lang="javascript"}
~~~~~~~
# leanpub-start-insert
const List = (props) => (
# leanpub-end-insert
  <ul>
# leanpub-start-insert
    {props.list.map((item) => (
# leanpub-end-insert
      <li key={item.objectID}>
        ...
      </li>
    ))}
  </ul>
);
~~~~~~~

Everything that we pass from a parent component to a child component via the component element's HTML attribute can be accessed in the child component. The child component receives a parameter (`props`) as object in its function signature which includes all the passed attributes as properties (short: props).

Note that at this point, we could also define `stories` directly in the List component and would not need to pass them as props from the App component; however, in the future we will make use of the `stories` in the App component and thus will keep them there. In addition, this was a great learning exercise to get to know props in React.

Another use case for React props is the List component and its potential child component. Previously, we couldn't extract an Item component from the List component, because we didn't know how to pass each item to the extracted Item component. With this new knowledge about React props, we can perform the component extraction and pass each item along to the List component's new child component.

Before you check the following solution, try it yourself: extract an Item component from the List component and pass each `item` in the `map()` method's callback function to this new component. If you don't come up with a solution yourself after some time, check out how the book implements it:

{title="src/App.jsx",lang="javascript"}
~~~~~~~
const List = (props) => (
  <ul>
    {props.list.map((item) => (
# leanpub-start-insert
      <Item key={item.objectID} item={item} />
# leanpub-end-insert
    ))}
  </ul>
);

# leanpub-start-insert
const Item = (props) => (
  <li>
    <span>
      <a href={props.item.url}>{props.item.title}</a>
    </span>
    <span>{props.item.author}</span>
    <span>{props.item.num_comments}</span>
    <span>{props.item.points}</span>
  </li>
);
# leanpub-end-insert
~~~~~~~

At the end, you should see the list rendering again. The most important fact about props: it's not allowed to change them, because they should be treated as an immutable data structure. They are only used to pass information *down* the component hierarchy. Continuing this thought, information (props) *can only* be passed from a parent to a child component and not vice versa. We will learn how to overcome this limitation later. For now, we have found our vehicle to share information from top to bottom in a React component tree.

### Exercises:

* Compare your source code against the author's [source code](https://bit.ly/3BZnazZ).
  * Recap all the [source code changes from this section](https://bit.ly/3DYIiI3).
  * Optional: If you are using TypeScript, check out the author's source code [here](https://bit.ly/3DSLIvL).
* Read more about [how to use props in React](https://www.robinwieruch.de/react-pass-props-to-component/).
* Optional: [Leave feedback for this section](https://forms.gle/APwaUSAuVAAA56sY6).
