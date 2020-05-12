## Advanced Testing in React

Earlier we have used React Test Renderer and Jest to test our React components. This was really the bare bones testing setup for React. In this section, I want to show you a more sophisticated tool called React Testing Library (RTL) which would replace Rest Test Renderer from earlier.

React Testing Library works great with Jest. While Jest gives you a test framework with test suites, test cases, assertions and mocking features, RTL helps you to render React components, to select HTML elements within the DOM, and to fire events on HTML element.

React Testing Library comes with a great philosophy. Rather than testing the implementation details of your React components, we are testing how a user would interact with our application and whether everything works as expected. After this section, you may want to look back at our old implementation with React Test Renderer and compare it to RTL to see the differences.

In this section, we will write all of our tests from scratch in the *src/App.test.js* file. You can also create a second file for these new tests to have a side-by-side comparison later. At the end, I hope you come out with the same good feeling that I had after using RTL for the first time.

Since you are using create-react-app, you don't need to set up anything for RTL, because it ships by default with this project. If you are using a custom React setup (e.g. React with Webpack), you would have to install RTL yourself.

Let's start with our new tests in the *src/App.test.js* file. We will already import all the components we are going to test and import all the utility functions from RTL as well. In addition, we define all the test suites too:

{title="src/App.test.js",lang="javascript"}
~~~~~~~
# leanpub-start-insert
import React from 'react';
import { render, fireEvent, act } from '@testing-library/react';

import App, { Item, List, SearchForm } from './App';

describe('Item', () => {

});

describe('List', () => {

});

describe('SearchForm', () => {

});

describe('App', () => {

});
# leanpub-end-insert
~~~~~~~

We are going to start with the Item component where we just want to assert whether it renders all the expected properties based on the given props:

{title="src/App.test.js",lang="javascript"}
~~~~~~~
describe('Item', () => {
# leanpub-start-insert
  const item = {
    title: 'React',
    url: 'https://reactjs.org/',
    author: 'Jordan Walke',
    num_comments: 3,
    points: 4,
    objectID: 0,
  };
# leanpub-end-insert

# leanpub-start-insert
  it('renders all properties', () => {
    const objectWithFunctions = render(<Item item={item} />);
# leanpub-end-insert
  });
});
~~~~~~~

Essentially we will use RTL's render function in every test to render a React component. In this case, we render the Item component as element and pass it an `item` object as props. The render function then returns an object which has lots of powerful functions as properties. One of these first powerful functions is the `debug` function:

{title="src/App.test.js",lang="javascript"}
~~~~~~~
describe('Item', () => {
  const item = { ... };

  it('renders all properties', () => {
    const objectWithFunctions = render(<Item item={item} />);

# leanpub-start-insert
    objectWithFunctions.debug();
# leanpub-end-insert
  });
});
~~~~~~~

Since it's an object that is returned from the render function, we can destructure it right away, because usually we are not using the object itself:

{title="src/App.test.js",lang="javascript"}
~~~~~~~
describe('Item', () => {
  const item = { ... };

# leanpub-start-insert
  it('renders all properties', () => {
    const { debug } = render(<Item item={item} />);

    debug();
# leanpub-end-insert
  });
});
~~~~~~~

Once you run your tests with `npm test`, you should be able to see the output that the `debug` function is giving you. In our case it's something like the following:

{title="Command Line",lang="text"}
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

Based on this knowledge, we can start with our first assertion. The `render` function's object provides us with a function called `getByText`. After the `render` and `debug` functions, this should be the most used function from RTL to perform assertions:

{title="src/App.test.js",lang="javascript"}
~~~~~~~
describe('Item', () => {
  const item = {
    title: 'React',
    url: 'https://reactjs.org/',
    author: 'Jordan Walke',
    num_comments: 3,
    points: 4,
    objectID: 0,
  };

  it('renders all properties', () => {
# leanpub-start-insert
    const { getByText } = render(<Item item={item} />);
# leanpub-end-insert

# leanpub-start-insert
    expect(getByText('Jordan Walke')).toBeTruthy();
    expect(getByText('React')).toHaveAttribute(
      'href',
      'https://reactjs.org/'
    );
# leanpub-end-insert
  });
});
~~~~~~~

