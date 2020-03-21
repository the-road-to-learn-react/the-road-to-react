## React JSX

Recall that I mentioned the returned output of the App component resembles HTML. This output is called JSX, which mixes HTML and JavaScript. Let's see how this works for displaying the variable:

{title="src/App.js",lang="javascript"}
~~~~~~~
import React from 'react';

const title = 'React';

function App() {
  return (
    <div>
# leanpub-start-insert
      <h1>Hello {title}</h1>
# leanpub-end-insert
    </div>
  );
}

export default App;
~~~~~~~

Start your application with `npm start` again, and look for the rendered variable in browser, which should read: "Hello React".

Let's focus on the HTML, which is expressed almost the same in JSX. An input field with a label can be defined as follows:

{title="src/App.js",lang="javascript"}
~~~~~~~
import React from 'react';

const title = 'React';

function App() {
  return (
    <div>
      <h1>Hello {title}</h1>

# leanpub-start-insert
      <label htmlFor="search">Search: </label>
      <input id="search" type="text" />
# leanpub-end-insert
    </div>
  );
}

export default App;
~~~~~~~

We specified three HTML attributes here: `htmlFor`, `id`, and `type`. Where `id` and `type` should be familiar from native HTML, `htmlFor` might be new. The `htmlFor` reflects the `for` attribute in HTML. JSX replaces a handful of internal HTML attributes, but you can find all the [supported HTML attributes](https://reactjs.org/docs/dom-elements.html#all-supported-html-attributes) in React's documentation, which follow the camelCase naming convention. Expect to come across more JSX-specific attributes like `className` and `onClick` instead of `class` and `onclick`, as you learn more about React.

We will revisit the HTML input field for implementation details later; for now, let's return to JavaScript in JSX. We have defined a string primitive to be displayed in the App component, and the same can be done with a JavaScript object:

{title="src/App.js",lang="javascript"}
~~~~~~~
import React from 'react';

# leanpub-start-insert
const welcome = {
  greeting: 'Hey',
  title: 'React',
};
# leanpub-end-insert

function App() {
  return (
    <div>
      <h1>
# leanpub-start-insert
        {welcome.greeting} {welcome.title}
# leanpub-end-insert
      </h1>

      <label htmlFor="search">Search: </label>
      <input id="search" type="text" />
    </div>
  );
}

export default App;
~~~~~~~

Remember, everything in curly braces in JSX can be used for JavaScript expressions (e.g. function execution):

{title="src/App.js",lang="javascript"}
~~~~~~~
import React from 'react';

# leanpub-start-insert
function getTitle(title) {
  return title;
}
# leanpub-end-insert

function App() {
  return (
    <div>
# leanpub-start-insert
      <h1>Hello {getTitle('React')}</h1>
# leanpub-end-insert

      <label htmlFor="search">Search: </label>
      <input id="search" type="text" />
    </div>
  );
}

export default App;
~~~~~~~

JSX was initially invented for React, but it became useful for other modern libraries and frameworks after it gained popularity. It is one of my favorite things about React. Without any extra templating syntax (except for the curly braces), we are now able to use JavaScript in HTML.

### Exercises:

* Confirm your [source code for the last section](https://codesandbox.io/s/github/the-road-to-learn-react/hacker-stories/tree/hs/React-JSX).
  * Confirm the [changes from the last section](https://github.com/the-road-to-learn-react/hacker-stories/compare/hs/Meet-the-React-Component...hs/React-JSX?expand=1).
* Read more about [React's JSX](https://reactjs.org/docs/introducing-jsx.html).
* Define more primitive and complex JavaScript data types and render them in JSX.
* Try to render a JavaScript array in JSX. If it's too complicated, don't worry, because you will learn more about this in the next section.