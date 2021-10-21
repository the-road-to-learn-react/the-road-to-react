## Inline Handler in JSX

The list of stories we have in our App component is only an unstateful variable. We can filter the rendered list with the search feature, but the list itself stays intact even if we apply or remove the filter. The filtered list is just a derived state through a third party (here `searchTerm`), but we can't manipulate the real list yet. To gain control over the list, make it stateful by using it as initial state in React's useState Hook. The returned values from the array are the current state (`stories`) and the state updater function (`setStories`):

{title="src/App.js",lang="javascript"}
~~~~~~~
# leanpub-start-insert
const initialStories = [
  {
    title: 'React',
    ...
  },
  {
    title: 'Redux',
    ...
  },
];
# leanpub-end-insert

...

const App = () => {
  const [searchTerm, setSearchTerm] = ...

# leanpub-start-insert
  const [stories, setStories] = React.useState(initialStories);
# leanpub-end-insert

  ...
};
~~~~~~~

The application behaves the same because the `stories`, now returned as a stateful list from React's `useState` Hook, are still filtered into `searchedStories` and displayed in the List component. It's just the origin where the stories are coming from have changed. Next, we'll manipulate the list by removing an item from it:

{title="src/App.js",lang="javascript"}
~~~~~~~
const App = () => {
  ...

  const [stories, setStories] = React.useState(initialStories);

# leanpub-start-insert
  const handleRemoveStory = (item) => {
    const newStories = stories.filter(
      (story) => item.objectID !== story.objectID
    );

    setStories(newStories);
  };
# leanpub-end-insert

  ...

  return (
    <div>
      <h1>My Hacker Stories</h1>

      ...

      <hr />

# leanpub-start-insert
      <List list={searchedStories} onRemoveItem={handleRemoveStory} />
# leanpub-end-insert
    </div>
  );
};
~~~~~~~

The callback handler in the App component -- which will be used in the List/Item components eventually -- receives the item as an argument which should be removed from the list. Based on this information, the function filters the current stories by removing all items that don't meet its condition. The returned stories -- where the desired item (story) has been removed -- are then set as a new state and passed to the List component. Since a new state is set, the App component and all components below (e.g. List/Item components) will render again and thus display the new state of stories.

However, what's missing is how the List and Item components are using this new functionality which modifies the state in the App component. The List component itself does not use this new callback handler, but only passes it on to the Item component:

{title="src/App.js",lang="javascript"}
~~~~~~~
# leanpub-start-insert
const List = ({ list, onRemoveItem }) => (
# leanpub-end-insert
  <ul>
    {list.map((item) => (
      <Item
        key={item.objectID}
        item={item}
# leanpub-start-insert
        onRemoveItem={onRemoveItem}
# leanpub-end-insert
      />
    ))}
  </ul>
);
~~~~~~~

Finally, the Item component uses the incoming callback handler as a function in a new handler. In this handler, we will pass the specific item to it. Moreover, an additional button element is needed to trigger the actual event:

{title="src/App.js",lang="javascript"}
~~~~~~~
# leanpub-start-insert
const Item = ({ item, onRemoveItem }) => {
  const handleRemoveItem = () => {
    onRemoveItem(item);
  };

  return (
# leanpub-end-insert
    <li>
      <span>
        <a href={item.url}>{item.title}</a>
      </span>
      <span>{item.author}</span>
      <span>{item.num_comments}</span>
      <span>{item.points}</span>
# leanpub-start-insert
      <span>
        <button type="button" onClick={handleRemoveItem}>
          Dismiss
        </button>
      </span>
# leanpub-end-insert
    </li>
# leanpub-start-insert
  );
};
# leanpub-end-insert
~~~~~~~

So far in this section, we have made the list of stories stateful with React's useState Hook; passed the still searched stories down as props to the List component, and implemented a callback handler (`handleRemoveStory`) and handler (`handleRemoveItem`) to be used in their respective components to remove a story by clicking on a button. In order to implement this feature, we applied many lessons learned from before: state, props, handlers, and callback handlers.

You may have noticed that we had to introduce an additional `handleRemoveItem` handler in the Item component which is in charge to execute the incoming `onRemoveItem` callback handler. If you want to make this more elegant, you can use an **inline handler** which would allow you to execute the callback handler function in the Item component right in the JSX. There are two solutions using the incoming `onRemoveItem` function in the Item component as an inline handler. First, using JavaScript's bind method:

{title="src/App.js",lang="javascript"}
~~~~~~~
const Item = ({ item, onRemoveItem }) => (
  <li>
    <span>
      <a href={item.url}>{item.title}</a>
    </span>
    <span>{item.author}</span>
    <span>{item.num_comments}</span>
    <span>{item.points}</span>
    <span>
# leanpub-start-insert
      <button type="button" onClick={onRemoveItem.bind(null, item)}>
# leanpub-end-insert
        Dismiss
      </button>
    </span>
  </li>
);
~~~~~~~

Using [JavaScript's bind method](https://mzl.la/3ncEkBu) on a function allows us to bind arguments directly to that function that should be used when executing it. The bind method returns a new function with the bound argument attached. In contrast, the second and more popular solution is to use a wrapping arrow function, which allows us to sneak in arguments like `item`:

{title="src/App.js",lang="javascript"}
~~~~~~~
const Item = ({ item, onRemoveItem }) => (
  <li>
    <span>
      <a href={item.url}>{item.title}</a>
    </span>
    <span>{item.author}</span>
    <span>{item.num_comments}</span>
    <span>{item.points}</span>
    <span>
# leanpub-start-insert
      <button type="button" onClick={() => onRemoveItem(item)}>
# leanpub-end-insert
        Dismiss
      </button>
    </span>
  </li>
);
~~~~~~~

This is a quick solution, because sometimes we don't want to refactor a function component's concise function body back to a block body to define an appropriate handler between function signature and return statement. While this way is more concise than the others, it can also be more difficult to debug because JavaScript logic may be hidden in JSX. It becomes even more verbose if the wrapping arrow function encapsulates more than one line of implementation logic, by using a block body instead of a concise body. This should be avoided:

{title="Code Playground",lang="javascript"}
~~~~~~~
const Item = ({ item, onRemoveItem }) => (
  <li>
    ...
    <span>
      <button
        type="button"
        onClick={() => {
          // do something else

          // note: avoid using complex logic in JSX

          onRemoveItem(item);
        }}
      >
        Dismiss
      </button>
    </span>
  </li>
);
~~~~~~~

All three handler versions, two of which are inline and the normal handler, are acceptable. The non-inlined handler moves the implementation details into the function component's block body; both inline handler versions move the implementation details into the JSX.

### Exercises:

* Confirm your [source code](https://bit.ly/3vrGWzb).
  * Confirm the [changes](https://bit.ly/3jj37CR).
* Read more about how to [add](https://www.robinwieruch.de/react-add-item-to-list), [update](https://www.robinwieruch.de/react-update-item-in-list), [remove](https://www.robinwieruch.de/react-remove-item-from-list) items in a list.
* Read more about [computed properties in React](https://www.robinwieruch.de/react-computed-properties).
* Review handlers, callback handlers, and inline handlers.
* Optional: [Leave feedback for this section](https://forms.gle/19NvNYMk2RUKTDyZ6).