## Meet the React Component

Our first React component is in the *src/App.js* file, which should look similar to the example below. The file might differ slightly, because create-react-app will sometimes update the default component's structure.

{title="src/App.js",lang="javascript"}
~~~~~~~
import React from 'react';
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
          Href="https://reactjs.org"
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

This file will be our focus throughout this tutorial, unless otherwise specified. Let's start by reducing the component to a more lightweight version for getting you started without too much boilerplate code from create-react-app.

{title="src/App.js",lang="javascript"}
~~~~~~~
# leanpub-start-insert
import React from 'react';

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

First, this React component, called App component, is just a JavaScript function. It's commonly called **function component**, because there are other variations of React components  (see **component types** later). Second, the App component doesn't receive any parameters in its function signature yet (see **props** later). And third, the App component returns code that resembles HTML which is called JSX (see **JSX** later).

The function component possess implementation details like any other JavaScript function. You will see this in practice in action throughout your React journey:

{title="src/App.js",lang="javascript"}
~~~~~~~
import React from 'react';

function App() {
# leanpub-start-insert
  // do something in between
# leanpub-end-insert

  return (
    <div>
      <h1>Hello World</h1>
    </div>
  );
}

export default App;
~~~~~~~

Variables defined in the function's body will be re-defined each time this function runs, like all JavaScript functions:

{title="src/App.js",lang="javascript"}
~~~~~~~
import React from 'react';

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

Since we don't need anything from within the App component used for this variable -- e.g. parameters coming from the function signature -- we can define the variable outside of the App component as well:

{title="src/App.js",lang="javascript"}
~~~~~~~
import React from 'react';

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

Let's use this variable in the next section.

### Exercises:

* Confirm your [source code for the last section](https://codesandbox.io/s/github/the-road-to-learn-react/hacker-stories/tree/hs/Meet-the-React-Component).
* If you are unsure when to use `const`, `let` or `var` in JavaScript (or React) for variable declarations, make sure to [read more about their differences](https://www.robinwieruch.de/const-let-var).
  * Read more about [const](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/const).
  * Read more about [let](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/let).
* Think about ways to display the `title` variable in your App component's returned HTML. In the next section, we'll put this variable to use.