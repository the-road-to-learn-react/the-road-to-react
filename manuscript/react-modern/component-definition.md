## React Component Definition (Advanced)

The following refactorings are optional recommendations to explain the different JavaScript/React patterns. You can build complete React applications without these advanced patterns, so don't be discouraged if they seem too complicated.

JavaScript has multiple ways to declare functions. So far, we have used the function statement, though arrow functions can be used more concisely:

{title="Code Playground",lang="javascript"}
~~~~~~~
// function declaration
function () { ... }

// arrow function declaration
const () => { ... }
~~~~~~~

You can remove the parentheses in an arrow function expression if it has only one argument, but multiple arguments require parentheses:

{title="Code Playground",lang="javascript"}
~~~~~~~
// allowed
const item => { ... }

// allowed (recommended)
const (item) => { ... }

// not allowed
const item, index => { ... }

// allowed (recommended)
const (item, index) => { ... }
~~~~~~~

Defining React function components with arrow functions makes them more concise:

{title="src/App.js",lang="javascript"}
~~~~~~~
# leanpub-start-insert
const App = () => {
# leanpub-end-insert
  return (
    <div>
      ...
    </div>
  );
# leanpub-start-insert
};
# leanpub-end-insert

# leanpub-start-insert
const Search = () => {
# leanpub-end-insert
  return (
    <div>
      <label htmlFor="search">Search: </label>
      <input id="search" type="text" />
    </div>
  );
# leanpub-start-insert
};
# leanpub-end-insert

# leanpub-start-insert
const List = () => {
# leanpub-end-insert
  return (
    <ul>
      ...
    </ul>
  );
# leanpub-start-insert
};
# leanpub-end-insert
~~~~~~~

This holds also true for other functions, like the one we used in our JavaScript array's map method:

{title="src/App.js",lang="javascript"}
~~~~~~~
const List = () => {
  return (
    <ul>
# leanpub-start-insert
      {list.map((item) => {
# leanpub-end-insert
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
  );
};
~~~~~~~

If an arrow function doesn't do *anything* in between, but only returns *something*, -- in other words, if an arrow function doesn't have business logic in between, but only returns --, you can remove the **block body** (curly braces) of the function. In a **concise body**, an **implicit return statement** is attached, so you can remove the return statement:

{title="Code Playground",lang="javascript"}
~~~~~~~
// with block body
const countPlusOne = (count) => {
  // perform any task in between

  return count + 1;
};

// with concise body
const countPlusOne = (count) =>
  count + 1;

// with concise body as one line
const countPlusOne = (count) => count + 1;
~~~~~~~

This can be done for the App, List and Search components as well, because they only return JSX and don't perform any task in between. Again it also applies for the arrow function that's used in the map function:

{title="src/App.js",lang="javascript"}
~~~~~~~
# leanpub-start-insert
const App = () => (
# leanpub-end-insert
  <div>
    ...
  </div>
# leanpub-start-insert
);
# leanpub-end-insert

# leanpub-start-insert
const Search = () => (
# leanpub-end-insert
  <div>
    <label htmlFor="search">Search: </label>
    <input id="search" type="text" />
  </div>
# leanpub-start-insert
);
# leanpub-end-insert

# leanpub-start-insert
const List = () => (
# leanpub-end-insert
  <ul>
# leanpub-start-insert
    {list.map((item) => (
# leanpub-end-insert
      <li key={item.objectID}>
        <span>
          <a href={item.url}>{item.title}</a>
        </span>
        <span>{item.author}</span>
        <span>{item.num_comments}</span>
        <span>{item.points}</span>
      </li>
# leanpub-start-insert
    ))}
# leanpub-end-insert
  </ul>
# leanpub-start-insert
);
# leanpub-end-insert
~~~~~~~

Our JSX is more concise now, as it omits the function statement, the curly braces, and the return statement. However, remember this is an optional step, and that it's acceptable to use normal functions instead of arrow functions and block bodies with curly braces for arrow functions over implicit returns. Sometimes block bodies will be necessary to introduce more business logic between function signature and return statement:

{title="Code Playground",lang="javascript"}
~~~~~~~
const App = () => {
  // perform any task in between

  return (
    <div>
      ...
    </div>
  );
};
~~~~~~~

Be sure to understand this refactoring concept, because we'll move quickly from arrow function components with and without block bodies as we go. Which one we use will depend on the requirements of the component.

### Exercises:

* Confirm your [source code](https://codesandbox.io/s/github/the-road-to-learn-react/hacker-stories/tree/2021/React-Component-Definition).
  * Confirm the [changes](https://github.com/the-road-to-learn-react/hacker-stories/compare/2021/Meet-another-React-Component...2021/React-Component-Definition).
* Read more about [JavaScript arrow functions](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/Arrow_functions).
* Familiarize yourself with arrow functions with block body and return, and concise body without return.