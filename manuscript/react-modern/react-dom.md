## React DOM

Now that we've learned about component definitions and their instantiation, we can move to the App component's instantiation. It has been in our application from the start, in the *src/index.js* file:

{title="src/index.js",lang="javascript"}
~~~~~~~
import React from 'react';
import ReactDOM from 'react-dom';

import App from './App';

ReactDOM.render(
  <App />,
  document.getElementById('root')
);
~~~~~~~

Next to React, there is another imported library called `react-dom`, in which a `ReactDOM.render()` function uses an HTML node to replace it with JSX. The process integrates React into HTML. `ReactDOM.render()` expects two arguments; the first is to render the JSX. It creates an instance of your App component, though it can also pass simple JSX without any component instantiation.

{title="Code Playground",lang="javascript"}
~~~~~~~
ReactDOM.render(
  <h1>Hello React World</h1>,
  document.getElementById('root')
);
~~~~~~~

The second argument specifies where the React application enters your HTML. It expects an element with an `id='root'`, found in the *public/index.html* file. This is a basic HTML file.

### Exercises:

* Open the *public/index.html* to see where the React application enters your HTML.
* Consider how we can include a React application in an external web application that uses HTML.
* Read more about [rendering elements in React](https://reactjs.org/docs/rendering-elements.html).