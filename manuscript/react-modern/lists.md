## Lists in React

When working with data in JavaScript, most often the data comes as an array of objects. Therefore, we will learn how to render a list of items in React next. In order to prepare you for rendering lists in React, let's recap one of the most common data manipulation methods: the [array's built-in map() method](https://mzl.la/3B3a7tf). It is used to iterate over each item of a list in order to return a new version of each item:

{title="Code Playground",lang="javascript"}
~~~~~~~
const numbers = [1, 2, 3, 4];

const exponentialNumbers = numbers.map(function (number) {
  return number * number;
});

console.log(exponentialNumbers);
// [1, 4, 9, 16]
~~~~~~~

In React, the array's built-in `map()` method is used to transform a list of items into JSX by returning JSX for each item. In the following, we want to display a list of items (here: JavaScript objects) in React. First, we will define the array outside of the component. Afterward, try yourself to render each object with its `title` property in React by inlining the array's `map()` method in JSX:

{title="src/App.jsx",lang="javascript"}
~~~~~~~
import * as React from 'react';

# leanpub-start-insert
const list = [
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

function App() { ... }

# leanpub-start-insert
// note the ... as placeholder
// for source code that didn't change
// and isn't relevant for this code snippet
# leanpub-end-insert

export default App;
~~~~~~~

Each item in the list has a `title`, an `url`, an `author`, an identifier (`objectID`), `points` -- which indicate the popularity of an item -- and a count of comments (`num_comments`). The property names are chosen this way, because they resemble real world data that we are going to use later. They don't fit the desired [naming conventions](https://www.robinwieruch.de/javascript-naming-conventions/) for JavaScript though.

Next, we'll render the list inlined in JSX with the array's built-in `map()` method. Hence we won't map from one JavaScript data type to another, but instead return JSX that renders each item of the list:

{title="src/App.jsx",lang="javascript"}
~~~~~~~
function App() {
  return (
    <div>
# leanpub-start-insert
      <h1>My Hacker Stories</h1>
# leanpub-end-insert

      <label htmlFor="search">Search: </label>
      <input id="search" type="text" />

# leanpub-start-insert
      <hr />
# leanpub-end-insert

# leanpub-start-insert
      <ul>
        {list.map(function (item) {
          return <li>{item.title}</li>;
        })}
      </ul>
# leanpub-end-insert
    </div>
  );
}
~~~~~~~

Actually, rendering a list of items in React was one of my personal JSX "Aha" moments. Without any made up templating syntax, it's possible to use JavaScript to map from a list of items to a list of HTML elements. That's what JSX is for the developer in the end: just JS mixed with HTML.

![](images/jsx-mapping.png)

Finally React displays each item now. But there is one important piece missing. If you check your browser's developer tools, you should see a warning showing up in the "Console"-tab which says that every React element in a list should have a key assigned to it. The `key` is an HTML attribute and should be a stable identifier. Fortunately, our items come with such a stable identifier, because they have an `id` (here: `objectId`):

{title="src/App.jsx",lang="javascript"}
~~~~~~~
function App() {
  return (
    <div>
      ...

      <ul>
        {list.map(function (item) {
# leanpub-start-insert
          return <li key={item.objectID}>{item.title}</li>;
# leanpub-end-insert
        })}
      </ul>
    </div>
  );
}
~~~~~~~

The `key` attribute is used for one specific reason: Whenever React has to re-render a list, it checks whether an item has changed. When using keys, React can efficiently exchange the changed items. When not using keys, React may update the list inefficiently. Take the following example where a new item gets appended at the start of the list.

![](images/react-keys-performance.png)

The key is not difficult to find, because usually when having data in shape of an array, we can use each item's stable identifier (e.g. `id` property). However, sometimes you do not have an `id`, so you need to come up with another identifier (e.g. `title` if it does not change and if it's unique in the array). As last resort, you can use the index of the item in the list too:

{title="Code Playground",lang="javascript"}
~~~~~~~
<ul>
  {list.map(function (item, index) {
    return (
      <li key={index}>
        {/* only use an index as last resort */}
        {/* and by the way: that's how you do comments in JSX */}

        {item.title}
      </li>
    );
  })}
</ul>
~~~~~~~

Usually using an index should be avoided though, because it comes with the same rendering performance issues from above. In addition, it can [cause actual bugs in the UI](https://www.robinwieruch.de/react-list-key/) whenever the order of items got changed (e.g. re-ordering, appending or removing items). However, as last resort, if the list does not change its order in any way, using the index is fine.

So far, we are only displaying the `title` of each item. Go ahead and render the item's `url`, `author`, `num_comments`, and `points` as well. In the special case of the `url`, use an HTML anchor HTML element (read: `<a>` tag) that surrounds the `title`. For guidance, the following solution will show you how the book implements this to be prepared for the next sections:

{title="src/App.jsx",lang="javascript"}
~~~~~~~
function App() {
  return (
    <div>
      ...

# leanpub-start-insert
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
# leanpub-end-insert
    </div>
  );
}
~~~~~~~

The array's `map()` method is inlined concisely in your JSX for rendering a list. Within the `map()` method, we have access to each object and its properties. The `url` property of each item is used as `href` attribute for the anchor HTML element. Not only can JavaScript in JSX be used to display elements, but also to assign HTML attributes dynamically. This section only scratches the surface of how powerful it is to mix JavaScript and HTML, however, using an array's `map()` method and assigning HTML attributes should give you a good first impression.

### Exercises:

* Compare your source code against the author's [source code](https://bit.ly/3ByuIYR).
  * Recap all the [source code changes from this section](https://bit.ly/3BVF1Yu).
  * Optional: If you are using TypeScript, check out the author's source code [here](https://bit.ly/3SEnv0u).
* Recap the [standard built-in array methods](https://mzl.la/3b9V9rf), especially *map*, *filter*, and *reduce*, which are available in JavaScript.
* Question: What happens if you return `null` instead of the JSX?
  * Answer: Returning `null` in JSX is allowed. It's always used if you want to render nothing.
* Extend the list with some more items to make the example more realistic.
* Practice using different JavaScript expressions in JSX.
* Optional: [Leave feedback for this section](https://forms.gle/aZmLFjEdSMTk9Thk9).