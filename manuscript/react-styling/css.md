# Styling in React

There are many ways to style a React application, and there are lengthy debates about the best **styling strategy** and **styling approach**. We'll go over a few of these approaches without giving them too much weight. There will be some pro and con arguments, but it's mostly a matter of what fits best for developers and teams.

We will begin React styling with common CSS in React, but then explore two alternatives for more advanced **CSS-in-CSS** (**CSS Modules**) and **CSS-in-JS** (**Styled Components**) strategies. CSS Modules and Styled Components are only two approaches out of many in both groups of strategies. We'll also cover how to include scalable vector graphics (SVGs), such as a logo or icons, in our React application.

![](images/css-style-strategies.png)

If you don't want to build common UI components (e.g. button, dialog, dropdown) from scratch, you can always pick a [popular UI library suited for React](https://www.robinwieruch.de/react-libraries/), which provides these components by default. However, it is better for learning React if you try building these components before using a pre-built solution. As a result, we won't use any of the UI component libraries.

![](images/ui-library.png)

The following styling approaches and SVGs are pre-configured in `create-react-app`. If you're in control of the build tools (e.g. Webpack) by having a custom setup, they might need to be configured to enable importing CSS or SVG files. Since we are using create-react-app, we can use these files as assets right away.

### Exercises:

