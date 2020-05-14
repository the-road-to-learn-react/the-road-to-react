## Advanced Testing in React

Testing source code is essential to programming, and should be seen as a mandatory exercise for serious developers. We want to verify our source code's quality and functionality before using it in production. The [testing pyramid](https://martinfowler.com/articles/practical-test-pyramid.html) will serve as our guide.

The testing pyramid includes end-to-end tests, integration tests, and unit tests. Unit tests are used for small, isolated blocks of code, such as a single function or component. Integration tests help us figure out if these units work well together. An end-to-end test simulates a real-life scenario, such as the login of a user in a web application. Unit tests are quick and easy to write and maintain; end-to-end tests are the opposite.

We want to have many unit tests covering our functions and components. After that, we can use several integration tests to make sure the most important functions and components work together as expected. Finally, we may need a few end-to-end tests to simulate critical scenarios. In this learning experience, we will cover **unit and integration tests**, along with a component specific testing technique called **snapshot tests**. **E2E tests** will be part of the exercise.

Since there are many testing libraries, it can be challenging to choose one as a beginner to React. We will use the most popular testing tools for React called [Jest](https://jestjs.io/) and [React Testing Library](https://testing-library.com/) (RTL). While Jest is a full-blown testing framework with test runner, test suites, test cases, and assertions, RTL is used for rendering React components, triggering events like mouse clicks, and selecting HTML elements from the DOM for performing assertions. In the next parts, we will explore both tools step by step from setup over unit testing to integration testing.

### Test Suites, Test Cases, and Assertions

Jest offers you everything that's needed from a testing framework. In JavaScript, but also many other programming languages, it's common to use test suites and test cases. Whereas a test suite groups many test cases into one larger subject, a test case usually resembles only one test scenario. Let's see how this looks like with Jest in our *src/App.test.js* file:

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

While the "describe"-block is our *test suite*, the "test"-blocks are our *test cases*. It should be noted that test cases can be used without a test suite, which isn't important at this stage, but should be known information:

{title="src/App.test.js",lang="javascript"}
~~~~~~~
test('true to be true', () => {
  expect(true).toBe(true);
});

test('false to be false', () => {
  expect(false).toBe(false);
});
~~~~~~~

But it happens quite often that one larger subject (e.g. a function or a component) -- which would resemble a test suite -- ends up with multiple test cases. Hence it makes sense to use test suites and test cases altogether:

{title="src/App.test.js",lang="javascript"}
~~~~~~~
describe('App component', () => {
  test('removes an item when clicking the Dismiss button', () => {

  });

  test('requests some initial stories from an API', () => {

  });
});
~~~~~~~

It should also be noted that a "test"-block can also be written as a "it"-block. Both are the same regarding their functionality; the only difference is that the "it"-block is has been used historically for quite a long time in JavaScript (and other programming languages). The "test"-block is newer and its intention is to give the whole thing a better name. You can use whatever suits you best:

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

Fortunately, create-react-app already comes with Jest, so you don't need to install anything. You can run the tests using the test script from your *package.json* on the command line. Execute your tests with `npm test` and you should be presented the following output:

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

Jest matches all files with a *test.js* suffix in its filename when its command is run. Successful tests are displayed in green; failed tests are displayed in red. The interactive test script watches your tests and source code and thus will execute your tests every time when something in these watched files has been changed. It also offers you a few interactive commands like pressing "f" for running only failed tests and "a" for running all tests again. Let's see how this looks for a failed test:

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

Your tests should run again and your command line output should show a failing test in red:

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

Get used to how this test output looks like, because it gives you all the information to spot the failed test and why it failed. You can fix your test again and see that everything turns green again.

What I didn't mention yet are test assertions. You have already used two test assertions with Jest's `expect` function. This function is automatically available in your Jest test files. An assertion works the following way: Expect something on the left hand-side (`expect`) to match something on the right hand-side (`toBe`). You will soon see that `toBe` is only one of many assertive functions that are available with Jest:

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

Once you start testing, it's a good practice to keep two command line interfaces open: one for watching your tests (`npm start`) and one for developing your application (`npm start`). If you are using source control like Git, you may end up with one more open command line interface to add your source code into a repository at certain stages.

### Exercises:

* Confirm your [source code for the last section](https://codesandbox.io/s/github/the-road-to-learn-react/hacker-stories/tree/hs/react-testing-setup).
  * Confirm the [changes from the last section](https://github.com/the-road-to-learn-react/hacker-stories/compare/hs/react-modern-final...hs/react-testing-setup?expand=1).
* Read more about [Jest](https://jestjs.io/).

### Unit Testing: Functions

We will start with the possibly smallest block of a test: a unit test. A unit tests usually tests a component or a function in isolation. For a function, this would mean we would only test a functions input and output. For a component, this would mean we would only test a component's props and maybe callback handlers which are communicating to the outside.

Before we can test anything from our *src/App.js* file, we have to export components and functions (e.g. reducer function) from our *src/App.js* file with a named export:

{title="src/App.js",lang="javascript"}
~~~~~~~
...

export default App;

# leanpub-start-insert
export { storiesReducer, SearchForm, InputWithLabel, List, Item };
# leanpub-end-insert
~~~~~~~

We are not going to test all of them in this section, however, it's up to you as exercise at the end of this section to test everything that we didn't touch here. Now, we can import the all the components and reducers in our *src/App.test.js* file. We are also importing React here, because we have to include it whenever we test React components.

{title="src/App.test.js",lang="javascript"}
~~~~~~~
import React from 'react';

import App, {
  storiesReducer,
  Item,
  List,
  SearchForm,
  InputWithLabel,
} from './App';
~~~~~~~

Before we start to unit test our first React component, I want to show you how to test just a JavaScript function first. The best candidate for this test use case would be the `storiesReducer` function and one of its actions. Therefore, we will define some test data -- which we will use for later tests too -- and the test suite for the reducer test:

{title="src/App.test.js",lang="javascript"}
~~~~~~~
import React from 'react';

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

If you extrapolate the test cases, you may end up for one test case per reducer action. We will just focus on one action though, the rest is up as exercise for you. In general, the reducer function accepts a state and an action and returns a new state, which means essentially every reducer test follows the same pattern:

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

For our particular case, we define action, state and expected state accordingly to our specific reducer. In the end, the expected state should have one less story, because the story which is passed to the reducer as action should be removed:

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

The test still fails though. That's because we are using `toBe` instead of `toStrictEqual` here. The `toBe` assertive function makes a strict comparison like `newState === expectedState`. However, the object reference is not the same here, it's just the content of the object. Hence we have to use `toStrictEqual` rather than `toBe` to just compare the object's content:

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

There is always the decision to make for JavaScript objects whether you want to make a strict comparison or just content comparison. Most often you only want to have a content comparison here, hence use `toStrictEqual`. For JavaScript primitives though, like strings or booleans, you can still use `toBe`. Also note that there is a `toEqual` function [which works slightly different](https://twitter.com/rwieruch/status/1260866850080067584) than `toStrictEqual`.

Anyway, your first reducer test should turn green. In the end, it's just testing a JavaScript function with given inputs and expecting an output. There is nothing related to React here yet. Also, as mentioned earlier, a reducer function always follows the same test pattern: given a state and action, we expect the following new state. Every action of the reducer could be another test case in our reducer's test suite. So testing all the remaining actions is up to you as exercise.

### Exercises:

* Confirm your [source code for the last section](https://codesandbox.io/s/github/the-road-to-learn-react/hacker-stories/tree/hs/react-testing-unit-function).
  * Confirm the [changes from the last section](https://github.com/the-road-to-learn-react/hacker-stories/compare/hs/react-testing-setup...hs/react-testing-unit-function?expand=1).
* Read more about Jest's assertive functions like `toBe` and `toStrictEqual`.

### Unit Testing: Components

After we have tested our first function in JavaScript with Jest, we will continue with testing our first React component in isolation with a unit test. Since we are using create-react-app, you don't need to set up anything for React Testing Library (RTL), which is needed for our component tests, because it comes as default testing library. If you are using a custom React setup (e.g. React with Webpack), you would need to install the library yourself.

Now, the following functions from React Testing Library will be used for the component tests. We will use and explain them step by step when we start calling them in our component tests:

{title="src/App.test.js",lang="javascript"}
~~~~~~~
import React from 'react';
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

We are going to start with the Item component where we just want to assert whether it renders all the expected properties based on its given props. In other words, based on the input (props), we are asserting the output (rendered HTML):

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

Basically we will use RTL's `render` function in every test to render a React component. In this case, we render the Item component as element and pass it an `item` object -- one of our earlier defined stories -- as props. After rendering it, we can use the `debug` function from RTL's `screen` object:

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

Once you run your tests with `npm test`, you should be able to see the output that the `debug` function is giving you. Essentially it prints all your component's (and child component's) HTML elements. In your case, the output should be similar to the following:

{title="src/App.test.js",lang="javascript"}
~~~~~~~
<body>
  <div>
    <div>
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
    </div>
  </div>
</body>
~~~~~~~

Here you could start to develop a habit of using RTL's `debug` function whenever you render a new component in a React component test. It gives you the perfect overview of what's rendered and a good idea about how to proceed with your test. Based on this output, we can start with our first assertion. RTL's `screen` object provides us with a function called `getByText` which is one of many search functions:

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

For the two assertions, we use two new assertive functions called `toBeInTheDocument` and `toHaveAttribute` to verify whether an element with the text "Jordan Walke" is in the document and whether there is an element with the text "React" with a specific `href` attribute value. Over time, you will see more of these assertive functions being used.

RTL's `getByText` search function finds the one element with the visible texts "Jordan Walke" and "React". If you would want to find more than one element, you could use the `getAllByText` equivalent. You will see that these equivalents exist for other search functions too.

Now, this may be the first time you are seeing RTL's philosophy in action. The `getByText` returns the element with a text that users are able to see themselves when using the application. It relates more to how users would use your application for real. We will see that `getByText` is not the only search function though. Another highly used search function is the `getByRole` or `getAllByRole` function:

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

The `getByRole` function is usually used to retrieve elements by [aria-label attributes](https://developer.mozilla.org/en-US/docs/Web/Accessibility/ARIA/ARIA_Techniques/Using_the_aria-label_attribute). However, there are also [implicit roles on HTML elements](https://developer.mozilla.org/en-US/docs/Web/Accessibility/ARIA/Roles) -- like button for a button element. Thus you can select elements not only by visible text, but also by their accessibility role with React Testing Library. A neat feature of `getRoleBy` is that [it suggests roles if you provide a role that's not available](https://twitter.com/rwieruch/status/1260912349978013696). Both, `getByText` and `getByRole` are RTL's most widely used search functions.

We can continue here by asserting not only that everything is *in the document*, but also by asserting whether our events work as expected. For example, the Item component's button element can be clicked and we want to verify that the callback handler gets called. Therefore, we are using Jest for creating a mocked function which we provide as callback handler to the Item component. Then, after firing a click event with React Testing Library on the button, we want to assert that the callback handler function has been called:

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

Jest lets us pass a test-specific function to the Item component as prop. These test specific functions are called **spy**, **stub**, or **mock**; each is used for different test scenarios. The `jest.fn()` returns us a *mock* for the actual function, which lets us capture when it's called. As a result, we can use Jest assertions like `toHaveBeenCalledTimes`, which lets us assert a number of times the function has been called; and `toHaveBeenCalledWith`, to verify arguments that are passed to it.

Ever time we want to spy a JavaScript function, whether it has been called or whether it received certain arguments, we can use Jest's helper function to create a mocked function. Then, after invoking this function implicitly with RTL's `fireEvent` object's function, we can assert that the provided callback handler -- which is the mocked function -- has been called one time.

With the last tests, we have tested the Item component's input and output via rendering assertions and callback handler assertions. However, we are not testing real state changes yet, because there is no actual item removed from the DOM after clicking the "Dismiss" button. The logic to remove the item from the list is in the App component, but we are only testing the Item component in isolation here. We will get to testing the actual implementation logic of removing an Item later when testing the App component.

We will continue with the SearchForm component which uses the InputWithLabel component as child component. As before, we will start with rendering the component with providing all the essential props:

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

Again, we start with the debugging. After evaluating what we have rendered here, we can make our first assertion for the SearchForm. In the case of input fields, the `getByDisplayValue` search function is the perfect candidate to return the input field as element:

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

Since the input element gets rendered with a default value, we can make use of this default value (here "React") -- which is the displayed value -- in our test assertion. If the input element wouldn't have a default value, the application could maybe show a placeholder with the `placeholder` HTML attribute on the input field. Then we could use another function from RTL, called `getByPlaceholderText`, for searching an element with a placeholder text.

Because the debug information showed us lots of options to query our HTML, we could continue with one more test to assert the rendered label:

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

The `getByLabelText` search function allows us to find an element by label in a form. This comes in handy for components which rendered multiple labels and HTML controls. However, you may have also seen how we used a [regular expression](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Regular_Expressions) here. If we would have used a string instead, we would need to include the colon for "Search:". However, by using a regular expression, we are matching strings that include the "Search" string, which makes it way more convenient to find elements. You may find yourself more often using regular expression rather than strings.

Anyway, perhaps it would be more interesting to test the interactive parts of the SearchForm component. Since our callback handlers, which are passed as props to the SearchForm component, are already mocked with Jest, we can assert whether these functions et called appropriately:

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

Similar to the Item component, we tested input (props) and output (callback handler) for the SearchForm component. The difference is that the SearchForm component renders a child component called InputWithLabel. If you check the debug output again, you will see that React Testing Library doesn't care much about this child component. It's because a user of our application wouldn't care about the component as well, so React Testing Library just outputs the HTML for us.

What we have also seen is that all the callback handler tests for Item and SearchForm component only test whether the functions have been called. There is no React re-rendering happening here, because all the components are tested in isolation without state management which solely happens in the App component for us. Therefore, the real testing with RTL starts further up the component tree where state changes and side-effects can be evaluated

### Exercises:

* Confirm your [source code for the last section](https://codesandbox.io/s/github/the-road-to-learn-react/hacker-stories/tree/hs/react-testing-unit-component).
  * Confirm the [changes from the last section](https://github.com/the-road-to-learn-react/hacker-stories/compare/hs/react-testing-unit-function...hs/react-testing-unit-component?expand=1).
* Read more about [React Testing Library](https://testing-library.com/docs/react-testing-library/intro).
  * Read more about [search functions](https://testing-library.com/docs/guide-which-query).
* Add tests for your List and InputWithLabel components.

### Integration Testing: Component

React Testing Library comes with one core philosophy: Rather than testing the implementation details of your React components, we are testing how a user would interact with our application and whether everything works as expected while interacting with it. This becomes especially powerful for integration tests.

Before we can begin testing anything for the App component, we need to provide data first, because the App component makes a request to our remote API for data after its initial render. We start with mocking and testing this part of the component. Therefore, import axios -- which we are using in the App component for our data request -- and mock it with Jest at the top of your testing file:

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

Second, implement the data you want to return from a mocked API request with a JavaScript Promise and use it for the axios mock. Afterward, we can render our component and assume that the correct data is mocked for our API request:

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

Now we use React Testing Library's `act` helper function to wait for our promise to resolve after the component rendered for the first time. With async/await, we can implement this like synchronous code. The great thing about the `debug` function from RTL is that it will output the App component's elements before and after the request for us:

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

Evaluating the output from debug, we can see that the loading indicator is rendered for the first debug function but absent for the second debug function. This is because the data fetching and component re-render is complete after we resolve the promise in our test with `act`. Let's assert the loading indicator for this case:

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

We will be using RTL's `queryByText` instead of `getByText` function this time, because we are testing for a returned element not being there anymore. If we would test this with `getByText`, we would get an error, because the element wouldn't be found. But with `queryByText` we just get `null` if no element is there.

Again, we are using a regular expression `/Loading/` instead of a string `'Loading'`. If we would want to use a string, we would have to be explicit by using `'Loading ...'` instead of `'Loading'`. With a regular expression, we are not forced to provide the whole content of the string but just matching a part of it.

Next, we can assert whether our fetched data gets rendered as expected:

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

The happy path for the data fetching is tested now. Similar to it, we can test the *unhappy path* in case of a failing API request. The only thing we need to change is that the promise needs to reject and that the error needs to be caught with a try/catch block:

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

You may notice that it becomes a bit fuzzy here when to use the `getBy` and when to use the `queryBy` search variants. As rule of thumb, use `getBy` for single elements and `getAllBy` for multiple elements. If you are checking for elements which aren't there, use `queryBy` (or `queryAllBy`). For the sake of readability and alignment, I used `queryBy` for all elements in this case though.

We know that the initial data fetching works for our App component now. Next, let's test the user interactions, which we have tested previously only in the child components by firing events without any state and side-effect. We will start with removing an item from the list after the data has been fetched successfully. Since the item with "Jordan Walke" is our first rendered item in the list, it gets removed if we click the first "Dismiss" button:

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

Next, we will test the search feature. First, we have to set up the mocking a bit differently than before, because we are dealing with the initial request (as before) but also with another request once a user searches for more stories by a specific search term:

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

    axios.get.mockImplementation(url => {
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

Instead of mocking the request once with Jest (`mockImplementationOnce`), we are mocking multiple requests (`mockImplementation`) now. Depending on the incoming URL, the request either returns the initial list ("React"-related stories) or the new list ("JavaScript"-related stories). Just in case if we provide a wrong URL to the request, the test will throw an error for us to get some confirmation that we set up the test wrong. Now, like before, let's render the App component:

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

We are resolving the first promise for the initial render and expect the input field to render "React" and the two items in the list to render the creators of React and Redux. We also make sure that no JavaScript related stories are rendered yet. Next, we are going to change the input field's value by firing an event and assert whether the new value is rendered from the App component through all its child components in the actual input field:

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

Last, we can submit this search request by firing a submit event with the button. The new search term will be used from the App component's state and thus the new URL should search for JavaScript related stories which we have mocked before:

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

Now Brendan Eich as the creator of JavaScript should be rendered, while the other creators of React and Redux shouldn't be there anymore. The last test has shown you how to depict an entire test scenario in one test case. We can move through each step, like initial fetching, changing the value of the input field, submitting the form, and retrieving new data from the API, with our tools at hand.

After all, React Testing Library with Jest is the status quo library combination when it comes to testing React. Whereas RTL gives you all the React relevant testing tools, Jest comes with the general testing framework for test suites, test cases, assertions and mocking capabilities. A popular alternative to RTL is called [Enzyme](https://www.robinwieruch.de/react-testing-jest-enzyme) by Airbnb.

### Exercises:

* Confirm your [source code for the last section](https://codesandbox.io/s/github/the-road-to-learn-react/hacker-stories/tree/hs/react-testing-integration).
  * Confirm the [changes from the last section](https://github.com/the-road-to-learn-react/hacker-stories/compare/hs/react-testing-unit-component...hs/react-testing-integration?expand=1).
* Read more about [E2E tests in React](https://www.robinwieruch.de/react-testing-cypress).
* While you continue with the learning experience in the upcoming sections, keep your tests green and add new tests whenever you feel the need for it.

### Snapshot Testing

Not related to the testing pyramid and its kinds of tests, Facebook came up with the idea of snapshot tests for React components to find a more lightweight way to test components and their structure. Basically a snapshot test creates a snapshot of your rendered component's output when you run your test. This snapshot is used for diffing it to the next snapshot whenever you run your test again. If your rendered component's output has changed, the diff of both snapshots will show it and the snapshot test will fail. That's not bad at all, because the snapshot test should only inform you when the output of your rendered component has changed. In case a snapshot test fails, you can either accept the changes or deny them and fix your component's implementation regarding of its rendered output.

By using snapshot tests, you can keep your tests lightweight, without worrying too much about implementation details of the component. Let's see how these work in React. So let's do a snapshot test for the SearchForm component:

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

After running your tests with `npm test`, you should see a new *src/__snapshots__* folder in your project. Browse into this folder and check out the file that has been generated there for you. Similar to RTL's `debug` function, you should see a snapshot of your rendered SearchForm component as HTML element structure in there. Now, go to your *src/App.js* file and change something in the HTML. For example, in the SearchForm component remove the bold text:

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

After your tests run again, you should see something similar to the following on the command line:

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

That's the typical case for a breaking snapshot test. Whenever you change a component's HTML structure and you didn't intend to change it, your snapshot test will tell you on the command line. Either you fix it and or you accept the change: If you want to fix it, you would go into your *src/App.js* file again and fix the SearchForm component. However, if you intended to perform this change, you can press "u" on the command line for your interactive tests and your new snapshot will be created. Try it yourself and see how the snapshot file in your *src/__snapshots__* folder applies the changes.

Jest stores snapshots in a folder so it can validate the difference against future snapshot tests. Users can share these snapshots across teams for version control (e.g. git). Running a snapshot test for the first time creates the snapshot file in your project's folder. When the test is run again, the snapshot is expected to match the version from the latest test run. This is how we make sure the DOM stays the same.

After all, snapshot tests are great for getting tests up and running quickly in React. However, it's not a great idea to snap snapshot tests on every component without any other tests. It's a good practice to use snapshot tests for components that don't update often, are not too complex, and make it easy to see what you are testing. Then snapshot tests are your most lightweight tool for testing.

### Exercises:

* Confirm your [source code for the last section](https://codesandbox.io/s/github/the-road-to-learn-react/hacker-stories/tree/hs/react-snapshot).
  * Confirm the [changes from the last section](https://github.com/the-road-to-learn-react/hacker-stories/compare/hs/react-testing-integration...hs/react-snapshot?expand=1).
* Add one snapshot test for each of all the other components and check the content of your *__snapshots__* folder.
