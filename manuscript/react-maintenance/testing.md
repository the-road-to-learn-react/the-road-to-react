## Testing in React

Testing source code is an essential part of programming and should be seen as a mandatory exercise for serious developers. The goal is to verify our source code's quality and functionality before using it in production. The [testing pyramid](https://bit.ly/3BYEra1) will serve as our guide.

The testing pyramid includes end-to-end tests, integration tests, and unit tests. Unit tests are for small, isolated blocks of code, such as a single function or component. Integration tests help us figure out how well these blocks of code work together. An end-to-end test simulates a real-life scenario, like a user logging into a web application. Unit tests are quick and easy to write and maintain; end-to-end tests are the opposite.

Many unit tests are required to cover all the functions and components in a working application, after which several integration tests make sure that the most important units work together. Finally, a few end-to-end tests to simulate critical user scenarios. In this learning experience, we will cover **unit and integration tests**, in addition to a useful component-specific testing technique called **snapshot tests**. **E2E tests** will be part of the exercise.

![](images/testing-pyramid.png)

Choosing a testing library can be a challenge for React beginners, as there are many options. To keep things simple, we'll employ the most popular tools: [Jest](https://jestjs.io) and [React Testing Library](https://testing-library.com) (RTL). Jest is a full-blown testing framework with test runners, test suites, test cases, and assertions. RTL is used for rendering React components, triggering events like mouse clicks, and selecting HTML elements from the DOM to perform assertions. We'll explore both tools step-by-step, from setup to unit testing to integration testing.

### Test Suites, Test Cases, and Assertions

Test suites and test cases are commonly used in JavaScript and many other programming languages. A test suite groups the individual test cases into one larger subject. Let's see how this looks with Jest in our *src/App.test.js* file:

{title="src/App.test.js",lang="javascript"}
~~~~~~~
describe('something truthy and falsy', () => {
  test('true to be true', () => {
    expect(true).toBe(true);
  });

  test('false to be false', () => {
    expect(false).toBe(false);
  });
});
~~~~~~~

The "describe" block is our *test suite*, and the "test" blocks are our *test cases*. Note that test cases can be used without test suites, which may apply to code outside the scope of this lesson:

{title="src/App.test.js",lang="javascript"}
~~~~~~~
test('true to be true', () => {
  expect(true).toBe(true);
});

test('false to be false', () => {
  expect(false).toBe(false);
});
~~~~~~~

Large subjects like functions or components often require multiple test cases, so it makes sense to use them with test suites:

{title="src/App.test.js",lang="javascript"}
~~~~~~~
describe('App component', () => {
  test('removes an item when clicking the Dismiss button', () => {

  });

  test('requests some initial stories from an API', () => {

  });
});
~~~~~~~

Note that a "test" block can also be written as an "it" block. The blocks have the same purpose, except the "it" block may be more familiar to programmers from other programming languages:

{title="src/App.test.js",lang="javascript"}
~~~~~~~
describe('something truthy and falsy', () => {
# leanpub-start-insert
  it('true to be true', () => {
# leanpub-end-insert
    expect(true).toBe(true);
  });

# leanpub-start-insert
  it('false to be false', () => {
# leanpub-end-insert
    expect(false).toBe(false);
  });
});
~~~~~~~

Fortunately, create-react-app comes with Jest, so you don't need to install anything. You can run tests using the test script from your *package.json* in the command line as soon as create-react-app is installed, and then execute your tests with `npm test` to produce the following output:

{title="Command Line",lang="text"}
~~~~~~~
 PASS  src/App.test.js
  something truthy and falsy
    ✓ true to be true (3ms)
    ✓ false to be false (1ms)

Test Suites: 1 passed, 1 total
Tests:       2 passed, 2 total
Snapshots:   0 total
Time:        2.78s, estimated 4s
Ran all test suites related to changed files.

Watch Usage
 › Press a to run all tests.
 › Press f to run only failed tests.
 › Press q to quit watch mode.
 › Press p to filter by a filename regex pattern.
 › Press t to filter by a test name regex pattern.
 › Press Enter to trigger a test run.
~~~~~~~

When we run the test command, the test runner matches all files with a *test.js* suffix. Successful tests are displayed in green, failed tests in red. The interactive test script watches your tests and source code and executes tests when the files change. Jest also provides a few interactive commands, such as pressing "f" to run failed tests and "a" for running all tests. Let's see how this looks for a failed test:

{title="src/App.test.js",lang="javascript"}
~~~~~~~
describe('something truthy and falsy', () => {
  test('true to be true', () => {
    expect(true).toBe(true);
  });

  test('false to be false', () => {
# leanpub-start-insert
    expect(false).toBe(true);
# leanpub-end-insert
  });
});
~~~~~~~