* Read more about [the different styling strategies and approaches in React](https://www.robinwieruch.de/react-css-styling/).

## CSS in React

Common CSS in React is similar to the standard CSS you may have already learned. Each web application gives HTML elements a `class` (in React it's `className`) attribute that is styled via a CSS file:

{title="src/App.js",lang="javascript"}
~~~~~~~
const App = () => {
  ...

  return (
# leanpub-start-insert
    <div className="container">
      <h1 className="headline-primary">My Hacker Stories</h1>
# leanpub-end-insert

      <SearchForm
        searchTerm={searchTerm}
        onSearchInput={handleSearchInput}
        onSearchSubmit={handleSearchSubmit}
      />

      {stories.isError && <p>Something went wrong ...</p>}

      {stories.isLoading ? (
        <p>Loading ...</p>
      ) : (
        <List list={stories.data} onRemoveItem={handleRemoveStory} />
      )}
    </div>
  );
};
~~~~~~~

The `<hr />` was removed because the CSS handles the border in the next steps. We'll import the CSS file, which is done with the help of the create-react-app configuration:

{title="src/App.js",lang="javascript"}
~~~~~~~
import * as React from 'react';
import axios from 'axios';

# leanpub-start-insert
import './App.css';
# leanpub-end-insert
~~~~~~~

This CSS file will define the two (and more) CSS classes we used (and will use) in the App component. In your *src/App.css* file, define them like the following:

{title="src/App.css",lang="css"}
~~~~~~~
.container {
  height: 100vw;
  padding: 20px;

  background: #83a4d4; /* fallback for old browsers */
  background: linear-gradient(to left, #b6fbff, #83a4d4);

  color: #171212;
}

.headline-primary {
  font-size: 48px;
  font-weight: 300;
  letter-spacing: 2px;
}
~~~~~~~

You should see the first stylings taking effect in your application when you start it again. Next, we will head over to the Item component. Some of its elements receive the `className` attribute too, however, we are also using a new styling technique here:

{title="src/App.js",lang="javascript"}
~~~~~~~
const Item = ({ item, onRemoveItem }) => (
# leanpub-start-insert
  <li className="item">
    <span style={{ width: '40%' }}>
# leanpub-end-insert
      <a href={item.url}>{item.title}</a>
    </span>
# leanpub-start-insert
    <span style={{ width: '30%' }}>{item.author}</span>
    <span style={{ width: '10%' }}>{item.num_comments}</span>
    <span style={{ width: '10%' }}>{item.points}</span>
    <span style={{ width: '10%' }}>
# leanpub-end-insert
      <button
        type="button"
        onClick={() => onRemoveItem(item)}
# leanpub-start-insert
        className="button button_small"
# leanpub-end-insert
      >
        Dismiss
      </button>
    </span>
  </li>
);
~~~~~~~

As you can see, we can also use the `style` attribute for HTML elements. In JSX, style can be passed as an inline JavaScript object to these attributes. This way we can define dynamic style properties in JavaScript files rather than mostly static CSS files. This approach is called **inline style**, which is useful for quick prototyping and dynamic style definitions. Inline style should be used sparingly, however, since a separate style definition with a CSS file keeps the JSX more concise.

In your *src/App.css* file, define the new CSS classes. Basic CSS features are used here, because advanced CSS features (e.g. nesting) from CSS extensions (e.g. Sass) are not included in this example, as they are [optional configurations](https://bit.ly/3E1a2bM):

{title="src/App.css",lang="css"}
~~~~~~~
.item {
  display: flex;
  align-items: center;
  padding-bottom: 5px;
}

.item > span {
  padding: 0 5px;
  white-space: nowrap;
  overflow: hidden;
  white-space: nowrap;
  text-overflow: ellipsis;
}

.item > span > a {
  color: inherit;
}
~~~~~~~

The button style from the previous component is still missing, so we'll define a base button style and two more specific button styles (small and large). One of the button specifications has been already used, the other will be used in the next steps:

{title="src/App.css",lang="css"}
~~~~~~~
.button {
  background: transparent;
  border: 1px solid #171212;
  padding: 5px;
  cursor: pointer;

  transition: all 0.1s ease-in;
}

.button:hover {
  background: #171212;
  color: #ffffff;
}

.button_small {
  padding: 5px;
}

.button_large {
  padding: 10px;
}
~~~~~~~

Apart from styling approaches in React, naming conventions ([CSS guidelines](https://mzl.la/3m5avnb)) are a whole other topic. The last CSS snippet followed BEM rules by defining modifications of a class with an underscore (`_`). Choose whatever naming convention suits you and your team. Without further ado, we will style the next React component:

{title="src/App.js",lang="javascript"}
~~~~~~~
const SearchForm = ({ ... }) => (
# leanpub-start-insert
  <form onSubmit={onSearchSubmit} className="search-form">
# leanpub-end-insert
    <InputWithLabel ... >
      <strong>Search:</strong>
    </InputWithLabel>

    <button
      type="submit"
      disabled={!searchTerm}
# leanpub-start-insert
      className="button button_large"
# leanpub-end-insert
    >
      Submit
    </button>
  </form>
);
~~~~~~~

We can also pass the `className` attribute as a prop to React components. For example, we can use this option to pass the SearchForm component a flexible style with a `className` prop from a range of predefined classes (e.g. `button_large` or `button_small`) from a CSS file. Lastly, style the InputWithLabel component:

{title="src/App.js",lang="javascript"}
~~~~~~~
const InputWithLabel = ({ ... }) => {
  ...

  return (
    <>
# leanpub-start-insert
      <label htmlFor={id} className="label">
# leanpub-end-insert
        {children}
      </label>
      &nbsp;
      <input
        ref={inputRef}
        id={id}
        type={type}
        value={value}
        onChange={onInputChange}
# leanpub-start-insert
        className="input"
# leanpub-end-insert
      />
    </>
  );
};
~~~~~~~

In your *src/App.css* file, add the remaining classes:

{title="src/App.css",lang="css"}
~~~~~~~
.search-form {
  padding: 10px 0 20px 0;
  display: flex;
  align-items: baseline;
}

.label {
  border-top: 1px solid #171212;
  border-left: 1px solid #171212;
  padding-left: 5px;
  font-size: 24px;
}

.input {
  border: none;
  border-bottom: 1px solid #171212;
  background-color: transparent;

  font-size: 24px;
}
~~~~~~~

For simplicity, we styled elements like label and input individually in the *src/App.css* file. However, in a real application, it may be better to define these elements once in the *src/index.css* file globally. As React components are split into multiple files, sharing style becomes a necessity. After all, this is the basic usage of CSS in React. Without CSS extensions like Sass (Syntactically Awesome Style Sheets), styling can become more burdensome, though, because features like CSS nesting are not available in native CSS.

### Exercises:

* Confirm your [source code](https://bit.ly/3jk9llQ).
  * Confirm the [changes](https://bit.ly/3vrrnr4).
* Read more about [CSS stylesheets in create-react-app](https://bit.ly/3G0fTjn).
  * Try to pass `className` prop from App to SearchForm component, either with the value `button_small` or `button_large`, and use this as `className` for the button element.
* Read more about [Sass in create-react-app](https://bit.ly/3GeJGF9) for taking advantage of more advanced CSS features like nesting.
* Enable Sass in your create-react-app and start to use its features (e.g. nesting) in your CSS file.
* Optional: [Leave feedback for this section](https://forms.gle/RovYbjYF9McD1h6c7).