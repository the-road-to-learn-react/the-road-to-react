## Handler Function in JSX

We have learned a lot about React components, but there are no interactions yet. If you happen to develop an application with React, there will come a time where you have to implement a user interaction. The best place to get started in our project is the Search component -- which already comes with an input field element.

In native HTML, we can [add event handlers](https://mzl.la/2ZbTcYZ) on elements by using the `addEventListener()` method programmatically on a DOM node. In React, we are going to discover how to add handlers in JSX the declarative way. First, refactor the Search component's function from a concise body to a block body, so that we can add implementation details prior the return statement:

{title="src/App.jsx",lang="javascript"}
~~~~~~~
# leanpub-start-insert
const Search = () => {
  // perform a task in between

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

Next, define a function, which can be either a function declaration or arrow function expression, for the change event of the input field. In React, this function is called an **(event) handler**. Afterward, the function can be passed to the `onChange` attribute (JSX named attribute) of the HTML input field:

{title="src/App.jsx",lang="javascript"}
~~~~~~~
const Search = () => {
# leanpub-start-insert
  const handleChange = (event) => {
    // synthetic event
    console.log(event);
    // value of target (here: input HTML element)
    console.log(event.target.value);
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

After opening your application in a web browser, open the browser's developer tools "Console"-tab to see the logging occur after you type into the input field. What you see is called a **synthetic event** as a JavaScript object and the input field's internal value.

React's synthetic event is essentially a wrapper around the [browser's native event](https://mzl.la/30Dk8kt). Since React started as library for single-page applications, there was the need for enhanced functionalities on the event to [prevent the native browser behavior](https://www.robinwieruch.de/react-preventdefault/). For example, in native HTML submitting a form triggers a page refresh. However, in React this page refresh should be prevented, because the developer should take care about what happens next. Anyway, if you happen to need access to the native HTML event, you could do so by using `event.nativeEvent`, but after several years of React development I never ran into this case myself.

After all, this is how we pass HTML elements in JSX handler functions to add listeners for user interactions. Always pass functions to these handlers, not the return value of the function, except when the return value is a function again. Knowing this is crucial because it's a well-known source for bugs in a React beginner's application:

{title="Code Playground",lang="javascript"}
~~~~~~~
// if handleChange is a function
// which does not return a function
// don't do this
<input onChange={handleChange()} />

// do this instead
<input onChange={handleChange} />
~~~~~~~

As you can see, HTML and JavaScript work well together in JSX. JavaScript in HTML can display JavaScript variables (e.g. `title` string in `<span>{title}</span>`), can pass JavaScript primitives to HTML attributes (e.g. `url` string to `<a href={url}>` HTML element), and can pass functions to an HTML element's attributes for handling user interactions (e.g. `handleChange` function to `<input onChange={handleChange} />`). When developing React applications, mixing HTML and JavaScript in JSX will become your bread and butter.

### Exercises:

* Compare your source code against the author's [source code](https://bit.ly/3Us2EPz).
  * Recap all the [source code changes from this section](https://bit.ly/3UuCxr6).
  * Optional: If you are using TypeScript, check out the author's source code [here](https://bit.ly/3LNvo1h).
* Read more about [React's event handler](https://www.robinwieruch.de/react-event-handler/).
  * Read more about [event capturing and bubbling in React](https://www.robinwieruch.de/react-event-bubbling-capturing/).
* In addition to the `onChange` attribute, add a `onBlur` attribute with an event handler to your input field and verify its logging in the browser's developer tools.
* Optional: [Leave feedback for this section](https://forms.gle/oSKyMudmb8X1iSsv8).