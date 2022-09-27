## React Component Instantiation

You have learned how to *declare* a component (e.g. `function List() { ... }`) and how to *instantiate* (e.g. `<List />`) it. In this section, we will intensify this learning by going through an analogy and the terminology. We will start with the analogy by using the JavaScript class. Technically, JavaScript classes and React components are not related, which is important to note, but it is still a fitting analogy for you to understand the concept of a component by using something you may have used in the past.

A class is most often used in object-oriented programming languages. JavaScript as a multi-paradigm programming language allows functional programming and object-oriented programming to co-exist side-by-side. To recap JavaScript classes for object-oriented programming, consider the following *Person* class:

{title="Code Playground",lang="javascript"}
~~~~~~~
class Person {
  constructor(firstName, lastName) {
    this.firstName = firstName;
    this.lastName = lastName;
  }

  getName() {
    return this.firstName + ' ' + this.lastName;
  }
}
~~~~~~~

Each class has a constructor that takes arguments and assigns them to the class instance when instantiating it. A class can also define functions that are associated with the instance (e.g. `getName()`) which are called **methods** or **class methods**. Now, declaring the Person class once is just one part; instantiating it is the other. The class declaration is the blueprint of its capabilities and usage occurs when an instance is created with the `new` statement. If a JavaScript class declaration exists, one can create *multiple* instances of it:

{title="Code Playground",lang="javascript"}
~~~~~~~
// class declaration
class Person { ... }

// class instantiation
const robin = new Person('Robin', 'Wieruch');

console.log(robin.getName());
// "Robin Wieruch"

// another class instantiation
const dennis = new Person('Dennis', 'Wieruch');

console.log(dennis.getName());
// "Dennis Wieruch"
~~~~~~~

The concept of a class with declaration and instantiation is similar to a React component, which also has only *one* component declaration, but can have *multiple* component instances:

{title="src/App.jsx",lang="javascript"}
~~~~~~~
// declaration of App component
function App() {
  return (
    <div>
      ...

      {/* creating an instance of List component */}
      <List />
      {/* creating another instance of List component */}
      <List />
    </div>
  );
}

// declaration of List component
function List() { ... }
~~~~~~~

Once we've defined a **component**, we can use it as an **element** anywhere in our JSX. The element produces an **instance** of your component, or in other words, the component gets instantiated. You can create as many instances of a component as you want as long as you have a component declaration. It's not much different from a JavaScript class declaration and instantiation, however, as mentioned before, technically a JavaScript class and React component are not the same. Just their usage makes it convenient to demonstrate their similarities.

### Exercises:

* Read more about [component, element, and instance in React](https://www.robinwieruch.de/react-element-component/).
  * Familiarize yourself with the terms *component declaration*, *component instance*, and *element*.
* Experiment by creating multiple component instances of a List component.
* If we keep treating the `list` variable as a global variable, every List component would use the same `list`. Think about how it could be possible to give each List component its own `list` variable.
* Optional: [Leave feedback for this section](https://forms.gle/sf1UMNR58v3NsRUSA).