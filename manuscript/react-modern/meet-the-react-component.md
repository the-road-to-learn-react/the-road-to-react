## Meet the React Component

Every React application is built on the foundation of **React components**. In this section, you will get to know your first React component which is located in the *src/App.jsx* file and which should look similar to the example below. Depending on your Vite version, the content of the file might differ slightly:

{title="src/App.jsx",lang="javascript"}
~~~~~~~
import { useState } from 'react';
import reactLogo from './assets/react.svg';
import './App.css';

function App() {
  const [count, setCount] = useState(0);

  return (
    <div className="App">
      <div>
        <a href="https://vitejs.dev" target="_blank">
          <img src="/vite.svg" className="logo" alt="Vite logo" />
        </a>
        <a href="https://reactjs.org" target="_blank">
          <img
            src={reactLogo}
            className="logo react"
            alt="React logo"
          />
        </a>
      </div>
      <h1>Vite + React</h1>
      <div className="card">
        <button onClick={() => setCount((count) => count + 1)}>
          count is {count}
        </button>
        <p>
          Edit <code>src/App.jsx</code> and save to test HMR
        </p>
      </div>
      <p className="read-the-docs">
        Click on the Vite and React logos to learn more
      </p>
    </div>
  );
}

export default App;
~~~~~~~

This file will be our focus throughout this book, unless otherwise specified. Even though this file will grow in size, because we are not splitting it up from the beginning into multiple files, it will be simpler to understand as a beginner, because everything is at one place. Once you get more comfortable with React, I will show you how to split your React project into multiple files.

Let's start by reducing the component to a more lightweight version for getting you started without too much distracting [boilerplate code](https://bit.ly/3lZzckS):

{title="src/App.jsx",lang="javascript"}
~~~~~~~
# leanpub-start-insert
import * as React from 'react';

function App() {
  return (
    <div>
      <h1>Hello React</h1>
    </div>
  );
}

export default App;
# leanpub-end-insert
~~~~~~~

Optionally you can also make the *src/index.css* and *src/App.css* files blank for starting from a clean slate style-wise.

Next, start your application with `npm run dev` on the command line and check what's displayed in the browser. You should see the headline "Hello React" showing up. Before we dive deeper into each topic, here comes a quick overview of what's in your code and what we will cover more in-depth in the following sections:

* First, this React component, specifically called App component, is just a JavaScript function. In contrast to traditional JavaScript functions, it's defined in [PascalCase](https://www.robinwieruch.de/javascript-naming-conventions/). A component has to start with a capital letter, otherwise it isn't treated as component in React. The kind of the App component is commonly called a **function component**. Function components are the modern way of using components in React, however, be aware that there are other variations of React components (see **component types** in a later section) too.
* Second, the App component doesn't have any parameters in its function signature yet. In the upcoming sections, you will learn how to pass information (see **props** in a later section) from one component to another component. These props will be accessible via the function's signature as parameters then.
* And third, the App component returns code that resembles HTML. You will see how this new syntax (see **JSX** in a later section), allows you to combine JavaScript and HTML for displaying highly dynamic and interactive content in a browser.

Like any other JavaScript function, a function component can have implementation details between the function signature and the return statement. You will see this in practice in action throughout your React journey:

{title="src/App.jsx",lang="javascript"}
~~~~~~~
import * as React from 'react';

function App() {
# leanpub-start-insert
  // you can do something in between
# leanpub-end-insert

  return (
    <div>
      <h1>Hello React</h1>
    </div>
  );
}

export default App;
~~~~~~~

Variables defined in the function's body will be re-defined each time this function runs, which shouldn't be something new if you are familiar with JavaScript and its functions:

{title="src/App.jsx",lang="javascript"}
~~~~~~~
import * as React from 'react';

function App() {
# leanpub-start-insert
  const title = 'React';
# leanpub-end-insert

  return (
    <div>
      <h1>Hello React</h1>
    </div>
  );
}

export default App;
~~~~~~~

The function of a component runs every time a component is displayed in the browser. This happens for the initial rendering (read: displaying) of the component, but also whenever the component updates because it has to display something different due to changes (re-rendering). We will learn more about this later too.

Since we do not want to re-define a variable within a function every time this function runs, we could define this variable outside of the component as well. In this case, the `title` does not depend on any information that's within the function component (e.g. parameters coming from the function's signature), hence it's okay to move it outside. Therefore it will be defined only once and not every time the function is called:

{title="src/App.jsx",lang="javascript"}
~~~~~~~
import * as React from 'react';

# leanpub-start-insert
const title = 'React';
# leanpub-end-insert

function App() {
  return (
    <div>
      <h1>Hello React</h1>
    </div>
  );
}

export default App;
~~~~~~~

On your journey as a React developer, and in this learning experience, you will come across both scenarios: variables (and functions) defined outside and within a component. As a rule of thumb: If a variable does not need anything from within the function component's body (e.g. parameters), then define it outside of the component which avoids re-defining it on every function call.

### Exercises:

* Compare your source code against the author's [source code](https://bit.ly/3UvoTEn).
  * Optional: If you are using TypeScript, check out the author's source code [here](https://bit.ly/3RhDTm8).
* Think about ways to display the `title` variable in your App component's returned HTML. In the next section, we'll put this variable to use.
* Optional: [Leave feedback for this section](https://forms.gle/VYiZqqjzXGE11wCv6).