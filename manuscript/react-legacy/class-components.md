# React's Legacy

React has changed a lot since 2013. The iterations of its library, how React applications are written, and especially its components have all changed drastically. However, many React applications were built over the last few years, so not everything was created with the current status quo in mind. This section of the book covers React's legacy. However, I won't cover all that's considered legacy in React, because some features have been revamped more than once.

Throughout this section, we will compare a [modern React application](https://bit.ly/3vrKgu9) to its [legacy version](https://bit.ly/2Z1matY). We'll discover that most differences between modern and legacy React are due to class components versus function components.

## React Class Components

React components have undergone many changes, from **createClass components** over **class components**, to **function components**. Going through a React application today, it's likely that we'll see class components next to the modern function components. Fortunately, you will not see many **createClass components** anymore.

A typical class component is a JavaScript class with a mandatory **render method** that returns the JSX. The class extends from a `React.Component` to inherit ([class inheritance](https://bit.ly/3vxh3xY)) all React's component features (e.g. state management, lifecycle methods for side-effects). React `props` are accessed via the class instance (`this`) in any class method (e.g. `render`):

{title="Code Playground",lang="javascript"}
~~~~~~~
class InputWithLabel extends React.Component {
  render() {
    const {
      id,
      value,
      type = 'text',
      onInputChange,
      children,
    } = this.props;

    return (
      <>
        <label htmlFor={id}>{children}</label>
        &nbsp;
        <input
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

For a while, class components were the popular choice for writing React applications. Eventually, function components were added, and both co-existed with their distinct purposes side by side:

{title="Code Playground",lang="javascript"}
~~~~~~~
const InputWithLabel = ({
  id,
  value,
  type = 'text',
  onInputChange,
  children,
}) => (
  <>
    <label htmlFor={id}>{children}</label>
    &nbsp;
    <input
      id={id}
      type={type}
      value={value}
      onChange={onInputChange}
    />
  </>
);
~~~~~~~

If no side-effects and no state were used in legacy apps, we'd use a function component instead of a class component. Before 2018 -- before React Hooks were introduced -- React's function components couldn't handle side-effects (`useEffect` hooks) or state (`useState`/`useReducer` hooks). As a result, these components were known as **functional stateless components**, there only to input props and output JSX. To use state or side-effects, it was necessary to refactor from a function component to a class component. When neither state nor side-effects were used, we used class components or more commonly the more lightweight function component.

With the addition of React Hooks, function components were granted the same features as class components, with state and side-effects. And since there was no longer any practical difference between them, the community chose function components over class components since they are more lightweight.

### Exercises:

* Read more about [JavaScript Classes](https://mzl.la/3vvc2FO).
* Read more about [how to refactor from a class component to a function component](https://www.robinwieruch.de/react-hooks-migration/).
* Learn more about a different [class component syntax](https://bit.ly/3lYzrfT) which wasn't popular but more effective.
* Read more about [class components in depth](https://bit.ly/3FXUibf).
* Read more about [other legacy and modern component types in React](https://www.robinwieruch.de/react-component-types/).
* Optional: [Leave feedback for this section](https://forms.gle/g5qLH1KZ5Y1JE1v57).