The tests run again, and the command line output shows a failed test in red:

{title="Command Line",lang="text"}
~~~~~~~
 FAIL  src/App.test.js
  something truthy and falsy
    ✓ true to be true (2ms)
    ✕ false to be false (4ms)

  ● something truthy and falsy › false to be false

    expect(received).toBe(expected) // Object.is equality

    Expected: true
    Received: false

       5 |
       6 |   test('false to be false', () => {
    >  7 |     expect(false).toBe(true);
         |                   ^
       8 |   });
       9 | });
      10 |

      at Object.<anonymous> (src/App.test.js:7:19)

Test Suites: 1 failed, 1 total
Tests:       1 failed, 1 passed, 2 total
Snapshots:   0 total
Time:        3.385s
Ran all test suites related to changed files.

Watch Usage: Press w to show more.
~~~~~~~

Familiarize yourself with this test output, because it shows all failed tests, as well as information on why they failed. Using this information, you can fix certain parts of your code until all tests run green. Next, we'll cover *test assertions*, two of which we've already used with Jest's `expect` function. An assertion works by expecting value on the left side (`expect`) to match a value on the right side (`toBe`). `toBe` is only one of many available assertive functions provided by Jest.

{title="src/App.test.js",lang="javascript"}
~~~~~~~
describe('something truthy and falsy', () => {
  test('true to be true', () => {
    expect(true).toBeTruthy();
  });

  test('false to be false', () => {
    expect(false).toBeFalsy();
  });
});
~~~~~~~

Once you start testing, it's a good practice to keep two command line interfaces open: one for watching your tests (`npm test`), and one for developing your application (`npm start`). Also, source control platforms like Git may require an additional command line interface for adding your source code to the repository.

### Exercises:

