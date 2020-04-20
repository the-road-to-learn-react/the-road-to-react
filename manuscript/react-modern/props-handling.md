## Props Handling (Advanced)

Props are passed from parent to child down the component tree. Since we use props to transport information from component to component frequently, and sometimes via other components which are in between, it is useful to know a few tricks to make passing props more convenient.

*Note: The following refactorings are recommended for you to learn different JavaScript/React patterns, though you can still build complete React applications without them. Consider this advanced React techniques that will make your source code more concise.*

React props are a JavaScript object, else we couldn't access `props.list` or `props.onSearch` in React components. Since `props` is an object which just passes information from one component to another component, we can apply a couple JavaScript tricks to it. For example, accessing an object's properties with modern [JavaScript object destructuring](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Destructuring_assignment):

{title="Code Playground",lang="javascript"}
~~~~~~~
const user = {
  firstName: 'Robin',
  lastName: 'Wieruch',
};

// without object destructuring
const firstName = user.firstName;
const lastName = user.lastName;

console.log(firstName + ' ' + lastName);
// "Robin Wieruch"

// with object destructuring
const { firstName, lastName } = user;

console.log(firstName + ' ' + lastName);
// "Robin Wieruch"
~~~~~~~

If we need to access multiple properties of an object, using one line of code instead of multiple lines is often simpler and more elegant. That's why object destructuring is already widely used in JavaScript. Let's transfer this knowledge to the React props in our Search component. First, we have to refactor the Search component's arrow function from concise body into block body:

{title="src/App.js",lang="javascript"}
~~~~~~~
# leanpub-start-insert
const Search = props => {
  return (
# leanpub-end-insert
    <div>
      <label htmlFor="search">Search: </label>
      <input
        id="search"
        type="text"
        value={props.search}
        onChange={props.onSearch}
      />
    </div>
# leanpub-start-insert
  );
};
# leanpub-end-insert
~~~~~~~

And second, we can apply the destructuring of the `props` object in the component's function body:

{title="src/App.js",lang="javascript"}
~~~~~~~
const Search = props => {
# leanpub-start-insert
  const { search, onSearch } = props;
# leanpub-end-insert

  return (
    <div>
      <label htmlFor="search">Search: </label>
      <input
        id="search"
        type="text"
# leanpub-start-insert
        value={search}
        onChange={onSearch}
# leanpub-end-insert
      />
    </div>
  );
};
~~~~~~~

That's a basic destructuring of the `props` object in a React component, so that the object's properties can be used conveniently in the component. However, we also had to refactor the Search component's arrow function from concise body into block body to access the properties of `props` with the object destructuring in the function's body. This would happen quite often if we followed this pattern, and it wouldn't make things easier for us, because we would constantly have to refactor our components. We can take all this one step further by destructuring the `props` object right away in the function signature of our component, omitting the function's block body of the component again:

{title="src/App.js",lang="javascript"}
~~~~~~~
# leanpub-start-insert
const Search = ({ search, onSearch }) => (
# leanpub-end-insert
  <div>
    <label htmlFor="search">Search: </label>
    <input
      id="search"
      type="text"
      value={search}
      onChange={onSearch}
    />
  </div>
 # leanpub-start-insert
);
# leanpub-end-insert
~~~~~~~

React's `props` are rarely used in components by themselves; rather, all the information that is contained in the `props` object is used. By destructuring the `props` object right away in the function signature, we can conveniently access all information without dealing with its `props` container. This should be the basic lesson learned from this section, however, we can take this one step further with the following advanced lessons.

Let's check out another scenario to dive deeper into advanced props handling in React: In order to prepare for this scenario, we will extract a new Item component from the List component with the previous lesson learned about object destructuring for React's `props` object:

{title="src/App.js",lang="javascript"}
~~~~~~~
# leanpub-start-insert
const List = ({ list }) =>
  list.map(item => <Item key={item.objectID} item={item} />);
# leanpub-end-insert

# leanpub-start-insert
const Item = ({ item }) => (
  <div>
    <span>
      <a href={item.url}>{item.title}</a>
    </span>
    <span>{item.author}</span>
    <span>{item.num_comments}</span>
    <span>{item.points}</span>
  </div>
);
# leanpub-end-insert
~~~~~~~

