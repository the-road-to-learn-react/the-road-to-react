## React Component Definition (Advanced)

The following refactorings are optional recommendations to explain the different JavaScript/React patterns. You can build complete React applications without these advanced patterns, so don't be discouraged if they seem too complicated.

All components in the *src/App.js* file are function component. JavaScript has multiple ways to declare functions. So far, we have used the function statement, though arrow functions can be used more concisely:

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

// allowed
const (item) => { ... }

// not allowed
const item, index => { ... }

// allowed
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
const List = () => {
# leanpub-end-insert
  return list.map(function(item) {
    return (
      <div key={item.objectID}>
        ...
      </div>
    );
  });
# leanpub-start-insert
};
# leanpub-end-insert
~~~~~~~

This holds also true for other functions, like the one we used in our JavaScript array's map method:

{title="src/App.js",lang="javascript"}
~~~~~~~
const List = () => {
# leanpub-start-insert
  return list.map(item => {
# leanpub-end-insert
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
  });
};
~~~~~~~

If an arrow function doesn't do *anything* in between, but only returns *something*, -- in other words, if an arrow function doesn't perform any task, but only returns information --, you can remove the **block body** (curly braces) of the function. In a **concise body**, an **implicit return statement** is attached, so you can remove the return statement:

{title="Code Playground",lang="javascript"}
~~~~~~~
// with block body
count => {
  // perform any task in between

  return count + 1;
}

// with concise body
count =>
  count + 1;
~~~~~~~

This can be done for the App and List component as well, because they only return JSX and don't perform any task in between. Again it also applies for the arrow function that's used in the map function:

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
const List = () =>
  list.map(item => (
# leanpub-end-insert
    <div key={item.objectID}>
      <span>
        <a href={item.url}>{item.title}</a>
      </span>
      <span>{item.author}</span>
      <span>{item.num_comments}</span>
      <span>{item.points}</span>
    </div>
# leanpub-start-insert
  ));
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

* Confirm your [source code for the last section](https://codesandbox.io/s/github/the-road-to-learn-react/hacker-stories/tree/hs/React-Component-Definition).
  * Confirm the [changes from the last section](https://github.com/the-road-to-learn-react/hacker-stories/compare/hs/Meet-another-React-Component...hs/React-Component-Definition?expand=1).
* Read more about [JavaScript arrow functions](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/Arrow_functions).
* Familiarize yourself with arrow functions with block body and return, and concise body without return.