## Reverse Sort

**Task:** The sort feature works, but the ordering only includes one direction. Implement a reverse sort when a sort button is clicked twice, so it becomes a toggle between normal (ascending) and reverse (descending) sort.

**Optional Hints:**

* Consider that reverse or normal sort could be just another state (e.g. `isReverse`) next to the `sortKey`.
* Set the new state in the `handleSort` handler based on the previous sort.
* Use the new `isReverse` state for sorting the list with the sort function from the dictionary with the optionally applied `reverse()` function from JavaScript arrays.

Let's get to the task. The initial sort direction works for strings, as well as numeric sorts like the reverse sort for JavaScript numbers that arranges them from high to low. Now we need another state to track whether the sort is reversed or normal:

{title="src/App.jsx",lang="javascript"}
~~~~~~~
const List = ({ list, onRemoveItem }) => {
# leanpub-start-insert
  const [sort, setSort] = React.useState({
    sortKey: 'NONE',
    isReverse: false,
  });
# leanpub-end-insert

  ...
};
~~~~~~~

Next, give the sort handler logic to see if the incoming `sortKey` triggers are a normal or reverse sort. If the `sortKey` is the same as the one in the state, it should be a reverse sort, but only if the sort state wasn't already reversed:

{title="src/App.jsx",lang="javascript"}
~~~~~~~
const List = ({ list, onRemoveItem }) => {
  const [sort, setSort] = React.useState({
    sortKey: 'NONE',
    isReverse: false,
  });

  const handleSort = (sortKey) => {
# leanpub-start-insert
    const isReverse = sort.sortKey === sortKey && !sort.isReverse;

    setSort({ sortKey: sortKey, isReverse: isReverse });
# leanpub-end-insert
  };

# leanpub-start-insert
  const sortFunction = SORTS[sort.sortKey];
# leanpub-end-insert
  const sortedList = sortFunction(list);

  return (
    ...
  );
};
~~~~~~~

Lastly, depending on the new `isReverse` state, apply the sort function from the dictionary with or without the built-in JavaScript reverse method for arrays:

{title="src/App.jsx",lang="javascript"}
~~~~~~~
const List = ({ list, onRemoveItem }) => {
  const [sort, setSort] = React.useState({
    sortKey: 'NONE',
    isReverse: false,
  });

  const handleSort = (sortKey) => {
    const isReverse = sort.sortKey === sortKey && !sort.isReverse;

# leanpub-start-insert
    setSort({ sortKey, isReverse });
# leanpub-end-insert
  };

  const sortFunction = SORTS[sort.sortKey];
# leanpub-start-insert
  const sortedList = sort.isReverse
    ? sortFunction(list).reverse()
    : sortFunction(list);
# leanpub-end-insert

  return (
    ...
  );
};
~~~~~~~

The reverse sort is now operational! Congratulations, you have a fully sortable list now. And by the way: For the object passed to the state updater function, we use what is called a **shorthand object initializer notation**:

{title="Code Playground",lang="javascript"}
~~~~~~~
const firstName = 'Robin';

const user = {
  firstName: firstName,
};

console.log(user);
// { firstName: "Robin" }
~~~~~~~

When the property name in your object is the same as your variable name, you can omit the key/value pair and just write the name:

{title="Code Playground",lang="javascript"}
~~~~~~~
const firstName = 'Robin';

const user = {
  firstName,
};

console.log(user);
// { firstName: "Robin" }
~~~~~~~

If necessary, read more about [JavaScript Object Initializers](https://mzl.la/2XuN651).

### Exercises:

* Compare your source code against the author's [source code](https://bit.ly/3SFSBEV).
  * Recap all the [source code changes from this section](https://bit.ly/3fgWqBT).
* Consider the drawback of keeping the sort state in the List instead of the App component. If you don't know, sort the list by "Title" and search for other stories afterward. What would be different if the sort state would be in the App component.
* Use your styling skills to give the user feedback about the current active sort and its reverse state. It could be an [arrow up or arrow down SVG](https://bit.ly/3lXfLZN) next to each active sort button.
* Optional: [Leave feedback for this section](https://forms.gle/ZoJSHFJf2swcBHXM6).