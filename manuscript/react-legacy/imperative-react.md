## Imperative React

In a React function component, React's useRef Hook is used mostly for imperative programming. Throughout React's history, the *ref* and its usage had a few versions:

* String Refs (deprecated)
* Callback Refs (used in class and function components)
* createRef Refs (exclusive for class components)
* useRef Hook Refs (exclusive for function components)

The React team introduced **React's createRef** with version 16.3 as the latest equivalent to a function component's useRef hook which has been integrated with React Hooks in 16.8:

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

With the helper function, the ref is created in the class' constructor, applied in the JSX for the `ref` attribute, and here used in a class component's lifecycle method (equivalent to a function component's useEffect Hook). The ref can also be used elsewhere, like focusing the input field on a button click.

Where createRef is used in React's class components, React's useRef Hook is used in React function components. As React shifts towards function components, today it's common practice to use the useRef hook exclusively to manage refs and apply imperative programming principles.

### Exercises:

* Read more about [the different ref techniques in React](https://bit.ly/3jjhCXa).
* Optional: [Leave feedback for this section](https://forms.gle/xd131YRHy6NdZa7p7).