Now, the incoming `item` in the Item component has something in common with the previously discussed `props`: they are both JavaScript objects. Also, even though the `item` object has already been destructured from the `props` in the Item component's function signature, it isn't directly used in the Item component. The `item` object only passes its information (object properties) to the elements.

The shown solution is fine as you will see in the ongoing discussion. However, I want to show you two more variations of it, because there are many things to learn about JavaScript objects here. Let's get started with *nested destructuring* and how it works:

{title="Code Playground",lang="javascript"}
~~~~~~~
const user = {
  firstName: 'Robin',
  pet: {
    name: 'Trixi',
  },
};

// without object destructuring
const firstName = user.firstName;
const name = user.pet.name;

console.log(firstName + ' has a pet called ' + name);
// "Robin has a pet called Trixi"

// with nested object destructuring
const {
  firstName,
  pet: {
    name,
  },
} = user;

console.log(firstName + ' has a pet called ' + name);
// "Robin has a pet called Trixi"
~~~~~~~

Nested destructuring helps us to access properties from objects which are deeply nested (e.g. the pet's name of a user). Now, in our Item components, because the `item` object is never directly used in the Item component's JSX elements, we can perform a *nested destructuring* in the component's function signature too:

{title="src/App.js",lang="javascript"}
~~~~~~~
// Variation 1: Nested Destructuring

const Item = ({
# leanpub-start-insert
  item: {
    title,
    url,
    author,
    num_comments,
    points,
  },
# leanpub-end-insert
}) => (
  <div>
    <span>
# leanpub-start-insert
      <a href={url}>{title}</a>
# leanpub-end-insert
    </span>
# leanpub-start-insert
    <span>{author}</span>
    <span>{num_comments}</span>
    <span>{points}</span>
# leanpub-end-insert
  </div>
);
~~~~~~~

The nested destructuring helps us to gather all the needed information of the `item` object in the function signature for its immediate usage in the component's elements. However, nested destructuring introduces lots of clutter through indentations in the function signature. While it's here not the most readable option, it can be useful in other scenarios though.

Let's take another approach with JavaScript's spread and rest operators. In order to prepare for it, we will refactor our List and Item components to the following implementation. Rather than passing the item as object from List to Item component, we are passing every property of the `item` object:

{title="src/App.js",lang="javascript"}
~~~~~~~
// Variation 2: Spread and Rest Operators
// 1. Iteration

const List = ({ list }) =>
  list.map(item => (
    <Item
      key={item.objectID}
# leanpub-start-insert
      title={item.title}
      url={item.url}
      author={item.author}
      num_comments={item.num_comments}
      points={item.points}
# leanpub-end-insert
    />
  ));

# leanpub-start-insert
const Item = ({ title, url, author, num_comments, points }) => (
# leanpub-end-insert
  <div>
    <span>
      <a href={url}>{title}</a>
    </span>
    <span>{author}</span>
    <span>{num_comments}</span>
    <span>{points}</span>
  </div>
);
~~~~~~~

Now, even though the Item component's function signature is more concise, the clutter ended up in the List component instead, because every property is passed to the Item component individually. We can improve this approach using [JavaScript's spread operator](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Spread_syntax):

{title="Code Playground",lang="javascript"}
~~~~~~~
const profile = {
  firstName: 'Robin',
  lastName: 'Wieruch',
};

const address = {
  country: 'Germany',
  city: 'Berlin',
  code: '10439',
};

const user = {
  ...profile,
  gender: 'male',
  ...address,
};

console.log(user);
// {
//   firstName: "Robin",
//   lastName: "Wieruch",
//   gender: "male"
//   country: "Germany,
//   city: "Berlin",
//   code: "10439"
// }
~~~~~~~

JavaScript's spread operator allows us to literally spread all key/value pairs of an object to another object. This can also be done in React's JSX. Instead of passing each property one at a time via props from List to Item component as before, we can use JavaScript's spread operator to pass all the object's key/value pairs as attribute/value pairs to a JSX element:

{title="src/App.js",lang="javascript"}
~~~~~~~
// Variation 2: Spread and Rest Operators
// 2. Iteration

const List = ({ list }) =>
# leanpub-start-insert
  list.map(item => <Item key={item.objectID} {...item} />);
# leanpub-end-insert

