## React DOM

We have learned about component declaration/instantiation and have already seen it in action for the List and Search components. However, at the very beginning we started with the declaration of the App component yet never came across its instantiation. It must be there, otherwise the App component and all of its descendant components in the component hierarchy would not render.

Open the *src/index.js* file to the see App components instantiation with `<App />`. The file may differ a bit from your file, however, the following version shows all the important aspects of it:

{title="src/index.js",lang="javascript"}
~~~~~~~
import * as React from 'react';
import ReactDOM from 'react-dom';

import App from './App';

ReactDOM.render(
  <App />,
  document.getElementById('root')
);
~~~~~~~

There are two libraries imported at the beginning of the file: react and react-dom. While React is used for the day to day business of a React developer, React DOM is usually used once in a React application to hook React into the native HTML world. Open the *public/index.html* on the side and verify yourself whether you can spot a HTML element where the `id` attribute equals `"root"`. That's exactly the element where React hooks into the HTML to bootstrap the entire React application -- starting with the App component.

In the JavaScript file, the `render()` method of `ReactDOM` expects two arguments. While the latter argument is an element which we have spotted in the HTML file, the former argument is JSX which usually represents the entry point component (also called root component). Normally the entry point component is the instance of the App component, but it can be any other JSX too:

{title="Code Playground",lang="javascript"}
~~~~~~~
const title = 'React';

ReactDOM.render(
  <h1>Hello {title}</h1>,
  document.getElementById('root')
);
~~~~~~~

Essentially React DOM is everything that's needed to integrate React into any website which uses HTML. If you start a React application from scratch, there is usually only one of these `ReactDOM.render()` calls in your application. However, if you happen to work on a legacy application that used something else than React before, you may see multiple `ReactDOM.render()` calls, because only certain areas of the application are powered by React.

### Exercises:

* Read more about [rendering elements in React](https://bit.ly/3aUySgP).
* Optional: [Leave feedback for this section](https://forms.gle/zSqHUhmsuQ35vqoj9).