## Meet the React Component

Every React application is build on the foundation of React components. In this section, you will get to know your first React component which is located in the _src/App.js_ file and which should look similar to the example below. Depending on your create-react-app version, the content of the file might differ slightly:

{title="src/App.js",lang="javascript"}
~~~~~~~
import * as React from 'react';
import logo from './logo.svg';
import './App.css';

function App() {
  return (
    <div className="App">
      <header className="App-header">
        <img src={logo} className="App-logo" alt="logo" />
        <p>
          Edit <code>src/App.js</code> and save to reload.
        </p>
        <a
          className="App-link"
          href="https://reactjs.org"
          target="_blank"
          rel="noopener noreferrer"
        >
          Learn React
        </a>
      </header>
    </div>
  );
}

export default App;
~~~~~~~

This file will be our focus throughout this tutorial, unless otherwise specified. Let's start by reducing the component to a more lightweight version for getting you started without too much [boilerplate code](https://en.wikipedia.org/wiki/Boilerplate_code). Afterward, start your application with `npm start` in the command line and check what's displayed in the browser:

{title="src/App.js",lang="javascript"}
~~~~~~~
# leanpub-start-insert
import * as React from 'react';

function App() {
  return (
    <div>
      <h1>Hello World</h1>
    </div>
  );
}

export default App;
# leanpub-end-insert
~~~~~~~

Before we dive deeper into each topic, here comes a quick overview of what you are seeing:

* First, this React component, called App component, is just a JavaScript function. In contrast to JavaScript functions, it's defined in [PascalCase](https://www.robinwieruch.de/javascript-naming-conventions). This kind of component is commonly called **function component**. Function components are the modern way of using components in React, however, be aware that there are other variations of React components too. (see **component types** later)
* Second, the App component doesn't receive any parameters in its function signature yet. In the upcoming sections, you will learn how to pass information (props) from one component to another component. These props will be accessible via the function's signature as parameters then. (see **props** later)
* And third, the App component returns code that resembles HTML. You will see how this new syntax, called JSX, allows you to combine JavaScript and HTML for displaying highly dynamic and interactive content in a browser. (see **JSX** later)

The function component possesses implementation details between function signature and return statement, like any other JavaScript function. You will see this in practice in action throughout your React journey:

{title="src/App.js",lang="javascript"}
~~~~~~~
import * as React from 'react';

function App() {
# leanpub-start-insert
  // you can do something in between
# leanpub-end-insert

  return (
    <div>
      <h1>Hello World</h1>
    </div>
  );
}

export default App;
~~~~~~~

Variables defined in the function's body will be re-defined each time this function runs, which shouldn't be something new if you are familiar with JavaScript and its functions:

{title="src/App.js",lang="javascript"}
~~~~~~~
import * as React from 'react';

function App() {
# leanpub-start-insert
  const title = 'React';
# leanpub-end-insert

  return (
    <div>
      <h1>Hello World</h1>
    </div>
  );
}

export default App;
~~~~~~~

The function of a function component will either run when a component is rendered for the first time or re-rendered when it updates. The act of rendering means that the component displays itself in the browser. Since we don't need anything from within the App component used to define this variable from the last code snippet -- for example parameters coming from the component's function signature -- we can define the variable outside of the App component as well. Thus it will be defined only once and not every time the function is called:

{title="src/App.js",lang="javascript"}
~~~~~~~
import * as React from 'react';

# leanpub-start-insert
const title = 'React';
# leanpub-end-insert

function App() {
  return (
    <div>
      <h1>Hello World</h1>
    </div>
  );
}

export default App;
~~~~~~~

On your journey as a React developer, and in this learning experience, you will come across both scenarios: variables defined outside and within a component. As a rule of thumb: If a variable does not need anything from within the function component's body (e.g. parameters), then define it outside of the component which avoids redefining it on every function call.

### Exercises:

* Confirm your [source code](https://codesandbox.io/s/github/the-road-to-learn-react/hacker-stories/tree/2021/Meet-the-React-Component).
* If you are unsure when to use `const`, `let` or `var` in JavaScript (or React) for variable declarations, make sure to [read more about their differences](https://www.robinwieruch.de/const-let-var).
  * Read more about [const](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/const).
  * Read more about [let](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/let).
* Think about ways to display the `title` variable in your App component's returned HTML. In the next section, we'll put this variable to use.