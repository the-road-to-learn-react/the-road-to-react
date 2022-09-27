## Inline Handler in JSX

While this section will teach you about inline handlers as new fundamental building block in React, we will implement our next feature which allows us to remove items from the list. Without reading any further, you can tackle this task yourself, because it can be solved without the knowledge about inline handlers too. What follows are a couple of instructions. If you get stuck, continue to read this section for the solution. If you found a solution yourself, compare it to the solution from the book which uses inline handlers.

**Task:** The application renders a list of items and allows its users to filter the list via a search feature. Next the application should render a button next to each list item which allows its users to remove the item from the list.

**Optional Hints:**

* The list of items needs to become a stateful value in order to manipulate it (e.g. removing an item) later.
* Every list item renders a button with a click handler. When clicking the button, the item gets removed from the list by manipulating the state.
* Since the stateful list resides in the App component, one needs to use callback handlers to enable the Item component to communicate up to the App component for removing an item by its identifier.

Now we want to check out how to implement this feature step by step. At the moment, the list of items (here: `stories`) that we have in our App component is an unstateful variable. We can filter the rendered list with the search feature, but the list itself stays intact. The filtered list is just a derived state through a third-party (here: `searchTerm`), but we do not manipulate the actual list yet. To gain control over the list, make it stateful by using it as initial state in React's useState Hook. The returned values from the array are the current state (`stories`) and the state updater function (`setStories`):

{title="src/App.jsx",lang="javascript"}
~~~~~~~
# leanpub-start-insert
const initialStories = [
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

The application behaves the same because the `stories`, now returned as a stateful list from React's `useState` Hook, are still filtered into `searchedStories` and displayed in the List component. Just the origin where the stories are coming from has changed. Next, we will write an event handler which removes an item from the list:

{title="src/App.jsx",lang="javascript"}
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

However, what's missing is how the List/Item components are using this new functionality which modifies the state in the App component. The List component itself does not use this new callback handler, but only passes it on to the Item component:

{title="src/App.jsx",lang="javascript"}
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

{title="src/App.jsx",lang="javascript"}
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

So far in this section, we have made the list of stories stateful with React's useState Hook, passed the still searched stories down as props to the List component, and implemented a callback handler (`handleRemoveStory`) and handler (`handleRemoveItem`) to be used in their respective components to remove a story by clicking on a button. In order to implement this feature, we applied many lessons learned from before: state, props, handlers, and callback handlers. The feature works and you may have arrived at the same or a similar solution yourself.

Let's enter the topic of inline handlers: You may have noticed that we had to introduce an additional `handleRemoveItem` handler in the Item component which is in charge of executing the incoming `onRemoveItem` callback handler. We had to introduce this extra event handler to pick up the item as argument for the callback handler.

If you want to make this more elegant though, you can use an **inline handler** which allows you to execute the callback handler function in the Item component right in the JSX. There are two solutions using the incoming `onRemoveItem` function in the Item component as an inline handler. First, using JavaScript's bind method:

{title="src/App.jsx",lang="javascript"}
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

Using [JavaScript's bind method](https://mzl.la/3ncEkBu) on a function allows us to bind arguments directly to that function that should be used when executing it. The bind method returns a new function with the bound argument attached. In contrast, the second and more popular solution is to use an inline arrow function, which allows us to sneak in arguments like the `item`:

{title="src/App.jsx",lang="javascript"}
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

While using an inline handler is more concise than using a normal event handler, it can also be more difficult to debug because JavaScript logic may be hidden in JSX. It becomes even more verbose if the inline arrow function encapsulates more than one line of implementation logic by using a block body instead of a concise body:

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

As a rule of thumb: It's okay to use inline handlers if they do not obscure critical implementation details. If inline handlers need to use a block body, because there are more than one line of code executed, it's about time to extract them as normal event handlers. After all, in this case all handler versions are readable and therefore acceptable.

### Exercises:

* Compare your source code against the author's [source code](https://bit.ly/3DKaWfy).
  * Recap all the [source code changes from this section](https://bit.ly/3SqUtku).
  * Optional: If you are using TypeScript, check out the author's source code [here](https://bit.ly/3BKP8hv).
* Read more about how to [add](https://www.robinwieruch.de/react-add-item-to-list), [update](https://www.robinwieruch.de/react-update-item-in-list/), [remove](https://www.robinwieruch.de/react-remove-item-from-list) items in a list.
* Read more about [computed properties in React](https://www.robinwieruch.de/react-computed-properties/).
* Review [handlers, callback handlers, and inline handlers](https://www.robinwieruch.de/react-event-handler/).
* Optional: [Leave feedback for this section](https://forms.gle/19NvNYMk2RUKTDyZ6).