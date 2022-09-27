## React Component Declaration

We have declared multiple React components so far. Since these components are function components, we can leverage the different ways of declaring functions in JavaScript. So far, we have used the standard function declaration, though arrow functions can be used more concisely and therefore can establish a new standard for declaring function components:

{title="Code Playground",lang="javascript"}
~~~~~~~
// function declaration
function App() { ... }

// arrow function expression
const App = () => { ... }
~~~~~~~

Equipped with this knowledge, go through your React project and refactor all function declarations to arrow function expressions. While this refactoring can be applied to function components, it can also be used for any other functions that are used in the project. In the book, we will go ahead as well and refactor all the function component's function declarations to arrow function expressions:

{title="src/App.jsx",lang="javascript"}
~~~~~~~
# leanpub-start-insert
const App = () => {
# leanpub-end-insert
  return ( ... );
# leanpub-start-insert
};
# leanpub-end-insert

# leanpub-start-insert
const Search = () => {
# leanpub-end-insert
  return ( ... );
# leanpub-start-insert
};
# leanpub-end-insert

# leanpub-start-insert
const List = () => {
# leanpub-end-insert
  return ( ... );
# leanpub-start-insert
};
# leanpub-end-insert
~~~~~~~

As said, not only function components can be refactored, but also other functions like the [callback function](https://www.robinwieruch.de/javascript-callback-function/) that we have used for the array's `map()` method:

{title="src/App.jsx",lang="javascript"}
~~~~~~~
const List = () => {
  return (
    <ul>
# leanpub-start-insert
      {list.map((item) => {
# leanpub-end-insert
        return (
          <li key={item.objectID}>
            ...
          </li>
        );
      })}
    </ul>
  );
};
~~~~~~~

If an arrow function's only purpose is to return a value and it doesn't have any business logic in between, you can remove the **block body** (curly braces) of the function. In a **concise body**, an **implicit return statement** is attached, so you can remove the return statement:

{title="Code Playground",lang="javascript"}
~~~~~~~
// with block body
const addOne = (count) => {
  // perform any task in between

  return count + 1;
};

// with concise body as multi line
const addOne = (count) =>
  count + 1;

// with concise body as one line
const addOne = (count) => count + 1;
~~~~~~~

This can be done for the App, List, and Search components as well, because they only return JSX and don't perform any task in between. In addition, it also applies to the arrow function that's used in the `map()` method:

{title="src/App.jsx",lang="javascript"}
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
    ...
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
        ...
      </li>
# leanpub-start-insert
    ))}
# leanpub-end-insert
  </ul>
# leanpub-start-insert
);
# leanpub-end-insert
~~~~~~~

All JSX is more concise now, because it omits the function statement, the curly braces, and the return statement. However, it's important to remember this is an optional step and that it's acceptable to use function declarations over arrow function expressions and block bodies with curly braces over concise bodies with implicit returns for arrow functions.

Often block bodies will be necessary to introduce more business logic between function signature and return statement. Be sure to understand this refactoring concept, because we'll move quickly from arrow function components with and without block bodies as we go. Which one we use will depend on the requirements of the component:

{title="Code Playground",lang="javascript"}
~~~~~~~
const App = () => {
  // perform a task in between

  return (
    <div>
      ...
    </div>
  );
};
~~~~~~~

As a rule of thumb, use either function declarations or arrow function expressions for your component declarations throughout your application. Both versions are fine to use, but make sure that you and your team working on the project share the same implementation style. In addition, while an implicit return statement when using an arrow function expressions makes your component declaration more concise, you may introduce tedious refactorings from concise to block body when you need to perform tasks between function signature and the return statement. So you may want to keep your arrow function expression with a block body (like in the last code snippet) all the time.

### Exercises:

* Compare your source code against the author's [source code](https://bit.ly/3QVhIlK).
  * Recap all the [source code changes from this section](https://bit.ly/3eX5hZi).
  * Optional: If you are using TypeScript, check out the author's source code [here](https://bit.ly/3STeJfd).
* Optional: Read more about [JavaScript arrow functions](https://mzl.la/3BYCOcp).
* Familiarize yourself with arrow functions with block body and explicit return and concise body without return (implicit return).
* Optional: [Leave feedback for this section](https://forms.gle/iWSchmqasbZUWUpT8).