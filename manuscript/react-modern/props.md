## React Props

We are currently using the `list` variable as a global variable in the current application. We used it directly from the global scope in the App component, and later in the List component. This could work if you only had one global variable, but it isn't maintainable with multiple variables across multiple components. By using so called props in React, we can pass variables as information from one component to another component.

Before using props for the first time, we'll move the `list` from the global scope into the App component and rename it to its actual domain. Don't forget to refactor the App component's function from concise to block body in order to define the list in between:

{title="src/App.js",lang="javascript"}
~~~~~~~
# leanpub-start-insert
const App = () => {
  const stories = [
    {
      title: 'React',
      url: 'https://reactjs.org/',
      author: 'Jordan Walke',
      num_comments: 3,
      points: 4,
      objectID: 0,
    },
    {
      title: 'Redux',
      url: 'https://redux.js.org/',
      author: 'Dan Abramov, Andrew Clark',
      num_comments: 2,
      points: 5,
      objectID: 1,
    },
  ];

  return ( ... );
};
# leanpub-end-insert
~~~~~~~

Next, we'll use **React props** to pass the list of items to the List component. The variable is called `stories` in the App component, and we pass it under this name to the List component. In the List component's instantiation, however, it is assigned to the `list` HTML attribute:

{title="src/App.js",lang="javascript"}
~~~~~~~
const App = () => {
  const stories = [ ... ];

  return (
    <div>
      <h1>My Hacker Stories</h1>

      <Search />

      <hr />

# leanpub-start-insert
      <List list={stories} />
# leanpub-end-insert
    </div>
  );
};
~~~~~~~

After passing it to the List component, we can access it as `list` property from the `props` object in the List component's function signature:

{title="src/App.js",lang="javascript"}
~~~~~~~
# leanpub-start-insert
const List = (props) => (
# leanpub-end-insert
  <ul>
# leanpub-start-insert
    {props.list.map((item) => (
# leanpub-end-insert
      <li key={item.objectID}>
        <span>
          <a href={item.url}>{item.title}</a>
        </span>
        <span>{item.author}</span>
        <span>{item.num_comments}</span>
        <span>{item.points}</span>
      </li>
    ))}
  </ul>
);
~~~~~~~

Everything that we pass from a parent component to a child component via a component element's HTML attribute can be accessed in the child component. The child component receives an object parameter (`props`) which includes all the passed attributes as properties (props).

Using this mechanism of passing information from one component down to another with React props, we've prevented the list variable from polluting the global scope in our *src/App.js* file when we defined it outside of the component. Now, the list is defined as `stories` in our App component. However, since `stories` is not used in the App component directly, but in one of its child components, we passed it as props to the List component. We could also define `stories` directly in the List component and would not need to use props in the first place, however, in the future we will make use of the `stories` in the App component and thus will keep it there.

Another use case for React props is the List component and its potential child component. Previously, we couldn't extract an Item component from the List component, because we didn't know how to pass each individual item to the extracted Item component. With the new knowledge about React props, we can perform the component extraction and pass each item along to the List component's new child component:

{title="src/App.js",lang="javascript"}
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

In conclusion, one can see how props are used to pass information down the component tree. Following this explanation, information (props) can only be passed from a parent to a child component and not vice versa. We will learn how to overcome this limitation later.

### Exercises:

* Confirm your [source code](https://codesandbox.io/s/github/the-road-to-learn-react/hacker-stories/tree/2021/React-Props).
  * Confirm the [changes](https://github.com/the-road-to-learn-react/hacker-stories/compare/2021/Handler-Function-in-JSX...2021/React-Props).
* Read more about [how to pass props to React components](https://www.robinwieruch.de/react-pass-props-to-component).