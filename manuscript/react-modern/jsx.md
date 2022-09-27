## React JSX

Everything returned from a React component will be displayed in the browser. Until now, we only returned HTML from the App component. However, recall that I mentioned the returned output of the App component not only resembles HTML, but it can also be mixed with JavaScript. In fact, this output is called **JSX (JavaScript XML)**, which powerfully combines HTML and JavaScript. Let's see how this works for displaying the variable from the previous section:

{title="src/App.jsx",lang="javascript"}
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

Either start your application again with `npm run dev` (or check whether your application still runs) and look for the rendered (read: displayed) `title` in the browser. The output should read "Hello React". If you change the variable in the source code, the browser should reflect that change.

Changing the variable in the source code and seeing the change reflected in the browser is not solely connected to React, but also to the underlying development server when we start our application on the command line. Any time one of our files changes, the development server notices it and reloads all affected files for the browser. The bridge between React and the development server which makes this behavior possible is called **React Fast Refresh** (prior to that it was **React Hot Loader**) on React's side and **Hot Module Replacement** on the development server's side.

Next, try to define a HTML input field (read: `<input>` tag) and a HTML label (read: `<label>` tag) in your JSX yourself. It should also be possible to focus the input field when clicking the label either by nesting the input field in the label or by using dedicated HTML attributes for both. The following code snippet will show you the book's implementation of this task and you may be surprised that HTML slightly differs when used in JSX:

{title="src/App.jsx",lang="javascript"}
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

For our input field and label combination, we specified three HTML attributes: `htmlFor`, `id`, and `type`. The `type` attribute is kinda mandatory and has nothing to do with focusing the input field when clicking the label. However, while `id` and `type` should be familiar from native HTML, `htmlFor` might be new to you.

The `htmlFor` reflects the `for` attribute in vanilla HTML. You may be wondering why this attribute differs from native HTML. JSX replaces a handful of internal HTML attributes caused by internal implementation details of React itself. However, you can find all the [supported HTML attributes](https://bit.ly/2Z42zcK) in React's documentation. Since JSX is closer to JavaScript than to HTML, React uses the [camelCase](https://bit.ly/3jljQFn) naming convention. Expect to come across more JSX-specific attributes like `className` and `onClick` instead of `class` and `onclick`, as you learn more about React.

When using HTML in JSX, React internally translates all HTML attributes to JavaScript where certain words such as `class` or `for` are reserved during the rendering process. Therefore React came up with replacements such as `className` and `htmlFor` for them. However, once the actual HTML is rendered for the browser, the attributes get translated back to their native variant.

![](images/rendering-jsx.png)

We will revisit the HTML input field and its label for further implementation details with JavaScript later. For now, in order to contrast how HTML and JavaScript are used in JSX, let's use more complex JavaScript data types in JSX. Instead of defining a JavaScript string primitive like `title`, define a JavaScript object called `welcome` which has a `title` (e.g. `'React'`) and a `greeting` (e.g. `'Hey'`) as properties. Afterward, try yourself to render both properties of the object in JSX side by side in the `<h1>` tag.

The following code snippet will show you the solution to the task. Before we have defined a JavaScript string primitive to be displayed in the App component. Now, the same can be done with a JavaScript object by accessing its properties within JSX:

{title="src/App.jsx",lang="javascript"}
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

While HTML can be used almost (except for the attributes) in its native way in JSX, everything in curly braces can be used to interpolate JavaScript in it. For example, you could define a function that returns the title and execute it within the curly braces:

{title="src/App.jsx",lang="javascript"}
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

JSX is a syntax extension to JavaScript. In the past, JavaScript files which made use of JSX [had to use](https://github.com/airbnb/javascript/pull/985) the *.jsx* instead of the *.js* extension. However, these days the underlying build tools (read: compiler/bundler) [can be configured to acknowledge JSX in a .js file](https://www.robinwieruch.de/minimal-react-webpack-babel-setup/). If they are configured this way, they will transpile JSX to JavaScript. Tools like Vite embrace the *.jsx* extension though, because it makes it more explicit for developers.

{title="Code Playground",lang="javascript"}
~~~~~~~
const title = 'React';

// JSX ...
const myElement = <h1>Hello {title}</h1>;

// ... gets transpiled to JavaScript
const myElement = React.createElement('h1', null, `Hello ${title}`);

// ... gets rendered as HTML by React
<h1>Hello React</h1>
~~~~~~~

JSX enables developers to express what should be rendered by mixing up HTML with JavaScript. Whereas the previous way of thinking was to decouple markup (read: HTML) from logic (read: JavaScript), React puts all of it together as one unit in a React component. As you can see from the last code snippet, React does not require you to use JSX at all, instead it's possible to use methods like `createElement()`. However, most people find it more intuitive to use JSX for its declarative nature instead of using JavaScript methods (here: methods offered by React) which only allow one to express the UI imperatively.

Initially invented for React, JSX gained popularity in other modern libraries and frameworks as well. These days, it's not strictly coupled to React, but people are usually connecting it to React. Anyway, [JSX is one of my favorite things when being asked about React](https://bit.ly/3aZbdM0). Without any extra templating syntax (except for the curly braces), we are able to use JavaScript in HTML. Every JavaScript data structure, from primitive to complex, can be used within HTML with the help of JSX.

### Exercises:

* Compare your source code against the author's [source code](https://bit.ly/3S2uyQy).
  * Recap all the [source code changes from this section](https://bit.ly/3dAr0G3).
  * Optional: If you are using TypeScript, check out the author's source code [here](https://bit.ly/3LJx1Nu).
* Beginner: Read more about [JavaScript Variables](https://www.robinwieruch.de/javascript-variable/).
  * Beginner: Define more primitive and complex JavaScript data types and render them in JSX.
  * Advanced: Try to render a JavaScript array in JSX by using the array's built-in `map()` method to return JSX for each item in the list. If it's too complicated, don't worry, because you will learn more about this in the next section.
* Optional: Read more about [React's JSX](https://bit.ly/3BZSkVk).
* Optional: [Leave feedback for this section](https://forms.gle/R6y6kEqGPACLrXmP8).