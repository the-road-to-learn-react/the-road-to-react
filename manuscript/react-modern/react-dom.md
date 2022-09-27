## React DOM

We have learned about component declaration/instantiation and have already seen it in action for the List and Search components. However, at the very beginning we started with the declaration of the App component yet never came across its instantiation. It must be there, otherwise the App component and all of its descendant components in the component hierarchy would not render.

Open the *src/main.jsx* file to the see App components instantiation with the `<App />` element. The file may differ a bit from your file, however, the following snippet shows all the essential aspects of it:

{title="src/main.jsx",lang="javascript"}
~~~~~~~
import React from 'react';
import ReactDOM from 'react-dom/client';
import App from './App';

import './index.css';

ReactDOM.createRoot(document.getElementById('root')).render(
  <React.StrictMode>
    <App />
  </React.StrictMode>
);
~~~~~~~

There are two libraries imported at the beginning of the file: `react` and `react-dom`. While React is used for the day to day business of a React developer, React DOM is usually used once in a React application to hook React into the native HTML world. Open the *index.html* file on the side and spot the HTML element where the `id` attribute equals `"root"`. That's exactly the element where React inserts itself into the HTML to bootstrap the entire React application -- starting with the App component.

In the JavaScript file, the  `createRoot()` method expects the HTML element that is used to instantiate React. There we are using JavaScript's built-in `getElementById()` method to return the HTML element that we have seen in the *index.html* file. Once we have the `root` object, we can call `render()` on the returned root object with JSX as parameter which usually represents the entry point component (also called root component). Normally the entry point component is the instance of the App component, but it can be any other JSX too:

{title="Code Playground",lang="javascript"}
~~~~~~~
const title = 'React';

ReactDOM.createRoot(document.getElementById('root')).render(
  <h1>Hello {title}</h1>
);
~~~~~~~

Essentially React DOM is everything that's needed to integrate React into any website which uses HTML. If you start a React application from scratch, there is usually only one `ReactDOM.createRoot()` call in your application. However, if you happen to work on a legacy application that used something else than React before, you may see multiple `ReactDOM.createRoot()` calls, because only certain areas of the application are powered by React.

Anyway, do you recall the introduction about the rise of single-page applications that are powered by only a small HTML file and a large JavaScript file? You can see how everything fits together now. While one small HTML file (here: *index.html*) and one large JavaScript file (here: compiled and bundled *src/main.jsx* and *src/App.jsx* files) are transferred from web server to browser, the JavaScript file(s) are mostly responsible to render all the HTML in the browser. The HTML file is only there to request the JavaScript file and to render the HTML element where React inserts itself. From there, React calls all of its needed function components to render itself as component hierarchy.

### Exercises:

* Read more about [rendering elements in React](https://bit.ly/3aUySgP).
* Read more about [React's StrictMode](https://bit.ly/3SufTxx).
* Optional: [Leave feedback for this section](https://forms.gle/zSqHUhmsuQ35vqoj9).