## Lists in React

So far we've rendered a few primitive variables in JSX; next we'll render a list of items. We'll experiment with sample data at first, then we'll apply that to fetch data from a remote API. First, let's define the array as a variable. As before, we can define a variable outside or inside the component. The following defines it outside:

{title="src/App.js",lang="javascript"}
~~~~~~~
import React from 'react';

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

export default App;
~~~~~~~

I used a `...` here as a placeholder, to keep my code snippet small (without App component implementation details) and focused on the essential parts (the `list` variable outside of the App component). I will use the `...` throughout the rest of this learning experience as placeholder for code blocks that I have established previous exercises. If you get lost, you can always verify your code using the CodeSandbox links I provide at the end of most sections.

Each item in the list has a title, a url, an author, an identifier (`objectID`), points -- which indicate the popularity of an item -- and a count of comments. Next, we'll render the list within our JSX dynamically:

{title="src/App.js",lang="javascript"}
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
      {/* render the list here */}
# leanpub-end-insert
    </div>
  );
}
~~~~~~~

You can use the [built-in JavaScript map method for arrays](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/map) to iterate over each item of the list and return a new version of each:

{title="Code Playground",lang="javascript"}
~~~~~~~
const numbers = [1, 4, 9, 16];

const newNumbers = numbers.map(function(number) {
  return number * 2;
});

console.log(newNumbers);
// [2, 8, 18, 32]
~~~~~~~

We won't map from one JavaScript data type to another in our case. Instead, we return a JSX fragment that renders each item of the list:

{title="src/App.js",lang="javascript"}
~~~~~~~
function App() {
  return (
    <div>
      ...

      <hr />

# leanpub-start-insert
      {list.map(function(item) {
        return <div>{item.title}</div>;
      })}
# leanpub-end-insert
    </div>
  );
}
~~~~~~~

Actually, one of my first React "Aha" moments was using barebones JavaScript to map a list of JavaScript objects to HTML elements without any other HTML templating syntax. It's just JavaScript in HTML.

React will display each item now, but you can still improve your code so React handles advanced dynamic lists more gracefully. By assigning a key attribute to each list item's element, React can identify modified items if the list changes (e.g. re-ordering). Fortunately, our list items come with an identifier:

{title="src/App.js",lang="javascript"}
~~~~~~~
function App() {
  return (
    <div>
      ...

      <hr />

      {list.map(function(item) {
# leanpub-start-insert
        return (
          <div key={item.objectID}>
# leanpub-end-insert
            {item.title}
# leanpub-start-insert
          </div>
        );
# leanpub-end-insert
      })}
    </div>
  );
}
~~~~~~~

We avoid using the index of the item in the array to make sure the key attribute is a stable identifier. If the list changes its order, for example, React will not be able to identify the items properly:

{title="Code Playground",lang="javascript"}
~~~~~~~
// don't do this
{list.map(function(item, index) {
  return (
    <div key={index}>
      ...
    </div>
  );
})}
~~~~~~~

So far, only the title is displayed for each item. Let's experiment with displaying more of the item's properties:

{title="src/App.js",lang="javascript"}
~~~~~~~
function App() {
  return (
    <div>
      ...

      <hr />

# leanpub-start-insert
      {list.map(function(item) {
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
      })}
# leanpub-end-insert
    </div>
  );
}
~~~~~~~

The map function is inlined concisely in your JSX. Within the map function, we have access to each item and its properties. The `url` property of each item is used as dynamic `href` attribute for the anchor tag. Not only can JavaScript in JSX be used to display items, but also to assign HTML attributes dynamically.

### Exercises:

* Confirm your [source code for the last section](https://codesandbox.io/s/github/the-road-to-learn-react/hacker-stories/tree/hs/Lists-in-React).
  * Confirm the [changes from the last section](https://github.com/the-road-to-learn-react/hacker-stories/compare/hs/React-JSX...hs/Lists-in-React?expand=1).
* Read more about why React's key attribute is needed ([0](https://dev.to/jtonzing/the-significance-of-react-keys---a-visual-explanation--56l7), [1](https://www.robinwieruch.de/react-list-key), [2](https://reactjs.org/docs/lists-and-keys.html)). Don't worry if you don't understand the implementation yet, just focus on what problem it causes for dynamic lists.
* Recap the [standard built-in array methods](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/) -- especially *map*, *filter*, and *reduce* -- which are available in native JavaScript.
* What happens if your return `null` instead of the JSX?
* Extend the list with some more items to make the example more realistic.
* Practice using different JavaScript expressions in JSX.