## Styled Components in React

With the previous approaches from CSS-in-CSS, Styled Components is one of several approaches for **CSS-in-JS**. I picked Styled Components because it's the most popular. It comes as a JavaScript dependency, so we must install it on the command line:

{title="Command Line",lang="text"}
~~~~~~~
npm install styled-components
~~~~~~~

Then import it in your *src/App.jsx* file:

{title="src/App.jsx",lang="javascript"}
~~~~~~~
import * as React from 'react';
import axios from 'axios';
# leanpub-start-insert
import styled from 'styled-components';
# leanpub-end-insert
~~~~~~~

As the name suggests, CSS-in-JS happens in your JavaScript file. In your *src/App.jsx* file, define your first styled components:

{title="src/App.jsx",lang="javascript"}
~~~~~~~
const StyledContainer = styled.div`
  height: 100vw;
  padding: 20px;

  background: #83a4d4;
  background: linear-gradient(to left, #b6fbff, #83a4d4);

  color: #171212;
`;

const StyledHeadlinePrimary = styled.h1`
  font-size: 48px;
  font-weight: 300;
  letter-spacing: 2px;
`;
~~~~~~~

When using Styled Components, you are using the JavaScript template literals the same way as JavaScript functions. Everything between the backticks can be seen as an argument and the `styled` object gives you access to all the necessary HTML elements (e.g. div, h1) as functions. Once a function is called with the style, it returns a React component that can be used in your App component:

{title="src/App.jsx",lang="javascript"}
~~~~~~~
const App = () => {
  ...

  return (
# leanpub-start-insert
    <StyledContainer>
      <StyledHeadlinePrimary>My Hacker Stories</StyledHeadlinePrimary>
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
# leanpub-start-insert
    </StyledContainer>
# leanpub-end-insert
  );
};
~~~~~~~

This kind of React component follows the same rules as a common React component. Everything passed between its element tags is passed automatically as React `children` prop. For the Item component, we are not using inline styles this time, but defining a dedicated styled component for it. `StyledColumn` receives its styles dynamically using a React prop:

{title="src/App.jsx",lang="javascript"}
~~~~~~~
const Item = ({ item, onRemoveItem }) => (
# leanpub-start-insert
  <StyledItem>
    <StyledColumn width="40%">
# leanpub-end-insert
      <a href={item.url}>{item.title}</a>
# leanpub-start-insert
    </StyledColumn>
    <StyledColumn width="30%">{item.author}</StyledColumn>
    <StyledColumn width="10%">{item.num_comments}</StyledColumn>
    <StyledColumn width="10%">{item.points}</StyledColumn>
    <StyledColumn width="10%">
      <StyledButtonSmall
# leanpub-end-insert
        type="button"
        onClick={() => onRemoveItem(item)}
      >
        Dismiss
# leanpub-start-insert
      </StyledButtonSmall>
    </StyledColumn>
  </StyledItem>
# leanpub-end-insert
);
~~~~~~~

The flexible `width` prop is accessible in the styled component's template literal as an argument of an inline function. The return value from the function is applied there as a string. Since we can use immediate returns when omitting the arrow function's body, it becomes a concise inline function:

{title="src/App.jsx",lang="javascript"}
~~~~~~~
const StyledItem = styled.li`
  display: flex;
  align-items: center;
  padding-bottom: 5px;
`;

const StyledColumn = styled.span`
  padding: 0 5px;
  white-space: nowrap;
  overflow: hidden;
  white-space: nowrap;
  text-overflow: ellipsis;

  a {
    color: inherit;
  }

  width: ${(props) => props.width};
`;
~~~~~~~

Advanced features like CSS nesting are available in Styled Components by default. Nested elements are accessible and the current element can be selected with the `&` CSS operator:

{title="src/App.jsx",lang="javascript"}
~~~~~~~
const StyledButton = styled.button`
  background: transparent;
  border: 1px solid #171212;
  padding: 5px;
  cursor: pointer;

  transition: all 0.1s ease-in;

  &:hover {
    background: #171212;
    color: #ffffff;
  }
`;
~~~~~~~

You can also create specialized versions of styled components by passing another component to the library's function. The specialized button receives all the base styles from the previously defined StyledButton component:

{title="src/App.jsx",lang="javascript"}
~~~~~~~
const StyledButtonSmall = styled(StyledButton)`
  padding: 5px;
`;

const StyledButtonLarge = styled(StyledButton)`
  padding: 10px;
`;

const StyledSearchForm = styled.form`
  padding: 10px 0 20px 0;
  display: flex;
  align-items: baseline;
`;
~~~~~~~

When we use a styled component like StyledSearchForm, its underlying form element is used in the real HTML output. We can continue using the native HTML attributes (`onSubmit`, `type`, `disabled`) there:

{title="src/App.jsx",lang="javascript"}
~~~~~~~
const SearchForm = ({ ... }) => (
# leanpub-start-insert
  <StyledSearchForm onSubmit={onSearchSubmit}>
# leanpub-end-insert
    <InputWithLabel
      id="search"
      value={searchTerm}
      isFocused
      onInputChange={onSearchInput}
    >
      <strong>Search:</strong>
    </InputWithLabel>

# leanpub-start-insert
    <StyledButtonLarge type="submit" disabled={!searchTerm}>
# leanpub-end-insert
      Submit
# leanpub-start-insert
    </StyledButtonLarge>
  </StyledSearchForm>
# leanpub-end-insert
);
~~~~~~~

Finally, the InputWithLabel decorated with its yet undefined styled components:

{title="src/App.jsx",lang="javascript"}
~~~~~~~
const InputWithLabel = ({ ... }) => {
  ...

  return (
    <>
# leanpub-start-insert
      <StyledLabel htmlFor={id}>{children}</StyledLabel>
# leanpub-end-insert
      &nbsp;
# leanpub-start-insert
      <StyledInput
# leanpub-end-insert
        ref={inputRef}
        id={id}
        type={type}
        value={value}
        onChange={onInputChange}
      />
    </>
  );
};
~~~~~~~

And its matching styled components are defined in the same file:

{title="src/App.jsx",lang="javascript"}
~~~~~~~
const StyledLabel = styled.label`
  border-top: 1px solid #171212;
  border-left: 1px solid #171212;
  padding-left: 5px;
  font-size: 24px;
`;

const StyledInput = styled.input`
  border: none;
  border-bottom: 1px solid #171212;
  background-color: transparent;

  font-size: 24px;
`;
~~~~~~~

CSS-in-JS with styled components shifts the focus of defining styles to actual React components. Styled Components are styles defined as React components without the intermediate CSS file. If a styled component isn't used in a JavaScript, your IDE/editor will tell you. Styled Components are bundled next to other JavaScript assets in JavaScript files for a production-ready application. There are no extra CSS files, but only JavaScript when using the CSS-in-JS strategy. Both strategies, CSS-in-JS and CSS-in-CSS, and their approaches (e.g. Styled Components and CSS Modules) are popular among React developers. Use what suits you and your team best.

### Exercises:

* Compare your source code against the author's [source code](https://bit.ly/3dwNzM9).
  * Recap all the [source code changes from this section](https://bit.ly/3UAbj2e).
* Read more about [best practices for Styled Components in React](https://www.robinwieruch.de/styled-components/).
* Usually there is no *src/index.css* file for global styles when using Styled Components. Find out how to use global styles when using Styled Components with the help of your favorite search engine.
* Optional: [Leave feedback for this section](https://forms.gle/5vFxvg9hSNAna37S8).