* Confirm your [source code](https://bit.ly/3C42rII).
  * Confirm the [changes](https://bit.ly/3B2CesG).
* Read more about [Jest](https://jestjs.io).

### Unit Testing: Functions

A unit test is generally used to test components or functions in isolation. For functions, unit tests are for input and output; for components, we test props or the callback handlers communicating to the outside. Before we can perform a unit test on our *src/App.js* file, we must export components and functions like the reducer from our *src/App.js* file with a named export:

{title="src/App.js",lang="javascript"}
~~~~~~~
...

export default App;

# leanpub-start-insert
export { storiesReducer, SearchForm, InputWithLabel, List, Item };
# leanpub-end-insert
~~~~~~~

The exercises at the end of this chapter will cover all the remaining tests you should consider performing. For now, we can import all the components and reducers in our *src/App.test.js* file and we will focus on the reducer test first. We are also importing React here, because we have to include it whenever we test React components:

{title="src/App.test.js",lang="javascript"}
~~~~~~~
import * as React from 'react';

import App, {
  storiesReducer,
  Item,
  List,
  SearchForm,
  InputWithLabel,
} from './App';
~~~~~~~

Before we unit test our first React component, we'll cover how to test just a JavaScript function. The best candidate for this test use case is the `storiesReducer` function and one of its actions. Let's define some test data and the test suite for the reducer test:

{title="src/App.test.js",lang="javascript"}
~~~~~~~
import * as React from 'react';

...

# leanpub-start-insert
const storyOne = {
  title: 'React',
  url: 'https://reactjs.org/',
  author: 'Jordan Walke',
  num_comments: 3,
  points: 4,
  objectID: 0,
};

const storyTwo = {
  title: 'Redux',
  url: 'https://redux.js.org/',
  author: 'Dan Abramov, Andrew Clark',
  num_comments: 2,
  points: 5,
  objectID: 1,
};

const stories = [storyOne, storyTwo];

describe('storiesReducer', () => {
  test('removes a story from all stories', () => {

  });
});
# leanpub-end-insert
~~~~~~~

If you extrapolate the test cases, there should be one test case per reducer action. We will focus on a single action, which you can use to perform the rest as exercise yourself. The reducer function accepts a state and an action, and then returns a new state, so reducer tests follow the same pattern:

{title="src/App.test.js",lang="javascript"}
~~~~~~~
...

describe('storiesReducer', () => {
  test('removes a story from all stories', () => {
# leanpub-start-insert
    const action = // TODO: some action
    const state = // TODO: some current state

    const newState = storiesReducer(state, action);

    const expectedState = // TODO: the expected state

    expect(newState).toBe(expectedState);
# leanpub-end-insert
  });
});
~~~~~~~

For our case, we define action, state, and expected state according to our reducer. The expected state will have one less story, which was removed as it passed to the reducer as action:

{title="src/App.test.js",lang="javascript"}
~~~~~~~
describe('storiesReducer', () => {
  test('removes a story from all stories', () => {
# leanpub-start-insert
    const action = { type: 'REMOVE_STORY', payload: storyOne };
    const state = { data: stories, isLoading: false, isError: false };
# leanpub-end-insert

    const newState = storiesReducer(state, action);

# leanpub-start-insert
    const expectedState = {
      data: [storyTwo],
      isLoading: false,
      isError: false,
    };
# leanpub-end-insert

    expect(newState).toBe(expectedState);
  });
});
~~~~~~~

This test still fails because we are using `toBe` instead of `toStrictEqual`. The `toBe` assertive function makes a strict comparison like `newState === expectedState`. The content of the objects are the same, however, their object references are not the same. We use `toStrictEqual` instead of `toBe` to limit our comparison to the object's content:

{title="src/App.test.js",lang="javascript"}
~~~~~~~
describe('storiesReducer', () => {
  test('removes a story from all stories', () => {
    const action = { type: 'REMOVE_STORY', payload: storyOne };
    const state = { data: stories, isLoading: false, isError: false };

    const newState = storiesReducer(state, action);

    const expectedState = {
      data: [storyTwo],
      isLoading: false,
      isError: false,
    };

# leanpub-start-insert
    expect(newState).toStrictEqual(expectedState);
# leanpub-end-insert
  });
});
~~~~~~~

There is always the decision to make for JavaScript objects whether you want to make a strict comparison or just a content comparison. Most often you only want to have a content comparison here, hence use `toStrictEqual`. For JavaScript primitives though, like strings or booleans, you can still use `toBe`. Also note that there is a `toEqual` function [which works slightly different](https://bit.ly/3jlPpii) than `toStrictEqual`.

We continue to make adjustments until the reducer test turns green, which is really testing a JavaScript function with a certain input and expecting a certain output. We haven't done any testing methods regarding React yet.

Remember, a reducer function will always follow the same test pattern: given a state and action, we expect the following new state. Every action of the reducer could be another test case in our reducer's test suite, so consider using the exercises as a way to move through your entire source code.

### Exercises:

* Confirm your [source code](https://bit.ly/3jjIhD3).
  * Confirm the [changes](https://bit.ly/3E175aO).
* Continue to write a test case for every reducer action and its state transition.
* Read more about Jest's assertive functions like `toBe` and `toStrictEqual`.

### Unit Testing: Components

We tested our first function in JavaScript with Jest in the previous sections. Next, we'll test our first isolated React component with a unit test. Since we are using create-react-app, we don't need to set up the React Testing Library (RTL), which is necessary for our component tests because it is the default testing library. If you are using a custom React setup like React with Webpack, RTL as a library would need to be installed.

The following functions from React Testing Library are used for component tests:

{title="src/App.test.js",lang="javascript"}
~~~~~~~
import * as React from 'react';
# leanpub-start-insert
import {
  render,
  screen,
  fireEvent,
  act,
} from '@testing-library/react';
# leanpub-start-insert

...
~~~~~~~

Start with the Item component, where we assert whether it renders all expected properties based on its given props. Based on the input (props), we are asserting an output (rendered HTML). We'll use RTL's `render` function in each test to render a React component. In this case, we render the Item component as an element and pass it an `item` object -- one of our previously defined stories -- as props:

{title="src/App.test.js",lang="javascript"}
~~~~~~~
# leanpub-start-insert
describe('Item', () => {
  test('renders all properties', () => {
    render(<Item item={storyOne} />);
  });
});
# leanpub-end-insert
~~~~~~~

After rendering it, we can use the `debug` function from RTL's `screen` object:

{title="src/App.test.js",lang="javascript"}
~~~~~~~
describe('Item', () => {
  test('renders all properties', () => {
    render(<Item item={storyOne} />);

# leanpub-start-insert
    screen.debug();
# leanpub-end-insert
  });
});
~~~~~~~

Run the tests with `npm test`, and you'll see the output from the `debug` function. It prints all your component's and child component's HTML elements. The output should be similar to the following:

{title="src/App.test.js",lang="javascript"}
~~~~~~~
<body>
  <div>
    <li>
      <span>
        <a
          href="https://reactjs.org/"
        >
          React
        </a>
      </span>
      <span>
        Jordan Walke
      </span>
      <span>
        3
      </span>
      <span>
        4
      </span>
      <span>
        <button
          type="button"
        >
          Dismiss
        </button>
      </span>
    </li>
  </div>
</body>
~~~~~~~

Here you should form the habit of using RTL's `debug` function whenever you render a new component in a React component test. The function gives a useful overview of what is rendered and informs the best way to proceed with testing. Based on the current output, we can start with our first assertion. RTL's `screen` object provides a function called `getByText`, one of many search functions:

{title="src/App.test.js",lang="javascript"}
~~~~~~~
describe('Item', () => {
  test('renders all properties', () => {
    render(<Item item={storyOne} />);

# leanpub-start-insert
    expect(screen.getByText('Jordan Walke')).toBeInTheDocument();
    expect(screen.getByText('React')).toHaveAttribute(
      'href',
      'https://reactjs.org/'
    );
# leanpub-end-insert
  });
});
~~~~~~~

For the two assertions, we use the two assertive functions `toBeInTheDocument` and `toHaveAttribute`. These are to verify an element with the text "Jordan Walke" is in the document, and the presence of an element with the text "React" with a specific `href` attribute value. Over time, you will see more of these assertive functions being used.

RTL's `getByText` search function finds the one element with the visible texts "Jordan Walke" and "React". We can use the `getAllByText` equivalent to find more than one element. Similar equivalents exist for other search functions.

The `getByText` function returns the element with a text that users can see, which relates to the real-world use of the application. Note that `getByText` is not the only search function, though. Another highly-used search function is the `getByRole` or `getAllByRole` function:

{title="src/App.test.js",lang="javascript"}
~~~~~~~
describe('Item', () => {
  test('renders all properties', () => {
    ...
  });

# leanpub-start-insert
  test('renders a clickable dismiss button', () => {
    render(<Item item={storyOne} />);

    expect(screen.getByRole('button')).toBeInTheDocument();
  });
# leanpub-end-insert
});
~~~~~~~

The `getByRole` function is usually used to retrieve elements by [aria-label attributes](https://mzl.la/3B3bBDP). However, there are also [implicit roles on HTML elements](https://mzl.la/3n7SgN7) -- like button for a button element. Thus you can select elements not only by visible text, but also by their (implicit) accessibility role with React Testing Library. A neat feature of `getRoleBy` is that [it suggests roles if you provide a role that's not available](https://bit.ly/3pnPXrQ). Both, `getByText` and `getByRole` are RTL's most widely used search functions.

We can continue here by asserting not only that everything is *in the document*, but also by asserting whether our events work as expected. For example, the Item component's button element can be clicked and we want to verify that the callback handler gets called. Therefore, we are using Jest for creating a mocked function which we provide as a callback handler to the Item component. Then, after firing a click event with React Testing Library on the button, we want to assert that the callback handler function has been called:

{title="src/App.test.js",lang="javascript"}
~~~~~~~
describe('Item', () => {
  test('renders all properties', () => {
    ...
  });

  test('renders a clickable dismiss button', () => {
    ...
  });

# leanpub-start-insert
  test('clicking the dismiss button calls the callback handler', () => {
    const handleRemoveItem = jest.fn();

    render(<Item item={storyOne} onRemoveItem={handleRemoveItem} />);

    fireEvent.click(screen.getByRole('button'));

    expect(handleRemoveItem).toHaveBeenCalledTimes(1);
  });
# leanpub-end-insert
});
~~~~~~~

Jest lets us pass a test-specific function to the Item component as a prop. These test-specific functions are called **spy**, **stub**, or **mock**; each is used for different test scenarios. The `jest.fn()` returns us a *mock* for the actual function, which lets us capture when it's called. As a result, we can use Jest assertions like `toHaveBeenCalledTimes`, which lets us assert a number of times the function has been called; and `toHaveBeenCalledWith`, to verify arguments that are passed to it.

Every time we want to spy a JavaScript function, whether it has been called or whether it received certain arguments, we can use Jest's helper function to create a mocked function. Then, after invoking this function implicitly with RTL's `fireEvent` object's function, we can assert that the provided callback handler -- which is the mocked function -- has been called one time.

In the last exercise we tested the Item component's input and output via rendering assertions and callback handler assertions. We are not testing real state changes yet, however, as there is no actual item removed from the DOM after clicking the "Dismiss"-button. The logic to remove the item from the list is in the App component, but we are only testing the Item component in isolation. Sometimes it's just useful to test whether a single block works, before testing everything all together. We will test the actual implementation logic for removing an Item when we cover the App component later.

For now, the SearchForm component will use the InputWithLabel component as a child component. As before, we will start by rendering the component and providing all the essential props:

{title="src/App.test.js",lang="javascript"}
~~~~~~~
# leanpub-start-insert
describe('SearchForm', () => {
  const searchFormProps = {
    searchTerm: 'React',
    onSearchInput: jest.fn(),
    onSearchSubmit: jest.fn(),
  };

  test('renders the input field with its value', () => {
    render(<SearchForm {...searchFormProps} />);

    screen.debug();
  });
});
# leanpub-end-insert
~~~~~~~

Again, we start with the debugging. After evaluating what renders, we can make the first assertion for the SearchForm. With input fields, the `getByDisplayValue` search function is the perfect candidate to return the input field as an element:

{title="src/App.test.js",lang="javascript"}
~~~~~~~
describe('SearchForm', () => {
  const searchFormProps = { ... };

  test('renders the input field with its value', () => {
    render(<SearchForm {...searchFormProps} />);

# leanpub-start-insert
    expect(screen.getByDisplayValue('React')).toBeInTheDocument();
# leanpub-end-insert
  });
});
~~~~~~~

Since the input element is rendered with a default value, we can use the default value (here: "React"), which is the displayed value in our test assertion. If the input element doesn't have a default value, the application could show a placeholder with the `placeholder` HTML attribute on the input field. Then we'd use another function from RTL called `getByPlaceholderText`, which is used for searching an element with a placeholder text.

Because the debug information presented multiple options to query the HTML, we could continue with one more test to assert the rendered label:

{title="src/App.test.js",lang="javascript"}
~~~~~~~
describe('SearchForm', () => {
  const searchFormProps = { ... };

  test('renders the input field with its value', () => {
    ...
  });

# leanpub-start-insert
  test('renders the correct label', () => {
    render(<SearchForm {...searchFormProps} />);

    expect(screen.getByLabelText(/Search/)).toBeInTheDocument();
  });
# leanpub-end-insert
});
~~~~~~~

The `getByLabelText` search function allows us to find an element by a label in a form. This is useful for components that render multiple labels and HTML controls. However, you may have noticed we used a [regular expression](https://mzl.la/3CdDjiZ) here. If we used a string instead, the colon for "Search:" must be included. By using a regular expression, we are matching strings that include the "Search" string, which makes finding elements much more efficient. For this reason, you may find yourself using regular expressions instead of strings quite often.

Anyway, perhaps it would be more interesting to test the interactive parts of the SearchForm component. Since our callback handlers, which are passed as props to the SearchForm component, are already mocked with Jest, we can assert whether these functions get called appropriately:

{title="src/App.test.js",lang="javascript"}
~~~~~~~
describe('SearchForm', () => {
  const searchFormProps = {
    searchTerm: 'React',
    onSearchInput: jest.fn(),
    onSearchSubmit: jest.fn(),
  };

  ...

# leanpub-start-insert
  test('calls onSearchInput on input field change', () => {
    render(<SearchForm {...searchFormProps} />);

    fireEvent.change(screen.getByDisplayValue('React'), {
      target: { value: 'Redux' },
    });

    expect(searchFormProps.onSearchInput).toHaveBeenCalledTimes(1);
  });

  test('calls onSearchSubmit on button submit click', () => {
    render(<SearchForm {...searchFormProps} />);

    fireEvent.submit(screen.getByRole('button'));

    expect(searchFormProps.onSearchSubmit).toHaveBeenCalledTimes(1);
  });
# leanpub-end-insert
});
~~~~~~~

Similar to the Item component, we tested input (props) and output (callback handler) for the SearchForm component. The difference is that the SearchForm component renders a child component called `InputWithLabel`. If you check the debug output, you'll likely notice that the React Testing Library doesn't bother with this child component. This happens because the end-user wouldn't care about the component either, so the React Testing Library outputs all the HTML that matters for the test.

All the callback handler tests for Item and SearchForm component test only whether the functions have been called. No React re-rendering occurs, because all the components are tested in isolation without state management, which solely happens in the App component. Real testing with RTL starts further up the component tree, where state changes and side-effects can be evaluated. Therefore, let me introduce integration testing next.

### Exercises:

* Confirm your [source code](https://bit.ly/2Z3Vc54).
  * Confirm the [changes](https://bit.ly/3E28IoZ).
* Read more about [React Testing Library](https://bit.ly/30KueQH).
  * Read more about [search functions](https://bit.ly/3jjUw2t).
* Add tests for your List and InputWithLabel components.

### Integration Testing: Component

React Testing Library adheres to a single core philosophy: instead of testing implementation details of React components, it tests how users interact with the application and if it works as expected. This becomes especially powerful for integration tests.

We'll need to provide some data before we test the App component, since it makes requests for data from the remote API after its initial render. We'll start by using axios in the App component for our data request, after which we'll mock it with Jest at the top of the testing file:

{title="src/App.test.js",lang="javascript"}
~~~~~~~
...

# leanpub-start-insert
import axios from 'axios';
# leanpub-end-insert

...

# leanpub-start-insert
jest.mock('axios');
# leanpub-end-insert

...
~~~~~~~

Next, implement the data you want to be returned from the mocked API request with a JavaScript Promise, and use it for the axios mock. Afterward, we can render our component and assume the correct data is mocked for our API request:

{title="src/App.test.js",lang="javascript"}
~~~~~~~
# leanpub-start-insert
describe('App', () => {
  test('succeeds fetching data', () => {
    const promise = Promise.resolve({
      data: {
        hits: stories,
      },
    });

    axios.get.mockImplementationOnce(() => promise);

    render(<App />);

    screen.debug();
  });
});
# leanpub-end-insert
~~~~~~~

Now we'll use React Testing Library's `act` helper function to wait until the promise resolves after the component's initial render. With async/await, we can implement this like synchronous code. The `debug` function from RTL is useful because it outputs the App component's elements before and after the request:

{title="src/App.test.js",lang="javascript"}
~~~~~~~
describe('App', () => {
# leanpub-start-insert
  test('succeeds fetching data', async () => {
# leanpub-end-insert
    const promise = Promise.resolve({
      data: {
        hits: stories,
      },
    });

    axios.get.mockImplementationOnce(() => promise);

    render(<App />);

    screen.debug();

# leanpub-start-insert
    await act(() => promise);

    screen.debug();
# leanpub-end-insert
  });
});
~~~~~~~

In the debug's output, we see the loading indicator renders for the first debug function, but not the second. This is because the data fetching and component re-render complete after we resolve the promise in our test with `act`. Let's assert the loading indicator for this case:

{title="src/App.test.js",lang="javascript"}
~~~~~~~
describe('App', () => {
  test('succeeds fetching data', async () => {
    const promise = Promise.resolve({
      data: {
        hits: stories,
      },
    });

    axios.get.mockImplementationOnce(() => promise);

    render(<App />);

# leanpub-start-insert
    expect(screen.queryByText(/Loading/)).toBeInTheDocument();
# leanpub-end-insert

    await act(() => promise);

# leanpub-start-insert
    expect(screen.queryByText(/Loading/)).toBeNull();
# leanpub-end-insert
  });
});
~~~~~~~

Because we're testing for a returned element that is absent, this time we use RTL's `queryByText` instead of the `getByText` function. Using `getByText` in this instance would produce an error, because the element can't be found; but with `queryByText` the value just returns `null`.

Again, we're using a regular expression `/Loading/` instead of a string `'Loading'`. To use a string, we'd have to explicitly use `'Loading ...'` instead of `'Loading'`. With a regular expression, we don't need to provide the whole string, we just need to match a part of it.

Next, we can assert whether or not our fetched data gets rendered as expected:

{title="src/App.test.js",lang="javascript"}
~~~~~~~
describe('App', () => {
  test('succeeds fetching data', async () => {
    const promise = Promise.resolve({
      data: {
        hits: stories,
      },
    });

    axios.get.mockImplementationOnce(() => promise);

    render(<App />);

    expect(screen.queryByText(/Loading/)).toBeInTheDocument();

    await act(() => promise);

    expect(screen.queryByText(/Loading/)).toBeNull();

# leanpub-start-insert
    expect(screen.getByText('React')).toBeInTheDocument();
    expect(screen.getByText('Redux')).toBeInTheDocument();
    expect(screen.getAllByText('Dismiss').length).toBe(2);
# leanpub-end-insert
  });
});
~~~~~~~

The [happy path](https://bit.ly/3jiAbuB) for the data fetching is tested now. Similarly, we can test the unhappy path in case of a failed API request. The promise needs to reject and the error should be caught with a try/catch block:

{title="src/App.test.js",lang="javascript"}
~~~~~~~
describe('App', () => {
  test('succeeds fetching data', async () => {
    ...
  });

# leanpub-start-insert
  test('fails fetching data', async () => {
    const promise = Promise.reject();

    axios.get.mockImplementationOnce(() => promise);

    render(<App />);

    expect(screen.getByText(/Loading/)).toBeInTheDocument();

    try {
      await act(() => promise);
    } catch (error) {
      expect(screen.queryByText(/Loading/)).toBeNull();
      expect(screen.queryByText(/went wrong/)).toBeInTheDocument();
    }
  });
# leanpub-end-insert
});
~~~~~~~

There may be some confusion about when to use `getBy` or the `queryBy` search variants. As a rule of thumb, use `getBy` for single elements, and `getAllBy` for multiple elements. If you are checking for elements that aren't present, use `queryBy` (or `queryAllBy`). In this code, I preferred using `queryBy` for the sake of alignment and readability.

Now we know the initial data fetching works for our App component, so we can move to testing user interactions. We have only tested user actions in the child components thus far, by firing events without any state and side-effect. Next, we'll remove an item from the list after the data has been fetched successfully. Since the item with "Jordan Walke" is the first rendered item in the list, it gets removed if we click the first "Dismiss"-button:

{title="src/App.test.js",lang="javascript"}
~~~~~~~
describe('App', () => {

  ...

# leanpub-start-insert
  test('removes a story', async () => {
    const promise = Promise.resolve({
      data: {
        hits: stories,
      },
    });

    axios.get.mockImplementationOnce(() => promise);

    render(<App />);

    await act(() => promise);

    expect(screen.getAllByText('Dismiss').length).toBe(2);
    expect(screen.getByText('Jordan Walke')).toBeInTheDocument();

    fireEvent.click(screen.getAllByText('Dismiss')[0]);

    expect(screen.getAllByText('Dismiss').length).toBe(1);
    expect(screen.queryByText('Jordan Walke')).toBeNull();
  });
# leanpub-end-insert
});
~~~~~~~

To test the search feature, we set up the mocking differently, because we're handling initial request, plus another request once the user searches for more stories by a specific search term:

{title="src/App.test.js",lang="javascript"}
~~~~~~~
describe('App', () => {

  ...

# leanpub-start-insert
  test('searches for specific stories', async () => {
    const reactPromise = Promise.resolve({
      data: {
        hits: stories,
      },
    });

    const anotherStory = {
      title: 'JavaScript',
      url: 'https://en.wikipedia.org/wiki/JavaScript',
      author: 'Brendan Eich',
      num_comments: 15,
      points: 10,
      objectID: 3,
    };

    const javascriptPromise = Promise.resolve({
      data: {
        hits: [anotherStory],
      },
    });

    axios.get.mockImplementation((url) => {
      if (url.includes('React')) {
        return reactPromise;
      }

      if (url.includes('JavaScript')) {
        return javascriptPromise;
      }

      throw Error();
    });
  });
# leanpub-end-insert
});
~~~~~~~

Instead of mocking the request once with Jest (`mockImplementationOnce`), now we mock multiple requests (`mockImplementation`). Depending on the incoming URL, the request either returns the initial list ("React"-related stories), or the new list ("JavaScript"-related stories). If we provide an incorrect URL to the request, the test throws an error for confirmation. As before, let's render the App component:

{title="src/App.test.js",lang="javascript"}
~~~~~~~
describe('App', () => {

  ...

  test('searches for specific stories', async () => {
    const reactPromise = Promise.resolve({ ... });

    const anotherStory = { ... };

    const javascriptPromise = Promise.resolve({ ... });

    axios.get.mockImplementation((url) => {
      ...
    });

# leanpub-start-insert
    // Initial Render

    render(<App />);

    // First Data Fetching

    await act(() => reactPromise);

    expect(screen.queryByDisplayValue('React')).toBeInTheDocument();
    expect(screen.queryByDisplayValue('JavaScript')).toBeNull();

    expect(screen.queryByText('Jordan Walke')).toBeInTheDocument();
    expect(
      screen.queryByText('Dan Abramov, Andrew Clark')
    ).toBeInTheDocument();
    expect(screen.queryByText('Brendan Eich')).toBeNull();
# leanpub-end-insert
  });
});
~~~~~~~

We are resolving the first promise for the initial render. We expect the input field to render "React", and the two items in the list to render the creators of React and Redux. We also make sure that no stories related to JavaScript are rendered yet. Next, change the input field's value by firing an event, and asserting that the new value is rendered from the App component through all its child components in the actual input field:

{title="src/App.test.js",lang="javascript"}
~~~~~~~
describe('App', () => {

  ...

  test('searches for specific stories', async () => {

    ...

    expect(screen.queryByText('Jordan Walke')).toBeInTheDocument();
    expect(
      screen.queryByText('Dan Abramov, Andrew Clark')
    ).toBeInTheDocument();
    expect(screen.queryByText('Brendan Eich')).toBeNull();

# leanpub-start-insert
    // User Interaction -> Search

    fireEvent.change(screen.queryByDisplayValue('React'), {
      target: {
        value: 'JavaScript',
      },
    });

    expect(screen.queryByDisplayValue('React')).toBeNull();
    expect(
      screen.queryByDisplayValue('JavaScript')
    ).toBeInTheDocument();
# leanpub-end-insert
  });
});
~~~~~~~

Lastly, we can submit this search request by firing a submit event with the button. The new search term is used from the App component's state, so the new URL searches for JavaScript-related stories that we have mocked before:

{title="src/App.test.js",lang="javascript"}
~~~~~~~
describe('App', () => {

  ...

  test('searches for specific stories', async () => {

    ...

    expect(screen.queryByDisplayValue('React')).toBeNull();
    expect(
      screen.queryByDisplayValue('JavaScript')
    ).toBeInTheDocument();

# leanpub-start-insert
    fireEvent.submit(screen.queryByText('Submit'));

    // Second Data Fetching

    await act(() => javascriptPromise);

    expect(screen.queryByText('Jordan Walke')).toBeNull();
    expect(
      screen.queryByText('Dan Abramov, Andrew Clark')
    ).toBeNull();
    expect(screen.queryByText('Brendan Eich')).toBeInTheDocument();
# leanpub-end-insert
  });
});
~~~~~~~

Brendan Eich is rendered as the creator of JavaScript, while the creators of React and Redux are removed. This test depicts an entire test scenario in one test case. We can move through each step -- initial fetching, changing the input field value, submitting the form, and retrieving new data from the API -- with the tools we've used.

React Testing Library with Jest is the most popular library combination for React testing. RTL provides relevant testing tools, while Jest has a general testing framework for test suites, test cases, assertions, and mocking capabilities. If you need an alternative to RTL, consider trying  [Enzyme](https://www.robinwieruch.de/react-testing-jest-enzyme/) by Airbnb.

### Exercises:

* Confirm your [source code](https://bit.ly/3DOSIqe).
  * Confirm the [changes](https://bit.ly/3jf3pKM).
* Read more about [React Testing Library in React](https://www.robinwieruch.de/react-testing-library/).
* Read more about [E2E tests in React](https://www.robinwieruch.de/react-testing-cypress/).
* While you continue with the upcoming sections, keep your tests green and add new tests when needed.

### Snapshot Testing

Facebook created snapshot tests as a more lightweight way to test React components and their structure. A snapshot test creates an instance of your rendered component's output as HTML elements and their structure. This snapshot is compared to the same snapshot in the next test to give more output on how the rendered component changed and show why any tests failed in the difference. You can accept or deny any differences in your source code until the component functions as intended.

Snapshot tests are lightweight, with less focus on the implementation details of the component. Let's perform a snapshot test for our SearchForm component:

{title="src/App.test.js",lang="javascript"}
~~~~~~~
describe('SearchForm', () => {

  ...

# leanpub-start-insert
  test('renders snapshot', () => {
    const { container } = render(<SearchForm {...searchFormProps} />);
    expect(container.firstChild).toMatchSnapshot();
  });
# leanpub-end-insert
});
~~~~~~~

Run these tests with `npm test` and you'll see a new *src/_snapshots_* folder has been created in your project folder. Similar to RTL's `debug` function, there's a snapshot of your rendered SearchForm component as an HTML element structure in the file. Next, head to *src/App.js* file and change the HTML. For example, try removing the bold text from the SearchForm component:

{title="src/App.js",lang="javascript"}
~~~~~~~
const SearchForm = ({
  searchTerm,
  onSearchInput,
  onSearchSubmit,
}) => (
  <form onSubmit={onSearchSubmit}>
    <InputWithLabel
      id="search"
      value={searchTerm}
      isFocused
      onInputChange={onSearchInput}
    >
# leanpub-start-insert
      Search:
# leanpub-end-insert
    </InputWithLabel>

    <button type="submit" disabled={!searchTerm}>
      Submit
    </button>
  </form>
);
~~~~~~~

After the next test, the command line should look similar to the following:

{title="Command Line",lang="text"}
~~~~~~~
- Snapshot
+ Received

    <label
      for="search"
    >
-     <strong>
-       Search:
-     </strong>
+     Search:
    </label>
~~~~~~~

This is a typical case for a breaking snapshot test. When a component's HTML structure is changed unintentionally, the snapshot test informs us in the command line. To fix it, we'd go into the *src/App.js* file and edit the SearchForm component. For intentional changes, press "u" on the command line for interactive tests and a new snapshot will be created. Try it and see how the snapshot file in your *src/_snapshots_* folder changes.

Jest stores snapshots in a folder so it can validate the difference against future snapshot tests. Users can share these snapshots across teams using version control platforms like git. This is how we make sure the DOM stays the same.

Snapshot tests are useful for setting up tests quickly in React, though it's best to avoid using them exclusively. Instead, use snapshot tests for components that don't update often, are less complex, and where it's easier to compare component results.

### Exercises:

* Confirm your [source code](https://bit.ly/3G7tF3M).
  * Confirm the [changes](https://bit.ly/3lUYpge).
* Add one snapshot test for each of all the other components and check the content of your *_snapshots_* folder.
* Optional: [Leave feedback for this section](https://forms.gle/tMJyXvxS1AmRvSUU9).