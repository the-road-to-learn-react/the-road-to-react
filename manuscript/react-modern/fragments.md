## React Fragments

You may have noticed that all of our React components return JSX with one top-level HTML element. When we introduced the Search component a while ago, we had to add a `<div>` tag (read: container element), because otherwise the label and input elements couldn't be returned side-by-side without a wrapping top-level element:

{title="src/App.jsx",lang="javascript"}
~~~~~~~
const Search = ({ search, onSearch }) => (
# leanpub-start-insert
  <div>
# leanpub-end-insert
    <label htmlFor="search">Search: </label>
    <input
      id="search"
      type="text"
      value={search}
      onChange={onSearch}
    />
# leanpub-start-insert
  </div>
# leanpub-end-insert
);
~~~~~~~

However, there are ways to render multiple top-level elements side-by-side. A rarely used approach returns all sibling elements as an array. Since this resembles a list of elements, we would have to give each list item a mandatory `key` attribute:

{title="src/App.jsx",lang="javascript"}
~~~~~~~
const Search = ({ search, onSearch }) => [
  <label key="1" htmlFor="search">
    Search:{' '}
  </label>,
  <input
    key="2"
    id="search"
    type="text"
    value={search}
    onChange={onSearch}
  />,
];
~~~~~~~

Fortunately there exists another way of returning siblings elements side-by-side without a top-level element, because the last approach with the array doesn't turn out very readable and becomes verbose with the additional key attribute. Another solution is to use a **React fragment**:

{title="src/App.jsx",lang="javascript"}
~~~~~~~
const Search = ({ search, onSearch }) => (
# leanpub-start-insert
  <React.Fragment>
# leanpub-end-insert
    <label htmlFor="search">Search: </label>
    <input
      id="search"
      type="text"
      value={search}
      onChange={onSearch}
    />
# leanpub-start-insert
  </React.Fragment>
# leanpub-end-insert
);
~~~~~~~

A fragment wraps sibling elements into a single top-level element without adding them to the rendered output. See for yourself by inspecting the elements in your browser's development tools after using a fragment in your React component. A more popular alternative these days is using fragments in their shorthand version:

{title="src/App.jsx",lang="javascript"}
~~~~~~~
const Search = ({ search, onSearch }) => (
# leanpub-start-insert
  <>
# leanpub-end-insert
    <label htmlFor="search">Search: </label>
    <input
      id="search"
      type="text"
      value={search}
      onChange={onSearch}
    />
# leanpub-start-insert
  </>
# leanpub-end-insert
);
~~~~~~~

Both elements in the Search component - input field and label - should be still visible in your browser now. After all, whenever you don't want to introduce an intermediary element that's only there to satisfy React, you can use fragments as helper "elements".

### Exercises:

* Compare your source code against the author's [source code](https://bit.ly/3faQJVY).
  * Recap all the [source code changes from this section](https://bit.ly/3BCwyb4).
  * Optional: If you are using TypeScript, check out the author's source code [here](https://bit.ly/3SfRv2B).
* Optional: [Leave feedback for this section](https://forms.gle/kNpEySPZzckNe6f96).