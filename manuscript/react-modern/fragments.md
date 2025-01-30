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

Fortunately there exists another way of returning sibling elements side-by-side without a top-level element, because the last approach with the array doesn't turn out very readable and becomes verbose with the additional key attribute. Another solution is to use a **React fragment**:

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

* Compare your source code against the author's [source code](https://tinyurl.com/mrf4hc3h).
  * Recap all the [source code changes](https://tinyurl.com/3cmv4ump) from this section.
  * Optional: If you are using TypeScript, check out the author's source code [here](https://bit.ly/3HLzrtH).

### Interview Questions:

* Question: What is a Fragment in React?
  * Answer: A Fragment is a way to group multiple React elements without introducing an additional DOM element.
* Question: How do you use Fragments in JSX?
  * Answer: Wrap the elements with `<React.Fragment>` or its shorthand syntax `<>...</>`.
* Question: Why use Fragments in React?
  * Answer: Fragments allow grouping elements without adding extra nodes to the DOM, useful when a parent wrapper is not desired.
* Question: Can Fragments have keys?
  * Answer: Yes, Fragments can have keys when mapping over a list of elements.
* Question: Are Fragments required in every React component?
  * Answer: No, Fragments are optional and are typically used when a component needs to return multiple elements without a parent wrapper.
* Question: Do Fragments impact the rendered HTML structure?
  * Answer: No, Fragments do not introduce any additional nodes to the HTML structure.
* Question: Can Fragments have attributes like class or style?
  * Answer: No, Fragments themselves cannot have attributes. Attributes should be applied to the elements within the Fragment.
* Question: How does using Fragments differ from using div containers?
  * Answer: Fragments don't create an extra DOM node, providing a cleaner HTML structure compared to using div containers.
