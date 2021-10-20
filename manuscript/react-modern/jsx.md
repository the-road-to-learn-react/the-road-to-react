## React JSX

Everything returned from a React component will be displayed in the browser. Until now, we only returned HTML from the App component. However, recall that I mentioned the returned output of the App component not only resembles HTML, but it can also be mixed with JavaScript. In fact, this output is called JSX (JavaScript XML), powerfully combines HTML and JavaScript. Let's see how this works for displaying the variable from the previous section:

{title="src/App.js",lang="javascript"}
~~~~~~~
import * as React from 'react';

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

Either start your application again with `npm start` (or check whether your application still runs) and look for the rendered variable in the browser, which should read: "Hello React". If you change the variable in the source code, the browser should reflect that change.

Now let's focus on the HTML which differs slightly in JSX. An HTML input field with a label can be defined as follows:

{title="src/App.js",lang="javascript"}
~~~~~~~
import * as React from 'react';

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

For our input field and label combination, we specified three HTML attributes: `htmlFor`, `id`, and `type`. Where `id` and `type` should be familiar from native HTML, `htmlFor` might be new to you. The `htmlFor` reflects the `for` attribute in HTML. JSX replaces a handful of internal HTML attributes caused by internal implementation details of React itself. However, you can find all the [supported HTML attributes](https://bit.ly/2Z42zcK) in React's documentation, which follows the [camel case](https://bit.ly/3jljQFn) naming convention. Expect to come across more JSX-specific attributes like `className` and `onClick` instead of `class` and `onclick`, as you learn more about React.

We will revisit the HTML input field for implementation details later; for now, let's return to JavaScript in JSX in contrast to HTML. We have defined a JavaScript string primitive to be displayed in the App component, and the same can be done with a JavaScript object:

{title="src/App.js",lang="javascript"}
~~~~~~~
import * as React from 'react';

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

Essentially everything in curly braces in JSX can be used for JavaScript. For example, executing a function works this way too:

{title="src/App.js",lang="javascript"}
~~~~~~~
import * as React from 'react';

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

JSX was initially invented for React, but it became useful for other modern libraries and frameworks after it gained popularity. [It's one of my favorite things about React](https://bit.ly/3aZbdM0). Without any extra templating syntax (except for the curly braces), we are now able to use JavaScript in HTML. Every data structure, from JavaScript primitive to a JavaScript function, can be displayed within HTML with the help of JSX.

### Exercises:

* Confirm your [source code](https://bit.ly/3vvS8ec).
  * Confirm the [changes](https://bit.ly/3n3WW6o).
* Read more about [React's JSX](https://bit.ly/3BZSkVk).
* Read more about [JavaScript Variables](https://www.robinwieruch.de/javascript-variable).
  * Define more primitive and complex JavaScript data types and render them in JSX.
  * Try to render a JavaScript array in JSX ([hint](https://mzl.la/3B3a7tf)). If it's too complicated, don't worry, because you will learn more about this in the next section.
* Optional: [Rate this section](https://forms.gle/R6y6kEqGPACLrXmP8).