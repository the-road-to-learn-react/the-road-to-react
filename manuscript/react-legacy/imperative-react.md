## Imperative React

In a React function component, React's useRef Hook is used mostly for imperative programming. Throughout React's history, the *ref* and its usage had a few versions:

* String Refs (deprecated)
* Callback Refs
* createRef Refs (exclusive for Class Components)
* useRef Hook Refs (exclusive for Function Components)

Recently, the React team introduced **React's createRef** as the  latest equivalent to a function component's useRef hook:

{title="Code Playground",lang="javascript"}
~~~~~~~
class InputWithLabel extends React.Component {
  constructor(props) {
    super(props);

# leanpub-start-insert
    this.inputRef = React.createRef();
# leanpub-end-insert
  }

  componentDidMount() {
    if (this.props.isFocused) {
# leanpub-start-insert
      this.inputRef.current.focus();
# leanpub-end-insert
    }
  }

  render() {
    ...

    return (
      <>
        ...
        <input
# leanpub-start-insert
          ref={this.inputRef}
# leanpub-end-insert
          id={id}
          type={type}
          value={value}
          onChange={onInputChange}
        />
      </>
    );
  }
}
~~~~~~~

With the helper function, the ref is created in the class' constructor, applied in the JSX for the `ref` attributed, and here used in a lifecycle method. The ref can also be used elsewhere, like focusing the input field on a button click.

Where createRef is used in React's class components, React's useRef Hook is used in React function components. As React shifts towards function components, today its common practice to use the useRef hook exclusively to manage refs and apply imperative programming principles.

### Exercises:

* Read more about [the different ref techniques in React](https://reactjs.org/docs/refs-and-the-dom.html).