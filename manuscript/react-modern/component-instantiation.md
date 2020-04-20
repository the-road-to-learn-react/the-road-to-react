## React Component Instantiation

Next, I'll briefly explain JavaScript classes, to help clarify React components. Technically they are not related, which is important to note, but it is a fitting analogy for you to understand the concept of a component.

Classes are most often used in object-oriented programming languages. JavaScript, always flexible in its programming paradigms, allows functional programming and object-oriented programming to co-exist side-by-side. To recap JavaScript classes for object-oriented programming, consider the following *Developer* class:

{title="Code Playground",lang="javascript"}
~~~~~~~
class Developer {
  constructor(firstName, lastName) {
    this.firstName = firstName;
    this.lastName = lastName;
  }

  getName() {
    return this.firstName + ' ' + this.lastName;
  }
}
~~~~~~~

Each class has a constructor that takes arguments and assigns them to the class instance. A class can also define functions that are associated with a subject (e.g. `getName`), called **methods** or **class methods**.

Defining the Developer class once is just one part; instantiating it is the other. The class definition is the blueprint of its capabilities, and usage occurs when an instance is created with the `new` statement.

{title="Code Playground",lang="javascript"}
~~~~~~~
// class definition
class Developer { ... }

// class instantiation
const robin = new Developer('Robin', 'Wieruch');

console.log(robin.getName());
// "Robin Wieruch"

// another class instantiation
const dennis = new Developer('Dennis', 'Wieruch');

console.log(dennis.getName());
// "Dennis Wieruch"
~~~~~~~

If a JavaScript class definition exists, one can create *multiple* instances of it. It is similar to a React component, which has only *one* component definition, but can have *multiple* component instances:

{title="src/App.js",lang="javascript"}
~~~~~~~
// definition of App component
function App() {
  return (
    <div>
      <h1>My Hacker Stories</h1>

      <label htmlFor="search">Search: </label>
      <input id="search" type="text" />

      <hr />

      {/* creating an instance of List component */}
      <List />
      {/* creating another instance of List component */}
      <List />
    </div>
  );
}

// definition of List component
function List() { ... }
~~~~~~~

Once we've defined a **component**, we can use it like an HTML **element** anywhere in our JSX. The element produces an **component instance** of your component, or in other words, the component gets instantiated. You can create as many component instances as you want. It's not much different from a JavaScript class definition and usage.

### Exercises:

* Familiarize yourself with the terms *component definition*, *component instance*, and *element*.
* Experiment by creating multiple component instances of a List component.
* Think about how it could be possible to give each List component its own `list`.