## Handler Function in JSX

The App component still has the input field and label, which we haven't used. In HTML outside of JSX, input fields have an [onchange handler](https://developer.mozilla.org/en-US/docs/Web/API/GlobalEventHandlers/onchange). We're going to discover how to use onchange handlers with a React component's JSX. First, refactor the App component from a concise body to a block body so we can add implementation details.

{title="src/App.js",lang="javascript"}
~~~~~~~
# leanpub-start-insert
const App = () => {
  // do something in between

  return (
# leanpub-end-insert
    <div>
      <h1>My Hacker Stories</h1>

      <label htmlFor="search">Search: </label>
      <input id="search" type="text" />

      <hr />

      <List />
    </div>
# leanpub-start-insert
  );
};
# leanpub-end-insert
~~~~~~~

Next, define a function -- which can be normal or arrow -- for the change event of the input field. In React, this function is called an **(event) handler**. Now the function can be passed to the `onChange` attribute (JSX named attribute) of the input field.

{title="src/App.js",lang="javascript"}
~~~~~~~
const App = () => {
# leanpub-start-insert
  const handleChange = event => {
    console.log(event);
  };
# leanpub-end-insert

  return (
    <div>
      <h1>My Hacker Stories</h1>

      <label htmlFor="search">Search: </label>
# leanpub-start-insert
      <input id="search" type="text" onChange={handleChange} />
# leanpub-end-insert

      <hr />

      <List />
    </div>
  );
};
~~~~~~~

After opening your application in a web browser, open the browser's developer tools to see logging occur after you type into the input field. This is called a **synthetic event** defined by a JavaScript object. Through this object, we can access the emitted value of the input field:

{title="src/App.js",lang="javascript"}
~~~~~~~
const App = () => {
  const handleChange = event => {
# leanpub-start-insert
    console.log(event.target.value);
# leanpub-end-insert
  };

  return ( ... );
};
~~~~~~~

The synthetic event is essentially a wrapper around the [browser's native event](https://developer.mozilla.org/en-US/docs/Web/Events), with more functions that are useful to prevent native browser behavior (e.g. refreshing a page after the user clicks a form's submit button). Sometimes you will use the event, sometimes you won't need it.

This is how we give HTML elements in JSX handler functions to respond to user interaction. Always pass functions to these handlers, not the return value of the function, except when the return value is a function:

{title="Code Playground",lang="javascript"}
~~~~~~~
// don't do this
<input
  id="search"
  type="text"
# leanpub-start-insert
  onChange={handleChange()}
# leanpub-end-insert
/>

// do this instead
<input
  id="search"
  type="text"
# leanpub-start-insert
  onChange={handleChange}
# leanpub-end-insert
/>
~~~~~~~

HTML and JavaScript work well together in JSX. JavaScript in HTML can display objects, can pass JavaScript primitives to HTML attributes (e.g. `href` to `<a>`), and can pass functions to an element's attributes for handling events.

I prefer using arrow functions because of their concision as event handlers, however, in a larger React component I see myself using the function statements too, because it gives them more visibility in contrast to other variable declarations within a component's body.

### Exercises:

* Confirm your [source code for the last section](https://codesandbox.io/s/github/the-road-to-learn-react/hacker-stories/tree/hs/Handler-Function-in-JSX).
  * Confirm the [changes from the last section](https://github.com/the-road-to-learn-react/hacker-stories/compare/hs/React-Component-Definition...hs/Handler-Function-in-JSX?expand=1).
* Read more about [React's events](https://reactjs.org/docs/events.html).