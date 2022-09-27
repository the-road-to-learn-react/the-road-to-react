## CSS Modules in React

CSS Modules are an advanced **CSS-in-CSS** approach. The CSS file stays the same, where you could apply CSS extensions like Sass, but its use in React components changes. To enable CSS modules in Vite, rename the *src/App.css* file to *src/App.module.css*. This action is performed on the command line from your project's directory:

{title="Command Line",lang="text"}
~~~~~~~
mv src/App.css src/App.module.css
~~~~~~~

In the renamed *src/App.module.css*, start with the first CSS class definitions, as before:

{title="src/App.module.css",lang="css"}
~~~~~~~
.container {
  height: 100vw;
  padding: 20px;

  background: #83a4d4; /* fallback for old browsers */
  background: linear-gradient(to left, #b6fbff, #83a4d4);

  color: #171212;
}

.headlinePrimary {
  font-size: 48px;
  font-weight: 300;
  letter-spacing: 2px;
}
~~~~~~~

Import the *src/App.module.css* file with a relative path again. This time, import it as a JavaScript object where the name of the object (here: `styles`) is up to you:

{title="src/App.jsx",lang="javascript"}
~~~~~~~
import * as React from 'react';
import axios from 'axios';

# leanpub-start-insert
import styles from './App.module.css';
# leanpub-end-insert
~~~~~~~

Instead of defining the `className` as a string mapped to a CSS file, access the CSS class directly from the `styles` object, and assign it with a JavaScript in JSX expression to your elements.

{title="src/App.jsx",lang="javascript"}
~~~~~~~
const App = () => {
  ...

  return (
# leanpub-start-insert
    <div className={styles.container}>
      <h1 className={styles.headlinePrimary}>My Hacker Stories</h1>
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

There are various ways to add multiple CSS classes via the `styles` object to the element's single `className` attribute. Here, we use JavaScript template literals:

{title="src/App.jsx",lang="javascript"}
~~~~~~~
const Item = ({ item, onRemoveItem }) => (
# leanpub-start-insert
  <li className={styles.item}>
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
        className={`${styles.button} ${styles.buttonSmall}`}
# leanpub-end-insert
      >
        Dismiss
      </button>
    </span>
  </li>
);
~~~~~~~

We can also add inline styles as more dynamic styles in JSX again. It's also possible to add a CSS extension like Sass to enable advanced features like CSS nesting (see the previous section). We will stick to native CSS features though:

{title="src/App.module.css",lang="css"}
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

Then the button CSS classes in the *src/App.module.css* file:

{title="src/App.module.css",lang="css"}
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

.buttonSmall {
  padding: 5px;
}

.buttonLarge {
  padding: 10px;
}
~~~~~~~

There is a shift toward pseudo BEM naming conventions here, in contrast to `button_small` and `button_large` from the previous section. If the previous naming convention holds true, we can only access the style with `styles['button_small']` which makes it more verbose because of  JavaScript's limitation with object underscores. The same shortcomings would apply for classes defined with a dash (`-`). In contrast, now we can use `styles.buttonSmall` instead (see: Item component):

{title="src/App.jsx",lang="javascript"}
~~~~~~~
const SearchForm = ({ ... }) => (
# leanpub-start-insert
  <form onSubmit={onSearchSubmit} className={styles.searchForm}>
# leanpub-end-insert
    <InputWithLabel ... >
      <strong>Search:</strong>
    </InputWithLabel>

    <button
      type="submit"
      disabled={!searchTerm}
# leanpub-start-insert
      className={`${styles.button} ${styles.buttonLarge}`}
# leanpub-end-insert
    >
      Submit
    </button>
  </form>
);
~~~~~~~

The SearchForm component receives the styles as well. It has to use string interpolation for using two styles in one element via JavaScript's template literals. One alternative way is the [clsx](https://bit.ly/3DNEA3R) library, which is installed via the command line as a project dependency:

{title="src/App.jsx",lang="javascript"}
~~~~~~~
import clsx from 'clsx';

...

// somewhere in a className attribute
className={clsx(styles.button, styles.buttonLarge)}
~~~~~~~

The library offers conditional styling too; whereas the left-hand side of the object's property must be used as a [computed property name](https://mzl.la/2XuN651) and is only applied if the right-hand side evaluates to true:

{title="src/App.jsx",lang="javascript"}
~~~~~~~
import clsx from 'clsx';

...

// somewhere in a className attribute
className={clsx(styles.button, { [styles.buttonLarge]: isLarge })}
~~~~~~~

Finally, continue with the InputWithLabel component:

{title="src/App.jsx",lang="javascript"}
~~~~~~~
const InputWithLabel = ({ ... }) => {
  ...

  return (
    <>
# leanpub-start-insert
      <label htmlFor={id} className={styles.label}>
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
        className={styles.input}
# leanpub-end-insert
      />
    </>
  );
};
~~~~~~~

And finish up the remaining style in the *src/App.module.css* file:

{title="src/App.module.css",lang="css"}
~~~~~~~
.searchForm {
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

The same caution as the last section applies: some of these styles like `input` and `label` might be more efficient in a global *src/index.css* file without CSS modules.

Again, CSS Modules -- like any other CSS-in-CSS approach -- can use Sass for more advanced CSS features like nesting. The advantage of CSS modules is that we receive an error in  JavaScript each time a style isn't defined. In the standard CSS approach, unmatched styles in JavaScript and CSS files might go unnoticed.

### Exercises:

* Compare your source code against the author's [source code](https://bit.ly/3xM1HYw).
  * Recap all the [source code changes from this section](https://bit.ly/3dxsZLz).
* Read more about [CSS Modules in Vite](https://bit.ly/3S7peLJ).
* Optional: [Leave feedback for this section](https://forms.gle/iuU7WaeJVwHN2pFCA).