With `getByText` we return the element which has the visible text "Jordan Walke" or "React" -- which in a real world scenario would be visible to a user too. To assert whether the former element is truly there, we can just assert its truthfulness. To assert whether the latter element has a `href` attribute, we can assert the anchor's href value.

This may be the first time you are seeing RTL's philosophy in action. The `getByText` literally returns the element with a text that users are able to see themselves. It relates more to how users would use your application. We will see that `getByText` is not the only selector though.

We tested the Item component for its rendered elements based on the incoming props. Now we are going to test whether the callback handler is working as expected if a user interacts with the component. Therefore, the component receives a mocked callback handler as prop this time:

{title="src/App.test.js",lang="javascript"}
~~~~~~~
describe('Item', () => {
  const item = { ... };
# leanpub-start-insert
  const handleRemoveItem = jest.fn();
# leanpub-end-insert

  it('renders all properties', () => { ... });

# leanpub-start-insert
  it('calls onRemoveItem on button click', () => {
    const { debug } = render(
      <Item item={item} onRemoveItem={handleRemoveItem} />
    );

    debug();
  });
# leanpub-end-insert
});
~~~~~~~

Next we can use RTL's `fireEvent` utility function to trigger a specific event (here click) on an element that is passed in as argument. We can get this element (the "Dismiss" button) again with the render object's `getByText` function. Afterward, we can assert with Jest whether the mocked callback function has been called and whether the correct arguments were provided to the callback function:

{title="src/App.test.js",lang="javascript"}
~~~~~~~
describe('Item', () => {
  const item = { ... };
  const handleRemoveItem = jest.fn();

  it('renders all properties', () => { ... });

  it('calls onRemoveItem on button click', () => {
# leanpub-start-insert
    const { getByText } = render(
# leanpub-end-insert
      <Item item={item} onRemoveItem={handleRemoveItem} />
    );

# leanpub-start-insert
    fireEvent.click(getByText('Dismiss'));
# leanpub-end-insert

# leanpub-start-insert
    expect(handleRemoveItem).toHaveBeenCalledTimes(1);
    expect(handleRemoveItem).toHaveBeenCalledWith(item);
# leanpub-end-insert
  });
});
~~~~~~~

Now we tested the Item component's input and output with rendering and callback handler assertions. However, we are not testing real state changes yet, because there is no actual item removed from the DOM after clicking the "Dismiss" button. The logic to remove the item from the list is in the App component, but we are only testing the Item component in isolation here. We will get to testing the actual implementation logic of removing an Item later when testing the App component.

From here, we'll move on to testing the List component. It's not much different from the Item component, because we will only test input and output of the component, because the component itself doesn't have state or side-effects:

{title="src/App.test.js",lang="javascript"}
~~~~~~~
# leanpub-start-insert
describe('List', () => {
  const list = [
    {
      title: 'React',
      url: 'https://reactjs.org/',
      author: 'Jordan Walke',
      num_comments: 3,
      points: 4,
      objectID: 0,
    },
    {
      title: 'Redux',
      url: 'https://redux.js.org/',
      author: 'Dan Abramov, Andrew Clark',
      num_comments: 2,
      points: 5,
      objectID: 1,
    },
  ];

  it('renders two items', () => {
    const { debug } = render(<List list={list} />);

    debug();
  });
});
# leanpub-end-insert
~~~~~~~

Again, we are going to debug our output first, because this helps us to get an overview of the available elements; their structure, attributes and texts. The debug result shows us that RTL doesn't think in implementation details, because it just outputs HTML. There is no List and no Item component, it's just HTML elements. After finishing our evaluation of the debug result, we want to assert that there are two rendered items:

{title="src/App.test.js",lang="javascript"}
~~~~~~~
describe('List', () => {
  const list = [ ... ];

  it('renders two items', () => {
    const { getAllByText, getAllByRole, container } = render(
      <List list={list} />
    );

# leanpub-start-insert
    expect(getAllByText('Dismiss').length).toBe(2); // (1)
    expect(getAllByRole('button').length).toBe(2); // (2)
    expect(container.querySelectorAll('button').length).toBe(2); // (3)
# leanpub-end-insert
  });
});
~~~~~~~