const Item = ({ title, url, author, num_comments, points }) => (
  <div>
    <span>
      <a href={url}>{title}</a>
    </span>
    <span>{author}</span>
    <span>{num_comments}</span>
    <span>{points}</span>
  </div>
);
~~~~~~~

This refactoring made the process of passing the information from List to Item component more concise. Finally, we'll use [JavaScript's rest parameters](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/rest_parameters) as the icing on the cake. The JavaScript rest operator happens always as the last part of an object destructuring:

{title="Code Playground",lang="javascript"}
~~~~~~~
const user = {
  id: '1',
  firstName: 'Robin',
  lastName: 'Wieruch',
  country: 'Germany',
  city: 'Berlin',
};

const { id, country, city, ...userWithoutAddress } = user;

console.log(userWithoutAddress);
// {
//   firstName: "Robin",
//   lastName: "Wieruch"
// }

console.log(id);
// "1"

console.log(city);
// "Berlin"
~~~~~~~

Even though both have the same syntax (three dots), the rest operator shouldn't be mistaken with the spread operator. Whereas the rest operator happens on the right side of an assignment, the spread operator happens on the left side. The rest operator is always used to separate an object from some of its properties.

Now it can be used in our List component to separate the `objectID` from the item, because the `objectID` is only used as `key` and isn't used in the Item component. Only the remaining (rest) item gets spread as attribute/value pairs into the Item component (as before):

{title="src/App.js",lang="javascript"}
~~~~~~~
// Variation 2: Spread and Rest Operators (final)

const List = ({ list }) =>
# leanpub-start-insert
  list.map(({ objectID, ...item }) => <Item key={objectID} {...item} />);
# leanpub-end-insert

const Item = ({ title, url, author, num_comments, points }) => (
  <div>
    <span>
      <a href={url}>{title}</a>
    </span>
    <span>{author}</span>
    <span>{num_comments}</span>
    <span>{points}</span>
  </div>
);
~~~~~~~

In this final variation, the rest operator is used to destructure the `objectID` from the rest of the `item` object. Afterward, the `item` is spread with its key/values pairs into the Item component. While this final variation is very concise, it comes with  advanced JavaScript features that may be unknown to some.

In this section, we have learned about JavaScript object destructuring which can be used commonly for the `props` object, but also for other objects like the `item` object. We have also seen how nested destructuring can be used (Variation 1), but also how it didn't add any benefits in our case, because it just made the component bigger. In the future you will find more likely use cases for nested destructuring which are beneficial. Last but not least, you have learned about JavaScript's spread and rest operators, which shouldn't be confused with each other, to perform operations on JavaScript objects and to pass the `props` object from one component to another component in the most concise way. In the end, I want to point out the initial version again which we will keep over the next chapters:

{title="src/App.js",lang="javascript"}
~~~~~~~
const List = ({ list }) =>
  list.map(item => <Item key={item.objectID} item={item} />);

const Item = ({ item }) => (
  <div>
    <span>
      <a href={item.url}>{item.title}</a>
    </span>
    <span>{item.author}</span>
    <span>{item.num_comments}</span>
    <span>{item.points}</span>
  </div>
);
~~~~~~~

It may not be the most concise, but it is the easiest to reason about. Variation 1 with its nested destructuring didn't add much benefit and variation 2 may add too many advanced JavaScript features (spread operator, rest operator) which are not familiar to everyone. After all, all these variations have their pros and cons. When refactoring a component, always aim for readability, especially when working in a team of people, and make sure make sure they're using a common React code style.

### Exercises:

* Confirm your [source code for the last section](https://codesandbox.io/s/github/the-road-to-learn-react/hacker-stories/tree/hs/Props-Handling).
  * Confirm the [changes from the last section](https://github.com/the-road-to-learn-react/hacker-stories/compare/hs/React-Controlled-Components...hs/Props-Handling?expand=1).
* Read more about [JavaScript's destructuring assignment](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Destructuring_assignment).
* Think about the difference between  JavaScript array destructuring -- which we used for React's `useState` hook -- and object destructuring.
* Read more about [JavaScript's spread operator](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Spread_syntax).
* Read more about [JavaScript's rest parameters](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/rest_parameters).
* Get a good sense about JavaScript (e.g. spread operator, rest parameters, destructuring) and what's related to React (e.g. props) from the last lessons.
* Continue to use your favorite way to handle React's props. If you're still undecided, consider the variation used in the previous section.