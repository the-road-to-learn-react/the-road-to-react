## React Fragments

One caveat with JSX, especially when we create a dedicated Search component, is that we must introduce a wrapping HTML element to render it:

{title="src/App.js",lang="javascript"}
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

Normally the JSX returned by a React component needs only one wrapping top-level element. To render multiple top-level elements side-by-side, we have to wrap them into an array instead. Since we're working with a list of elements, we have to give every sibling element React's `key` attribute:

{title="src/App.js",lang="javascript"}
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

This is one way to have multiple top-level elements in your JSX. It doesn't turn out very readable, though, as it becomes verbose with the additional key attribute. Another solution is to use a **React fragment**:

{title="src/App.js",lang="javascript"}
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

A fragment wraps other elements into a single top-level element without adding to the rendered output. Both Search elements should be visible in your browser now, with input field and label. So if you prefer to omit the wrapping `<div>` or `<span>` elements, substitute them with an empty tag that is allowed in JSX, and doesn't introduce intermediate elements in our rendered HTML.

### Exercises:

* Confirm your [source code for the last section](https://codesandbox.io/s/github/the-road-to-learn-react/hacker-stories/tree/hs/React-Fragments).
 * Confirm the [changes from the last section](https://github.com/the-road-to-learn-react/hacker-stories/compare/hs/React-Custom-Hooks...hs/React-Fragments?expand=1).
* Read more about [React fragments](https://reactjs.org/docs/fragments.html).