There are several ways to test whether we have two rendered items here: First, we could check whether there are two elements with the "Dismiss" text rendered. Because each list item renders a button to remove an item, there should be two of these items for our assertion. Here we are using `getAllByText` instead of `getByText`, because we know that we are dealing with a list of elements rather than a single element. Every selector function comes with a `getBy` and `getAllBy` option.

Second, we could check whether there are two buttons rendered. Usually `getByRole` and `getAllByRole` are used for [aria-label attributes](https://developer.mozilla.org/en-US/docs/Web/Accessibility/ARIA/ARIA_Techniques/Using_the_aria-label_attribute), but here RTL steps in for us without having us to define the roles explicitly on the HTML elements in the actual implementation in the *src/App.js* file.

Third, we could use so called manual queries with the [native querySelector API](https://developer.mozilla.org/en-US/docs/Web/API/Document/querySelector) by using the `container` object. That's one of the least desired options though, because it tends more towards testing implementation logic than testing what the user sees.

If none of these selectors would work, you could always go into your source code and assign `data-testid` attributes to your HTML elements for retrieving them with `getByTestId` in your tests. Please refer to this [general overview of which query to use with RTL](https://testing-library.com/docs/guide-which-query).

You could continue testing the List component by checking whether each callback handler (here `onRemoveItem`) is called for each Item component, which would have a similar solution to the previous Item component's test. We will move on to the SearchForm with InputWithLabel component however:

{title="src/App.test.js",lang="javascript"}
~~~~~~~
# leanpub-start-insert
describe('SearchForm', () => {
  const searchFormProps = {
    searchTerm: 'React',
    onSearchInput: jest.fn(),
    onSearchSubmit: jest.fn(),
  };

  it('renders the input field with its value', () => {
    const { debug } = render(<SearchForm {...searchFormProps} />);

    debug();
  });
});
# leanpub-end-insert
~~~~~~~

Again, we start with the debugging. After evaluating what we have rendered here, we can make our first assertion for the SearchForm. In the case of input fields, the `getByDisplayValue` function is the perfect selector to return the input field as element:

{title="src/App.test.js",lang="javascript"}
~~~~~~~
describe('SearchForm', () => {
  const searchFormProps = {
    searchTerm: 'React',
    onSearchInput: jest.fn(),
    onSearchSubmit: jest.fn(),
  };

  it('renders the input field with its value', () => {
# leanpub-start-insert
    const { getByDisplayValue } = render(
# leanpub-end-insert
      <SearchForm {...searchFormProps} />
    );

# leanpub-start-insert
    expect(getByDisplayValue('React')).toBeTruthy();
# leanpub-end-insert
  });
});
~~~~~~~

Since the input element gets rendered with a default value, we can make use of this default value (here "React") -- which is the displayed value -- in our test assertion. If the input element wouldn't have a default value, the application could maybe show a placeholder with the `placeholder` HTML attribute on the input field. Then we could use another function from RTL called `getByPlaceholderText`. While the debug information showed us lots of options to query our HTML, we could continue with more tests to assert the rendered label and the rendered button:

{title="src/App.test.js",lang="javascript"}
~~~~~~~
describe('SearchForm', () => {
  const searchFormProps = {
    searchTerm: 'React',
    onSearchInput: jest.fn(),
    onSearchSubmit: jest.fn(),
  };

  it('renders the input field with its value', () => {
    ...
  });

# leanpub-start-insert
  it('renders the correct label', () => {
    const { getByLabelText } = render(
      <SearchForm {...searchFormProps} />
    );

    expect(getByLabelText('Search:')).toBeTruthy();
  });

  it('renders a submit button', () => {
    const { getByText } = render(<SearchForm {...searchFormProps} />);

    expect(getByText('Submit')).toBeTruthy();
  });
# leanpub-end-insert
});
~~~~~~~

It would be more interesting though to test the interactive parts of the component. Since our callback handlers which are passed as props to the SearchForm component are already mocked with Jest, we can assert whether these functions are called appropriately:

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
  it('calls onSearchInput on input field change', () => {
    const { getByDisplayValue } = render(
      <SearchForm {...searchFormProps} />
    );

    fireEvent.change(getByDisplayValue('React'), {
      target: { value: 'Redux' },
    });

    expect(searchFormProps.onSearchInput).toHaveBeenCalledTimes(1);
  });

  it('calls onSearchSubmit on button submit click', () => {
    const { getByText } = render(<SearchForm {...searchFormProps} />);

    fireEvent.submit(getByText('Submit'));

    expect(searchFormProps.onSearchSubmit).toHaveBeenCalledTimes(1);
  });
# leanpub-end-insert
});
~~~~~~~

What we have also seen is that all the callback handler tests for Item, List, and SearchForm component only test whether the functions have been called. There is no real React re-rendering happening here, because all the components are tested in isolation without state management which solely happens in the App component. Therefore, the real testing with RTL starts further up the component tree where state changes and side-effects can be evaluated:

{title="src/App.test.js",lang="javascript"}
~~~~~~~
# leanpub-start-insert
describe('App', () => {
  it('removes a story from the list', async () => {

  });

  it('searches for specific items in the list', async () => {

  });
});
# leanpub-end-insert
~~~~~~~

Before we can begin testing these interactive parts, we need to provide data first. Since the App component makes a request to our remote API for data after its initial render, we start with mocking and testing this part of the component first. Therefore, import axios -- which we are using in the App component for our data request -- and mock it with Jest:

{title="src/App.test.js",lang="javascript"}
~~~~~~~
import React from 'react';
...
# leanpub-start-insert
import axios from 'axios';
# leanpub-end-insert

# leanpub-start-insert
jest.mock('axios');
# leanpub-end-insert

...
~~~~~~~

Second, implement the data you want to return from a mocked API request with a JavaScript Promise and use it for the axios mock:

{title="src/App.test.js",lang="javascript"}
~~~~~~~
describe('App', () => {
# leanpub-start-insert
  const list = [
    {
      title: 'React',
      url: 'https://reactjs.org/',
      author: 'Jordan Walke',
      num_comments: 3,
      points: 4,
      objectID: 0,
    },
    {
      title: 'Redux',
      url: 'https://redux.js.org/',
      author: 'Dan Abramov, Andrew Clark',
      num_comments: 2,
      points: 5,
      objectID: 1,
    },
  ];

  it('succeeds fetching data with a list', () => {
    const promise = Promise.resolve({
      data: {
        hits: list,
      },
    });

    axios.get.mockImplementationOnce(() => promise);

    const { debug } = render(<App />);
  });
# leanpub-end-insert

  it('removes a story from the list', async () => {});

  it('searches for specific items in the list', async () => {});
});
~~~~~~~

Now we use React Testing Library's `act` helper function to wait for our promise to resolve after the component rendered for the first time. With async/await, we can implement this like synchronous code. The great thing about the debug method from RTL is that it will output the App component's elements before and after the request too:

{title="src/App.test.js",lang="javascript"}
~~~~~~~
describe('App', () => {
  const list = [ ... ];

# leanpub-start-insert
  it('succeeds fetching data with a list', async () => {
# leanpub-end-insert
    const promise = Promise.resolve({
      data: {
        hits: list,
      },
    });

    axios.get.mockImplementationOnce(() => promise);

    const { debug } = render(<App />);

# leanpub-start-insert
    debug();

    await act(() => promise);

    debug();
# leanpub-end-insert
  });

  ...
});
~~~~~~~

We will be using RTL's `queryByText` instead of `getByText` method, because we are testing for a returned element not being there anymore. If we would test this with `getByText`, we would get an error, because the element wouldn't be found. But with `queryByText` we just get `null` if no element can be found:

{title="src/App.test.js",lang="javascript"}
~~~~~~~
describe('App', () => {
  const list = [ ... ];

  it('succeeds fetching data with a list', async () => {
    ...

# leanpub-start-insert
    const { queryByText } = render(<App />);
# leanpub-end-insert

# leanpub-start-insert
    expect(queryByText(/Loading/)).toBeTruthy();
# leanpub-end-insert

    await act(() => promise);

# leanpub-start-insert
    expect(queryByText(/Loading/)).toBeFalsy();
# leanpub-end-insert
  });

  ...
});
~~~~~~~

In addition, we are using a [regular expression](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Regular_Expressions) (`/Loading/`) instead of a string (`'Loading'`) here. If we would want to use a string, we would have to be explicit by using `'Loading ...'` instead of `'Loading'`. With a regular expression, we are not forced to provide the whole string content.

Next, we can test whether our fetched list gets rendered as expected. Again we are using `queryBy` instead of `getBy`, because we are not sure if these elements are truly there. Same as `getBy` and `getAllBy`, `queryBy` comes with a `queryAllBy` equivalent:

{title="src/App.test.js",lang="javascript"}
~~~~~~~
describe('App', () => {
  const list = [ ... ];

  it('succeeds fetching data with a list', async () => {
    ...

# leanpub-start-insert
    const { queryByText, queryAllByText } = render(<App />);
# leanpub-end-insert

    expect(queryByText(/Loading/)).toBeTruthy();

    await act(() => promise);

    expect(queryByText(/Loading/)).toBeFalsy();

# leanpub-start-insert
    expect(queryByText('Jordan Walke')).toBeTruthy();
    expect(queryByText('Dan Abramov, Andrew Clark')).toBeTruthy();
    expect(queryAllByText('Dismiss').length).toBe(2);
# leanpub-end-insert
  });

  ...
});
~~~~~~~

Similar to the happy data fetching path, we can test the unhappy path in case our API request fails:

{title="src/App.test.js",lang="javascript"}
~~~~~~~
describe('App', () => {
  const list = [ ... ];

  it('succeeds fetching data with a list', async () => {
    ...
  });

# leanpub-start-insert
  it('fails fetching data with a list', async () => {
    const promise = Promise.reject();

    axios.get.mockImplementationOnce(() => promise);

    const { queryByText } = render(<App />);

    expect(queryByText(/Loading/)).toBeTruthy();

    try {
      await act(() => promise);
    } catch (error) {
      expect(queryByText(/Loading/)).toBeFalsy();
      expect(queryByText(/Something went wrong/)).toBeTruthy();
    }
  });
# leanpub-end-insert
});
~~~~~~~

We know that the initial data fetching works for our App component now. Next, let's test the user interactions, which have have tested previously only in the child components without any state and side-effect, but only by asserting whether the callback handlers have been called. First, we will start with removing an item from the list after the data has been fetched successfully:

{title="src/App.test.js",lang="javascript"}
~~~~~~~
describe('App', () => {
  const list = [ ... ];

  ...

# leanpub-start-insert
  it('removes a story from the list', async () => {
    const promise = Promise.resolve({
      data: {
        hits: list,
      },
    });

    axios.get.mockImplementationOnce(() => promise);

    const { queryByText, getAllByText } = render(<App />);

    await act(() => promise);

    expect(queryByText('Jordan Walke')).toBeTruthy();
    fireEvent.click(getAllByText('Dismiss')[0]);
    expect(queryByText('Jordan Walke')).toBeFalsy();
  });
# leanpub-end-insert
});
~~~~~~~

And second, we will test the search feature. First, we have to set up the mocking a bit differently than before, because we are dealing with the initial request (like before) but also another request once a user searches for more stories by a specific search term:

{title="src/App.test.js",lang="javascript"}
~~~~~~~
describe('App', () => {
  const list = [ ... ];

  ...

# leanpub-start-insert
  it('searches for specific items in the list', async () => {
    const reactPromise = Promise.resolve({
      data: {
        hits: list,
      },
    });

    const javascriptPromise = Promise.resolve({
      data: {
        hits: [
          {
            title: 'JavaScript',
            url: 'https://en.wikipedia.org/wiki/JavaScript',
            author: 'Brendan Eich',
            num_comments: 15,
            points: 10,
            objectID: 3,
          },
        ],
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

Instead of mocking the request once with Jest (`mockImplementationOnce`), we are mocking multiple requests (`mockImplementation`) now. Depending on the incoming URL, the request either returns the initial list ("React"-related stories) or the new list ("JavaScript"-related stories). Just in case if we provide a wrong URL to the request in our test, the test will throw an error for us. Now, like before, let's render the App component:

{title="src/App.test.js",lang="javascript"}
~~~~~~~
describe('App', () => {

  ...

  it('searches for specific items in the list', async () => {
    const reactPromise = Promise.resolve({ ... });

    const javascriptPromise = Promise.resolve({ ... });

    axios.get.mockImplementation((url) => {
      ...
    });

# leanpub-start-insert
    const { queryByText, queryByDisplayValue } = render(<App />);

    await act(() => reactPromise);

    expect(queryByDisplayValue('React')).toBeTruthy();
    expect(queryByDisplayValue('JavaScript')).toBeFalsy();

    expect(queryByText('Jordan Walke')).toBeTruthy();
    expect(queryByText('Dan Abramov, Andrew Clark')).toBeTruthy();
    expect(queryByText('Brendan Eich')).toBeFalsy();
# leanpub-end-insert
  });
});
~~~~~~~

We are resolving the first promise for the initial render and expect the input field to render "React" and the two items in the list to render the creators of React and Redux. We also make sure that no JavaScript related stories are rendered yet. Next, we are going to change the input field's value by firing an event and assert whether the new value is rendered from the App component through all its child components in the actual input field:

{title="src/App.test.js",lang="javascript"}
~~~~~~~
describe('App', () => {
  ...

  it('searches for specific items in the list', async () => {
    const reactPromise = Promise.resolve({ ... });

    const javascriptPromise = Promise.resolve({ ... });

    axios.get.mockImplementation((url) => {
      ...
    });

    ...
    expect(queryByText('Brendan Eich')).toBeFalsy();

# leanpub-start-insert
    fireEvent.change(queryByDisplayValue('React'), {
      target: { value: 'JavaScript' },
    });

    expect(queryByDisplayValue('React')).toBeFalsy();
    expect(queryByDisplayValue('JavaScript')).toBeTruthy();
# leanpub-end-insert
  });
});
~~~~~~~

Last, we can submit this search request by firing a submit event with the button. The new search term will be used from the App component's state and thus the new URL should search for JavaScript related stories which we have mocked before:

{title="src/App.test.js",lang="javascript"}
~~~~~~~
describe('App', () => {
  ...

  it('searches for specific items in the list', async () => {
    const reactPromise = Promise.resolve({ ... });

    const javascriptPromise = Promise.resolve({ ... });

    axios.get.mockImplementation((url) => {
      ...
    });

    ...

    expect(queryByDisplayValue('React')).toBeFalsy();
    expect(queryByDisplayValue('JavaScript')).toBeTruthy();

# leanpub-start-insert
    fireEvent.submit(queryByText('Submit'));

    await act(() => javascriptPromise);

    expect(queryByText('Jordan Walke')).toBeFalsy();
    expect(queryByText('Dan Abramov, Andrew Clark')).toBeFalsy();
    expect(queryByText('Brendan Eich')).toBeTruthy();
# leanpub-end-insert
  });
});
~~~~~~~

Now Brendan Eich as creator of JavaScript should be rendered, while the other creators of React and Redux shouldn't be there anymore. The last test has shown you how to depict an entire test scenario in one test case. We can move through each step, like initial fetching, changing the value of the input field, submitting the form, and retrieving new data from the API, with our tools at hand.

You may have also noticed that it's more desirable to test further up the component tree with React Testing Library. When we have tested the other child components, we only tested whether they render everything correctly based on their incoming props and whether their callback handler were working. Only when we started to test the App component, we had to deal with state management and side-effects like data fetching in our tests. That's the best place where we can assert real user behavior.

After all, React Testing Library with Jest is the status quo when it comes to testing React. Whereas RTL gives you all the React relevant testing tools, Jest comes with the general testing framework for test suites, test cases, assertions and mocking capabilities. A popular alternative to RTL is called [Enzyme](https://www.robinwieruch.de/react-testing-jest-enzyme) by Airbnb.

### Exercises:

* Confirm your [source code for the last section](https://codesandbox.io/s/github/the-road-to-learn-react/hacker-stories/tree/hs/react-testing-advanced).
  * Confirm the [changes from the last section](https://github.com/the-road-to-learn-react/hacker-stories/compare/hs/react-testing..hs/react-testing-advanced?expand=1).
* Read more about [React Testing Library](https://testing-library.com/).
* Come up with some more tests yourself to fortify your learnings.