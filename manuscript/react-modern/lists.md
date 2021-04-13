## Lists in React

So far we've rendered a few variables in JSX, however, most often you will deal with arrays. Thus we'll render a list of items next. We'll experiment with sample data first, and later we'll apply that knowledge to fetched data from a remote API.

First, let's define the array as a variable. As before, we can define a variable outside or inside the component. The following defines it outside:

{title="src/App.js",lang="javascript"}
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

export default App;
~~~~~~~

*Note: I used a `...` here as a placeholder, to keep my code snippet small (without the App component's implementation details) and focused on the essential parts (the `list` variable outside of the App component). I will use the `...` throughout the rest of this learning experience as a placeholder for code blocks that I have established in previous sections. If you get lost, you can always verify your code using the CodeSandbox links I provide at the end of most sections.*

Each item in the list has a title, a url, an author, an identifier (`objectID`), points -- which indicate the popularity of an item -- and a count of comments (`num_comments`). Next, we'll render the list within our JSX dynamically:

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
      {/* and by the way: that's how you do comments in JSX */}
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

In our case, we won't map from one JavaScript data type to another. Instead, we return a JSX fragment that renders each item of the list:

{title="src/App.js",lang="javascript"}
~~~~~~~
function App() {
  return (
    <div>
      ...

      <hr />

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

Actually, one of my first React "Aha" moments was using barebones JavaScript to map a list of JavaScript objects to HTML elements without any other HTML templating syntax. It's just JavaScript mixed with HTML.

![](images/jsx-mapping.png)

React will display each item now, but you can still improve your code so React handles dynamic lists more gracefully. By assigning a `key` attribute to each list item's element, React can identify items if the list changes (e.g. re-ordering). The `key` isn't necessary yet in our current situation, however, it's a best practice to use it from the start. Fortunately, our items come with an identifier:

{title="src/App.js",lang="javascript"}
~~~~~~~
function App() {
  return (
    <div>
      ...

      <hr />

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

We avoid using the `index` of the item in the array to make sure the key attribute is a stable identifier. If the list changes its order, for example, React will not be able to identify the items properly when using the array's `index`:

{title="Code Playground",lang="javascript"}
~~~~~~~
// avoid doing this
<ul>
  {list.map(function (item, index) {
    return (
      <li key={index}>
        ...
      </li>
    );
  })}
</ul>
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

The map function is inlined concisely in your JSX. Within the map function, we have access to each item and its properties. The `url` property of each item is used as dynamic `href` attribute for the HTML anchor tag. Not only can JavaScript in JSX be used to display items, but also to assign HTML attributes dynamically. This section only scratches the surface of how powerful it is to use mixed JavaScript and HTML, however, using an array's map function or assigning dynamic HTML attributes should give you a good first impression.

### Exercises:

* Confirm your [source code](https://codesandbox.io/s/github/the-road-to-learn-react/hacker-stories/tree/2021/Lists-in-React).
  * Confirm the [changes](https://github.com/the-road-to-learn-react/hacker-stories/compare/2021/React-JSX...2021/Lists-in-React).
* Read more about why React's key attribute is needed ([0](https://dev.to/jtonzing/the-significance-of-react-keys---a-visual-explanation--56l7), [1](https://www.robinwieruch.de/react-list-key), [2](https://reactjs.org/docs/lists-and-keys.html)). Don't worry if you don't understand the implementation yet, just focus on what problem it causes for dynamic lists.
* Recap the [standard built-in array methods](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/) -- especially *map*, *filter*, and *reduce* -- which are available in native JavaScript.
* What happens if you return `null` instead of the JSX? Try it first, then read the answer:
  * Returning `null` in JSX is allowed. It's always used if you want to render nothing.
* Extend the list with some more items to make the example more realistic.
* Practice using different JavaScript expressions in JSX.