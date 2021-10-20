## Handler Function in JSX

The Search component still has the input field and label, which we haven't used. In HTML outside of JSX, input fields have an [onchange handler](https://mzl.la/3n9wit4). We're going to discover how to use onchange handlers with a React component's JSX. First, refactor the Search component from a concise body to a block body so we can add implementation details:

{title="src/App.js",lang="javascript"}
~~~~~~~
# leanpub-start-insert
const Search = () => {
  // do something in between

  return (
# leanpub-end-insert
    <div>
      <label htmlFor="search">Search: </label>
      <input id="search" type="text" />
    </div>
# leanpub-start-insert
  );
};
# leanpub-end-insert
~~~~~~~

Next, define a function -- which can be a normal or arrow function -- for the change event of the input field. In React, this function is called an **(event) handler**. Then the function can be passed to the `onChange` attribute (JSX named attribute) of the HTML input field:

{title="src/App.js",lang="javascript"}
~~~~~~~
const Search = () => {
# leanpub-start-insert
  const handleChange = (event) => {
    console.log(event);
  };
# leanpub-end-insert

  return (
    <div>
      <label htmlFor="search">Search: </label>
# leanpub-start-insert
      <input id="search" type="text" onChange={handleChange} />
# leanpub-end-insert
    </div>
  );
};
~~~~~~~

After opening your application in a web browser, open the browser's developer tools to see logging occur after you type into the input field. What you see is called a **synthetic event** defined by a JavaScript object. Through this object, we can access the emitted value of the input field:

{title="src/App.js",lang="javascript"}
~~~~~~~
const Search = () => {
  const handleChange = (event) => {
# leanpub-start-insert
    console.log(event.target.value);
# leanpub-end-insert
  };

  return ( ... );
};
~~~~~~~

React's synthetic event is essentially a wrapper around the [browser's native event](https://mzl.la/30Dk8kt), with more functions that are useful to prevent native browser behavior (e.g. refreshing a page after the user clicks a form's submit button). Sometimes you will use the event, sometimes you won't need it.

After all, this is how we give HTML elements in JSX handler functions to respond to user interaction. Always pass functions to these handlers, not the return value of the function, except when the return value is a function. Knowing this is crucial because it's a well-known source for bugs in a React beginners application:

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

As you can see, HTML and JavaScript work well together in JSX. JavaScript in HTML can display JavaScript variables (e.g. `title` JavaScript string in <span>{title}</span>), can pass JavaScript primitives to HTML attributes (e.g. `url` JavaScript string to `<a href={url}>` HTML element), and can pass functions to an HTML element's attributes for handling user interactions.

### Exercises:

* Confirm your [source code](https://bit.ly/3lY8usB).
  * Confirm the [changes](https://bit.ly/3BYqQzp).
* Read more about [React's event handler](https://www.robinwieruch.de/react-event-handler) and [React's events](https://bit.ly/3jiFdaz).