# Fundamentals of React

In the first part of this learning experience, you will learn everything about the fundamentals -- basics but also advanced topics -- in React. After getting a first introduction to React, you will dive right away into creating your first React project. Every other section will teach you something new about React and its aspects by implementing real features. In the end, you will have a working React application with features like client- and server-side searching, remote data fetching, and advanced state management.

## Hello there, my name is React.

Single page applications ([SPA](https://en.wikipedia.org/wiki/Single-page_application)) have become increasingly popular in recent years, as frameworks like Angular, Ember, and Backbone allow JavaScript developers to build modern web applications using techniques beyond vanilla JavaScript and jQuery. The three mentioned are among the first SPAs, each coming into its own between 2010 and 2011, but there are many more options for single-page development. The first generation of SPA frameworks arrived at the enterprise level, so their frameworks are more rigid. React, on the other hand, remains an innovative library that has been adopted by many technological leaders like [Airbnb, Netflix, and Facebook](https://github.com/facebook/react/wiki/Sites-Using-React).

React was released by Facebook's web development team in 2013 as a view library, which makes it the 'V' in the [MVC](https://en.wikipedia.org/wiki/Model–view–controller). If you haven't heard about MVC before, don't bother about it, because it's just there to put React historically into context for people who come from other programming languages.

As a view, React allows you to render components as viewable elements in a browser, while its ecosystem lets us build single page applications. While the first generation of frameworks tried to solve many things at once, React is only used to build your view layer; specifically, it is a library wherein the view is a hierarchy of composable components.

In React, the focus remains on the view layer until more aspects are introduced to the application. These are the building blocks for an SPA, which are essential to build a mature application. They come with two advantages:

* You can learn the building blocks one at a time without having to understand them altogether. In contrast, an SPA framework gives you every building block from the start. This book focuses on React as the first building block. More building blocks will eventually follow.

* All building blocks are interchangeable, which makes the ecosystem around React highly innovative. Multiple solutions can compete with each other, and you can choose the most appealing solution for any given challenge.

React is a great choice for building modern web applications. Even though it has a strong focus on components as a library, the surrounding ecosystem makes up an entirely flexible and interchangeable framework. React has a slim API, a robust and evolving ecosystem, and a welcoming community. We are happy to welcome you!

### Exercises

* Read more about [why I moved from Angular to React](https://www.robinwieruch.de/reasons-why-i-moved-from-angular-to-react/).
* Read more about [how to learn a framework](https://www.robinwieruch.de/how-to-learn-framework/).
* Read more about [how to learn React](https://www.robinwieruch.de/learn-react-js).
* Read more about [why framworks matter](https://www.robinwieruch.de/why-frameworks-matter).

## Requirements

To follow this book, you should be familiar with the basics of web development, i.e how to use HTML, CSS, and JavaScript. It also makes sense to understand [APIs](https://www.robinwieruch.de/what-is-an-api-javascript/), as they will be covered thoroughly.

### Editor and Terminal

I have provided [a setup guide](https://www.robinwieruch.de/developer-setup/) to get you started with general web development on your own machine. For this learning experience, you will need (A) a text editor (e.g. Sublime Text) and command line tool (e.g. iTerm) or (B) an IDE (e.g. Visual Studio Code) with integrated terminal. I highly recommend the latter (B), using Visual Studio Code, for getting started.

Throughout the learning experience I will use the term *command line* which will be synonymously used for the terms **command line tool*, *terminal*, and *integrated terminal*. The same applies for the terms *editor* and *IDE* depending on what you decided to use for your setup.

Optionally, I recommend you keep your projects in GitHub while conducting the exercises in this book. There is a [short guide](https://www.robinwieruch.de/git-essential-commands/) on how to use these tools. Github has excellent version control, so you can see what changes were made if you make a mistake or just want a more direct way to follow along.

If you don't want to have a whole setup on your machine or just want to fiddle around with the code online, you can use [CodeSandbox](https://codesandbox.io/) too. After every chapter, you will get a link to a CodeSandbox for the current application. However, even though CodeSandbox is a great tool for sharing, it would be best if you have a development setup on your machine too.

### Node and NPM

You will need an installation of [node and npm](https://nodejs.org/en/). Both are used to manage libraries (node packages) you will need along the way. In this learning experience, you will install external node packages via npm (node package manager). These node packages can be libraries or whole frameworks.

You can verify your versions of node and npm on the command line. If you don't get any output in the terminal, you need to install node and npm first. These are my versions at the time of writing this book:

```text
node --version
*vXX.YY.ZZ
npm --version
*vXX.YY.ZZ
```

If you have installed Node and NPM a while ago, please make sure that your installation isn't too old by updating it to a more recent version. Next, if you are not familiar with npm on the command line, take this [npm crash course](https://www.robinwieruch.de/npm-crash-course).

## Setting up a React Project

In the Road to React, we will be using [create-react-app](https://github.com/facebook/create-react-app) to bootstrap your application. It's an opinionated yet zero-configuration starter kit for React introduced by Facebook in 2016, [recommended for beginners by 96% of React users](https://twitter.com/dan_abramov/status/806985854099062785). In *create-react-app* the tooling and configuration evolve in the background, while the focus is on the application implementation.

To get started, after you have installed Node and NPM before, use your command line for typing the following command. The project will be referred to as *hacker-stories*, but you may choose any name you like:

```text
npx create-react-app hacker-stories
```

Then navigate into your new folder after the setup has finished:

```text
cd hacker-stories
```

Open the application in your editor or IDE. If you are using Visual Studio Code, you can simply type `code .` on the command line. The following folder structure, or a variation of it depending on the *create-react-app* version, should be presented to you:

```text
hacker-stories/
--node_modules/
--public/
--src/
----App.css
----App.js
----App.test.js
----index.css
----index.js
----logo.svg
--.gitignore
--package-lock.json
--package.json
--README.md
```

This is a breakdown of the most important folders and files:

* **README.md:** The *.md* extension indicates the file is a markdown file. Markdown is used as a lightweight markup language with plain text formatting syntax. Many source code projects come with a README.md file to give you initial instructions about the project. When pushing your project to a platform such as GitHub, the README.md file usually displays information about the content contained in the repository. Because you used create-react-app, your README.md should be the same as the official [create-react-app GitHub repository](https://github.com/facebook/create-react-app).
* **node_modules/:** This folder contains all node packages that have been installed via npm. Since you used create-react-app, there should already be a couple of node modules installed for you. You will rarely touch this folder, because node packages are generally installed and uninstalled with npm from the command line.
* **package.json:** This file shows you a list of node package dependencies and other project configurations.
* **package-lock.json:** This file indicates npm how to break down all node package versions.
* **.gitignore:** This file displays all files and folders that shouldn't be added to your git repository when using git; such files and folders should only be located in your local project. The *node_modules/* folder is one example. It is enough to share the *package.json* file with others, so they can install dependencies on their end with `npm install` without your dependency folder.
* **public/:** This folder holds development files, such as *public/index.html*. The index file is displayed on localhost:3000 when developing your app or on a doman when hosting it somewhere. The boilerplate takes care of relating this index with all the JavaScript from *src/*.

In the beginning, everything you need is located in the *src/* folder. The main focus lies on the *src/App.js* file which is used to implement React components. It will be used to implement your application, but later you might want to split up your components into multiple files, where each file maintains one or more components on its own.

Additionally, you will find a *src/App.test.js* file for your tests, and a *src/index.js* as an entry point to the React world. You will get to know both files intimately in later sections. There is also a *src/index.css* and a *src/App.css* file to style your general application and components, which comes with the default style when you open them. You will modify them later as well.

After you have learned about the folder and file structure of your React project, let's go through the available commands to get it started.

All your project specific commands can be found in your *package.json* under the *scripts* property. They may look similar to these:

```javascript
{
  ...
  },
  "scripts": {
    "start": "react-scripts start",
    "build": "react-scripts build",
    "test": "react-scripts test",
    "eject": "react-scripts eject"
  },
  ...
}
```

All of them can be executed with a `npm run <script>` as a command in your IDE integrated terminal or your external command line tool. We will go through all of them one by one:

```text
# Runs the application in http://localhost:3000
npm start

# Runs the tests
npm test

# Builds the application for production
npm run build
```

Another command from the previous npm scripts called `eject` shouldn't be used for this learning experience. It's a one way operation. Once you eject, you can't go back. Essentially this command is only there to make all the build tool and configuration from create-react-app accessible if you are not satified with the choices or if you want to change something. Here we will keep all the defaults though.

### Exercises:

* Read a bit more through React's [create-react-app](https://github.com/facebook/create-react-app) documentation.
* Go through all of your React project's folders and files one by one.
* Start your React application with `npm start` on the command line and check it out in the browser.
  * Exit the command on the command line by pressing `Control + C`.
* Every time we change something in our code throughout the coming learning experience, make sure to check the output in your browser for getting a visual feedback.
* Run the `npm test` script.
* Run the `npm run build` script and verify that a *build/* folder was added to your project (you can remove it afterward). Note that the build folder can be used later on to [deploy your application](https://www.robinwieruch.de/deploy-applications-digital-ocean/).

## Meet the React Component

In your *src/App.js* file, your first React component awaits you. It should look similar to the following one -- I say similar, because create-react-app changes the default component's structure from time to time.

```javascript
import React from 'react';
import logo from './logo.svg';
import './App.css';

function App() {
  return (
    <div className="App">
      <header className="App-header">
        <img src={logo} className="App-logo" alt="logo" />
        <p>
          Edit <code>src/App.js</code> and save to reload.
        </p>
        <a
          className="App-link"
          href="https://reactjs.org"
          target="_blank"
          rel="noopener noreferrer"
        >
          Learn React
        </a>
      </header>
    </div>
  );
}

export default App;
```

Throughout this learning experience, we will focus on this file. If we switch files, I will tell you. For now, let's cut down the the component to a more lightweight version to get to know React.

```javascript
import React from 'react';

function App() {
  return (
    <div>
      <h1>Hello World</h1>
    </div>
  );
}

export default App;
```

As you can see, this React component, called App component, is just a JavaScript function. Generally it's known under the name **function component**, because there are other variations of React components out there. You will learn more about these later (see **component types**).

The App component doesn't receive any parameters in its function signature. You will learn later how it can receive parameters though (see **props**). Also the App component returns something that resembles HTML. It's called JSX and we will learn about this soonish (see **JSX**).

The function component, since it is a function, can have implementation details in between like any other JavaScript function. You will see this in practice through the rest of our React journey.

```javascript{4}
import React from 'react';

function App() {
  // do something

  return (
    <div>
      <h1>Hello World</h1>
    </div>
  );
}

export default App;
```

Variables can be defined in the function's body, but will be re-defined every time this function runs again. After all, it behaves like a JavaScript function.

```javascript{4}
import React from 'react';

function App() {
  const title = 'React';

  return (
    <div>
      <h1>Hello World</h1>
    </div>
  );
}

export default App;
```

This means, as long as we don't need anything from within the App component (e.g. parameters coming from the function signature) that's used for this variable, we can define the variable outside of the App component too.

```javascript{3}
import React from 'react';

const title = 'React';

function App() {
  return (
    <div>
      <h1>Hello World</h1>
    </div>
  );
}

export default App;
```

Let's use this variable in the next section.

### Exercises:

* Confirm your [source code for the last section](https://codesandbox.io/s/github/the-road-to-learn-react/hackernews-client/tree/hs/Meet-the-React-Component).
* If you are unsure when to use `const`, `let` or `var` in JavaScript (or React) for variable declarations, make sure to [read more about their differences](https://www.robinwieruch.de/const-let-var).
* Think about ways on how to display the `title` variable in your App component's returned HTML. Don't worry if it doesn't work, you will learn it in the next section.

## React JSX

Previously, I have mentioned the returned output of the App component which looks very much like HTML. Actually it's called JSX which mixes HTML and JavaScript. Let's see how this works for displaying our variable.

```javascript{8}
import React from 'react';

const title = 'React';

function App() {
  return (
    <div>
      <h1>Hello {title}</h1>
    </div>
  );
}

export default App;
```

Once you start your application again, you will see the rendered variable in your browser. It should read: "Hello React".

Let's focus on the HTML for a moment. HTML is expressed almost the same in JSX like normal HTML. An input field with a label can be defined the following way.

```javascript{10-11}
import React from 'react';

const title = 'React';

function App() {
  return (
    <div>
      <h1>Hello {title}</h1>

      <label htmlFor="search">Search: </label>
      <input id="search" type="text" />
    </div>
  );
}

export default App;
```

We specified three HTML attributes here: `htmlFor`, `id`, and `type`. Whereas `id` and `type` should be familiar from HTML, `htmlFor` seems to be something new. Actually the `htmlFor` reflects the `for` attribute in HTML. JSX had to replace a handful of internal HTML attributes, but you can find all the [supported HTML attributes](https://reactjs.org/docs/dom-elements.html#all-supported-html-attributes) in React's documentation, which all follow the camelCase naming convention. On your way to learn React, expect to run across more JSX specific attributes like `className` and `onClick` instead of `class` and `onclick`.

We will revisit the HTML input field later for some actual implementation details. For now, let's get back to JavaScript in JSX. Before we have defined only a string primitive to be displayed in our App component. The same can be done with a JavaScript object:

```javascript{3-6,12}
import React from 'react';

const welcome = {
  greeting: 'Hey',
  title: 'React',
};

function App() {
  return (
    <div>
      <h1>
        {welcome.greeting} {welcome.title}
      </h1>

      <label htmlFor="search">Search: </label>
      <input id="search" type="text" />
    </div>
  );
}

export default App;
```

After all, everything in curly braces in JSX can be used for JavaScript expressions (e.g. function execution):

```javascript{3-5,10}
import React from 'react';

function getTitle(title) {
  return title;
}

function App() {
  return (
    <div>
      <h1>Hello {getTitle('React')}</h1>

      <label htmlFor="search">Search: </label>
      <input id="search" type="text" />
    </div>
  );
}

export default App;
```

JSX has been invented for React initially, but is used in other modern libraries and frameworks after it got popular. It's

### Exercises:

* Confirm your [source code for the last section](https://codesandbox.io/s/github/the-road-to-learn-react/hackernews-client/tree/hs/React-JSX).
* Read more about [React's JSX](https://reactjs.org/docs/introducing-jsx.html).
* Define more primitive and complex JavaScript data types and render them in JSX.
* Try to render a JavaScript array in JSX. If it's too complicated, don't worry, because you will learn more about this in the next section.

## Lists in React

So far, you have rendered a few primitive variables in your JSX. Now, we will render a list of items. The list will contain sample data in the beginning, but later we will learn how to fetch the data from an external API.

First, let's define the array as a variable. As we have learned before, we can define the variable outside or inside of the component. For now, we will do it outside of it.

```javascript{3-20}
import React from 'react';

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

function App() { ... }

export default App;
```

Each item in the list has a title, a url, and an author, as well an identifier (`objectID`), points -- which indicate the popularity of an item -- and a count of comments. Next we want to render this list dynamically within our JSX.

```javascript{4,9,11}
function App() {
  return (
    <div>
      <h1>My Hacker Stories</h1>

      <label htmlFor="search">Search: </label>
      <input id="search" type="text" />

      <hr />

      {/* render the list here */}
    </div>
  );
}
```

You can use the [built-in JavaScript map method for arrays](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/map) to iterate over each item of the list and to return a new version of each item.

```javascript
const numbers = [1, 4, 9, 16];

const newNumbers = numbers.map(function(number) {
  return number * 2;
});

console.log(newNumbers);
// [2, 8, 18, 32]
```

In our case, we will not map from one JavaScript data type to another one, but return a JSX fragment instead to render each item of our list.

```javascript{8-10}
function App() {
  return (
    <div>
      ...

      <hr />

      {list.map(function(item) {
        return <div>{item.title}</div>;
      })}
    </div>
  );
}
```

On a personal note, seeing this has been one of my first React ["Aha" moments](https://en.m.wikipedia.org/wiki/Eureka_effect). Being able to use bare bones JavaScript for mapping a list of JavaScript objects to HTML elements without any additional HTML templating syntax coming from React was just incredible. It's just plain JavaScript in HTML by using curly braces.

Now, React will display each item, but you can still do more to help React embrace its full potential for more advanced dynamic lists. By assigning a key attribute to each list item's element, React can identify modified items in case the list changes (e.g. re-ordering). Fortunately our list items come with an identifier:

```javascript{9-10,12-13}
function App() {
  return (
    <div>
      ...

      <hr />

      {list.map(function(item) {
        return (
          <div key={item.objectID}>
            {item.title}
          </div>
        );
      })}
    </div>
  );
}
```

Make sure that the key attribute is a stable identifier. Avoid using the index of the item in the array, because the array index is not stable. If the list changes its order, for example, React will not be able to identify the items properly and mess up things.

```javascript
// don't do this
{list.map(function(item, index) {
  return (
    <div key={index}>
      ...
    </div>
  );
})}
```

So far, only the title is displayed for each item. Let's experiment with displaying more of the item's properties:

```javascript{8-21}
function App() {
  return (
    <div>
      ...

      <hr />

      {list.map(function(item) {
        return (
          <div key={item.objectID}>
            <span>
              <a href={item.url}>{item.title}</a>
            </span>
            <span>{item.author}</span>
            <span>{item.num_comments}</span>
            <span>{item.points}</span>
          </div>
        );
      })}
    </div>
  );
}
```

The map function is concisley inlined in your JSX. Within the map function, we have access to each item which helps us to display its properties. In addition, the `url` property of each item is used as dynamic `href` attribute for the anchor tag. As you can see, not only can JavaScript in JSX be used to display things, but also to assign HTML attributes dynamically.

### Exercises:

* Confirm your [source code for the last section](https://codesandbox.io/s/github/the-road-to-learn-react/hackernews-client/tree/hs/Lists-in-React).
* Read more about [why React's key attribute is needed](https://www.robinwieruch.de/react-list-key) for rendering a list of items. Don't worry if you don't understand the implementation yet, just focus on what problem it causes for dynamic lists.
* Recap the [standard built-in array methods](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/) -- especially map, filter, reduce -- which are available in native JavaScript.
* What happens if your return `null` instead of the JSX?
* Extend the list with some more items to make the example more realistic.
* Use more JavaScript expressions on your own in JSX.

## Meet another React Component

We have met only one React component so far: the App component. We did quite some work in the last section in this component though by expressing everything that's needed for rendering our list in JSX. Eventually the App component will grow in size and will take care of things beyond its responsiblities. Hence, we will split out some of its responsibilities into a standalone List component.

```javascript{5-18}
const list = [ ... ];

function App() { ... }

function List() {
  return list.map(function(item) {
    return (
      <div key={item.objectID}>
        <span>
          <a href={item.url}>{item.title}</a>
        </span>
        <span>{item.author}</span>
        <span>{item.num_comments}</span>
        <span>{item.points}</span>
      </div>
    );
  });
}
```

Optional: If this component looks weird to you, because the outer most part of the retuened JSX starts with JavaScript, you *could* use it with a wrapping HTML element too. Afterward, we will continue with the previous version though.

```javascript{2-4,6-8}
function List() {
  return (
    <div>
      {list.map(function(item) {
        return (... );
      })}
    </div>
  );
}
```

This new List component can be used in the App component now.

```javascript{11}
function App() {
  return (
    <div>
      <h1>My Hacker Stories</h1>

      <label htmlFor="search">Search: </label>
      <input id="search" type="text" />

      <hr />

      <List />
    </div>
  );
}
```

You have created your first React component yourself! That's another milestone in learning React, and perhaps another "Aha" moment, because one can clearly see how this approach of using components -- which encapsulate a meaningful responsibility -- may turn out for larger React applications.

In a larger React application, one speaks of a **component hierarchy** or **component tree**. Usually there is one upper most **entry point component** (e.g. App component) which spans a tree of components below it. Whereas the App component is the **parent component** of the List component, the List component is a **child component** of the App component. The App component can have multiple child components and the List component can have child component as well. Hence the tree of components, whereas the App component could also be called **root component** and the components which don't render any other components themselves (e.g. List component) are called **leaf components**.

### Exercises:

* Confirm your [source code for the last section](https://codesandbox.io/s/github/the-road-to-learn-react/hackernews-client/tree/hs/Meet-another-React-Component).
* Draw your components -- the App component and List component -- as a component tree on a sheet of paper. Extend this component tree with other possible components (e.g. extracted Search component for the input field and label in the App component). What other parts could be extracted as standalone components?
* If there would be a Search component used in the App component, what would the Search component be for the List component and vice versa?
* Ask yourself: What problems could arise if we keep treating the `list` variable as global variable? We will overcome the problems that may come with this in one of the next sections (see props).

## React Component Instantiation

If you are new to React and JavaScript, let's go briefly into the concept of a JavaScript class for explaining the React component instantiation afterward. Technically they are not related, however, there is a fitting analogy between both for explaining the concept of component instantiation.

Classes are commonly used in object-oriented programming languages. JavaScript, always flexible in its programming paradigms, allows paradigms like functional programming and object-oriented programming to co-exist side-by-side. To recap JavaScipt classes, consider the following Developer class:

```javascript
class Developer {
  constructor(firstname, lastname) {
    this.firstname = firstname;
    this.lastname = lastname;
  }

  getName() {
    return this.firstname + ' ' + this.lastname;
  }
}
```

A class has a constructor to make create an instance of it. The constructor takes arguments and assigns them to the class instance. A class can also define functions. Because the function is associated to a subject (here: class), it is called a **method**, or a **class method**.

Defining the Developer class once is only one part of it. Actually using it by invoking it with the `new` statement is the other part of it. Whereas the class definition is the blueprint of the class' capabilities, the usage happens when an instance of it gets created.

```javascript
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
```

The behavior of defining a class and creating an instance of it is quite similar to a React component, which has only one component definition, but can have multiple instances of it:

```javascript{1,12-15,20-21}
// declaration of App component
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

// declaration of List component
function List() { ... }
```

Once you have declared a **component**, you can use it as an **element** anywhere in your JSX. It will produce an **instance** of your component or, in other words, the component gets instantiated.

### Exercises:

* Get comfortable with the terms component declaration, instance and element.
* Experiment with component instantiations yourself by creating multiple instances of a List component. Thought experiment: How would it be possible to give each List component its own list? We will learn leater about how to solve this.

## React DOM

You have learned about components declarations and their instantiation before. What about the App component's instantiation though. It has been there from beginning and we never questioned it. Let's check where it happens in another file: open up the *src/index.js* file:

```javascript
import React from 'react';
import ReactDOM from 'react-dom';

import App from './App';

ReactDOM.render(
  <App />,
  document.getElementById('root')
);
```

Next to React, there is another library called `react-dom`. It's `ReactDOM.render()` function uses a HTML node to replace it with JSX. It's a way to integrate React in any foreign web application easily, and you can use `ReactDOM.render()` multiple times across your application as well. We will have it only one time though.

`ReactDOM.render()` expects two arguments: The first argument is for rendering the JSX. In this case, it takes your App component, though it can also pass simple JSX.

```javascript
ReactDOM.render(
  <h1>Hello React World</h1>,
  document.getElementById('root')
);
```

The second argument specifies the place where the React application enters your HTML. It expects an element with an `id='root'`, found in the *public/index.html* file.

### Exercises:

* Open the *public/index.html* to see where the React application hooks into your HTML.
* Read more about [rendering elements in React](https://reactjs.org/docs/rendering-elements.html).

## React Component Definition (Advanced)

*Note: All the following refactorings are optional recommendations. They are just there to learn about the different JavaScript/React patterns; but if they give you a headache, don't bother too much about them. You can build React applications perfectly without these patterns. See it as a cherry on top when learning about advanced React.*

We have learned that we are dealing with React function components in our *src/App.js* file. Since every component there is a function, we can use a more concise version of defining it. JavaScript introduced arrow functions expressions, which are shorter than a function expressions:

```javascript
// function declaration
function () { ... }

// arrow function declaration
() => { ... }
```

You can remove the parentheses in an arrow function expression if it only has one argument, but you have to keep the parentheses if it gets multiple arguments:

```javascript
// allowed
item => { ... }

// allowed
(item) => { ... }

// not allowed
item, index => { ... }

// allowed
(item, index) => { ... }
```

Defining React function components with arrow functions makes them more concise. This holds also true for other functions like the function used within our JavaScript array's map method:

```javascript{1,7,9-10,22}
const App = () => {
  return (
    <div>
      ...
    </div>
  );
};

const List = () => {
  return list.map(item => {
    return (
      <div key={item.objectID}>
        <span>
          <a href={item.url}>{item.title}</a>
        </span>
        <span>{item.author}</span>
        <span>{item.num_comments}</span>
        <span>{item.points}</span>
      </div>
    );
  });
};
```

If an arrow function doesn't do anything in between, but only returns something like the App component, you can remove the **block body** (curly braces) of the function. In a **concise body**, an **implicit return statement** is attached; thus, you can remove the return statement:

```javascript{1,5,7-8,17}
const App = () => (
  <div>
    ...
  </div>
);

const List = () =>
  list.map(item => (
    <div key={item.objectID}>
      <span>
        <a href={item.url}>{item.title}</a>
      </span>
      <span>{item.author}</span>
      <span>{item.num_comments}</span>
      <span>{item.points}</span>
    </div>
  ));
```

Your JSX should look more concise and readable now, as it omits the function statement, the curly braces, and the return statement.

However, bear in mind that this was an optional step. There is nothing wrong keeping normal functions over arrow functions or keeping the block body with the curly braces for arrow functions. Often you need to move back to a block body anyway, if you want to introduce more business logic between function signature and return statement.

Anyway, make sure you understand this refactoring concept, because we will move swiftly from function components with block and without block body throughout the ongoing learning experience. You can keep it your way, but I will try to keep it as concise as possible depending on the requirements of the component.

### Exercises:

* Confirm your [source code for the last section](https://codesandbox.io/s/github/the-road-to-learn-react/hackernews-client/tree/hs/React-Component-Definition).
* Read more about [JavaScript arrow functions](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/Arrow_functions).
* Get comfortable with arrow functions with block with return and concise body with implicit return.

## Handler Function in JSX

The App component has still the input field and label which aren't really used yet. In HTML outside of JSX, an input field would have an [onchange handler](https://developer.mozilla.org/en-US/docs/Web/API/GlobalEventHandlers/onchange). You will learn how to use this in a React component's JSX. First, we will refactor the App component back form a concise to a block body in order to add implementation details in between.

```javascript{1-4,15-16}
const App = () => {
  // do something

  return (
    <div>
      <h1>My Hacker Stories</h1>

      <label htmlFor="search">Search: </label>
      <input id="search" type="text" />

      <hr />

      <List />
    </div>
  );
};
```

And second, we will define a function, be it normal function or arrow function, for the change event of the input field. The function can be passed to the `onChange` attribute (JSX named attribute) of the input field afterward.

```javascript{2-4,11}
const App = () => {
  const handleChange = event => {
    console.log(event);
  };

  return (
    <div>
      <h1>My Hacker Stories</h1>

      <label htmlFor="search">Search: </label>
      <input id="search" type="text" onChange={handleChange} />

      <hr />

      <List />
    </div>
  );
};
```

After starting your application and checking it in the browser, open your browser's developer tools to see the logging happen after typing something into the input field. What you are seeing there is called a synthetic event in React. It's possible to access the value of the input field through it:

```javascript{3}
const App = () => {
  const handleChange = event => {
    console.log(event.target.value);
  };

  return ( ... );
};
```

The synthetic event is not much more than a wrapper around the browser's native event, but comes with some more functions which will be useful to prevent some native browser behavior (e.g. refreshing a page after clicking on a form's submit button).

After all, that's how you can give HTML elements in JSX a handler function to do something if a user interacts with the element. Make sure to always pass a function to these handlers and not the return value of the function.

```javascript{5,12}
// don't do this
<input
  id="search"
  type="text"
  onChange={handleChange()}
/>

// do this instead
<input
  id="search"
  type="text"
  onChange={handleChange}
/>
```

One can see again how HTML and JavaScript work perfectly together in JSX. The JavaScript in HTML can not only be used to display something, or to pass a JavaScript primitive to a HTML attribue (e.g. `href` to `<a>`), but also to pass functions to an element's attributes.

### Exercises:

* Confirm your [source code for the last section](https://codesandbox.io/s/github/the-road-to-learn-react/hackernews-client/tree/hs/Handler-Function-in-JSX).
* Read more about [React's events](https://reactjs.org/docs/events.html).

## React Props

At the moment, we are using the `list` as a global variable. We used it directly from the global scope in the App component, then we used it directly in the List component. This may seem okay for this one variable, but it doesn't scale very well when using multiple variables. There needs to be a way to initialize a variable in one component and pass it as information to another component. Entering props in React.

First, let's move the list from the global scope into the App component and let's rename it to its actual domain:

```javascript{2-19}
const App = () => {
  const stories = [
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

  const handleChange = event => { ... };

  return ( ... );
};
```

And second, we will be using **React props** to pass the array to the List component.

```javascript{15}
const App = () => {
  const stories = [ ... ];

  const handleChange = event => { ... };

  return (
    <div>
      <h1>My Hacker Stories</h1>

      <label htmlFor="search">Search: </label>
      <input id="search" type="text" onChange={handleChange} />

      <hr />

      <List list={stories} />
    </div>
  );
};
```

The variable is called `stories` in the App component, and as `stories` we pass it to the List component, howevever, there it's assigned to the `list` attribute. Hence, we can access it as `list` from the `props` object in our List compoent's function signature.

```javascript{1-2}
const List = props =>
  props.list.map(item => (
    <div key={item.objectID}>
      <span>
        <a href={item.url}>{item.title}</a>
      </span>
      <span>{item.author}</span>
      <span>{item.num_comments}</span>
      <span>{item.points}</span>
    </div>
  ));
```

That's it. We have moved the list/stories variable from the global scope into the App component. This avoids polluting the global scope of the application. Since the `stories` are not directly used in App component, but in one of its child components, we passed them as props to the List component where we have access to them through the first function signature's argument called `props`.

* Confirm your [source code for the last section](https://codesandbox.io/s/github/the-road-to-learn-react/hackernews-client/tree/hs/React-Props).
* Read more about [how to pass props to React components](https://www.robinwieruch.de/react-pass-props-to-component).

## React State

Whereas React Props are used to pass information down the component tree, React State is used to make your application interactive. Suddenly you will be able to change your application's appearance by interacting with it.

First, there is a utility function called `useState` that we take from React for managing state. The `useState` function is called a hook in React. There are more than one **React hook** -- related to state management but also other things in React -- and you will learn about them later. For now, let's focus on React's `useState` hook:

```javascript{4}
const App = () => {
  const stories = [ ... ];

  const [searchTerm, setSearchTerm] = React.useState('');

  ...
};
```

React's `useState` hook takes an *initial state* as argument. In our case, we are using an empty string. Then the function returns an array with two values. While the first value (`searchTerm`) represents the *current state*, the second value is a *function to update this state* (`setSearchTerm`).

If you are not familiar with the syntax of the two values from the returned array, it's called [JavaScript array destructuring](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Destructuring_assignment#Array_destructuring):

```javascript
const list = ['a', 'b'];

// no array destructuring
const itemOne = list[0];
const itemTwo = list[1];

// array destructuring
const [itemOne, itemTwo] = list;
```

After we have initialized the state and have access to the current state and the state updater function, use them to display the current state and to update the state in the App component.

```javascript{7,17-19}
const App = () => {
  const stories = [ ... ];

  const [searchTerm, setSearchTerm] = React.useState('');

  const handleChange = event => {
    setSearchTerm(event.target.value);
  };

  return (
    <div>
      <h1>My Hacker Stories</h1>

      <label htmlFor="search">Search: </label>
      <input id="search" type="text" onChange={handleChange} />

      <p>
        Searching for <strong>{searchTerm}</strong>.
      </p>

      <hr />

      <List list={stories} />
    </div>
  );
};
```

The current state can be used to display it somewhere. Now, every time someone types into the input field, the change event of the input field is captured by the handler. The handler's logic in return uses the state updater function to set the state. After new state has been set in a component, it renders again. The new state becomes the current state.

### Exercises:

* Confirm your [source code for the last section](https://codesandbox.io/s/github/the-road-to-learn-react/hackernews-client/tree/hs/React-State).
* Read more about [JavaScript array destructuring](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Destructuring_assignment#Array_destructuring).
* Read more about React's useState Hook ([0](https://www.robinwieruch.de/react-usestate-hook), [1](https://reactjs.org/docs/hooks-state.html)) and double down on it, because it's what makes your React components dynamic.

## Callback Handler in JSX

Shift your attention to the input field and label, by splitting out a standalone Search component, and creating an instance of it in the App component. The Search component becomes a sibling component of the List component and vice versa. You will also have to move the handler and the state into the Search component.

```javascript{8,17-34}
const App = () => {
  ...

  return (
    <div>
      <h1>My Hacker Stories</h1>

      <Search />

      <hr />

      <List list={stories} />
    </div>
  );
};

const Search = () => {
  const [searchTerm, setSearchTerm] = React.useState('');

  const handleChange = event => {
    setSearchTerm(event.target.value);
  };

  return (
    <div>
      <label htmlFor="search">Search: </label>
      <input id="search" type="text" onChange={handleChange} />

      <p>
        Searching for <strong>{searchTerm}</strong>.
      </p>
    </div>
  );
};
```

The Search component handels state itself without letting someone else know about it. It just displays the `searchTerm` as text but doesn't share this information with its parent nor sibling component. How do we change this in order to filter the list in the App component before it gets passed to the List component? Without this, the Search component would be useless. In other words: If we can pass information down with React's props, how do we pass information up in React?

There is no way to pass information as JavaScript data types up the component tree. Props are naturally only passed downwards. However, we can introduce a so called **callback handler** as a function: A callback function gets introduced somewhere (A), is used somewhere else (B), but "calls back" to its origin where it go introduced (C).

```javascript{4-8,14,23,29}
const App = () => {
  const stories = [ ... ];

  // A
  const handleSearch = event => {
    // C
    console.log(event.target.value);
  };

  return (
    <div>
      <h1>My Hacker Stories</h1>

      <Search onSearch={handleSearch} />

      <hr />

      <List list={stories} />
    </div>
  );
};

const Search = props => {
  const [searchTerm, setSearchTerm] = React.useState('');

  const handleChange = event => {
    setSearchTerm(event.target.value);

    props.onSearch(event);
  };

  return ( ... );
};
```

Omit the comments A, B, and C in your implementation, they are just there to visualize the statement from before. Anyway, let this concept of a callback handler sink for a moment. We pass a function from one component (App) to another component (List), use it in the second component (List) but get -- and eventually use -- the actual callback of the function call in the first component (App). This way, we are able to communicate up the component tree. A handler function which is just used in one component becomes a callback handler which is passed down to a single or multiple components.

In conclusion, whereas React props are always passed down as information the component tree, callback handlers passed as functions in props can be used to communicate up the component tree.

### Exercises:

* Confirm your [source code for the last section](https://codesandbox.io/s/github/the-road-to-learn-react/hackernews-client/tree/hs/Callback-Handler-in-JSX).
* Go through the concepts of handler and callback handler one more time yourself.

## Lifting State in React

So far, the Search component still has its interal state. Even though we have established a callback handler to pass information up the component tree to the App component, we are not doing anything about it yet. If all state for the search feature is handled in the Search component, how can the App and List component know about it? We have to **lift the state up**; from Search to App component:

```javascript{4,7,14,26}
const App = () => {
  const stories = [ ... ];

  const [searchTerm, setSearchTerm] = React.useState('');

  const handleSearch = event => {
    setSearchTerm(event.target.value);
  };

  return (
    <div>
      <h1>My Hacker Stories</h1>

      <Search onSearch={handleSearch} />

      <hr />

      <List list={stories} />
    </div>
  );
};

const Search = props => (
  <div>
    <label htmlFor="search">Search: </label>
    <input id="search" type="text" onChange={props.onSearch} />
  </div>
);
```

Conveniently we have learned about the callback handler in a previous section. This one helps us to communicate from the Search component up to the App to update the state in the App component. If you would want, you could also display the `searchTerm` again -- either in the App component or in the Search component by passing it down as prop again.

Rule of thumb: Manage the state at a component where every component that's interested in the state is either the component that manages the state (state) or a component below the managing component (props). If a component below needs to update the state, pass down a callback handler.

Now, by managing the Search state in the App component, we can finally filter the list with the stateful `searchTerm` before passing it to the List component:

```javascript{10-12,22}
const App = () => {
  const stories = [ ... ];

  const [searchTerm, setSearchTerm] = React.useState('');

  const handleSearch = event => {
    setSearchTerm(event.target.value);
  };

  const searchedStories = stories.filter(function(story) {
    return story.title.includes(searchTerm);
  });

  return (
    <div>
      <h1>My Hacker Stories</h1>

      <Search onSearch={handleSearch} />

      <hr />

      <List list={searchedStories} />
    </div>
  );
};
```

Here the [JavaScript array's built-in filter function](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/filter) is used to create a new filtered array. The filter function takes a function as argument; which has access to each item in the array and returns true or false for each of them. If the function returns true, meaning the condition is met, the item stays in the newly created array. If the function returns false, it's removed from the new array.

```javascript
const words = [
  'spray',
  'limit',
  'elite',
  'exuberant',
  'destruction',
  'present'
];

const filteredWords = words.filter(function(word) {
  return word.length > 6;
});

console.log(filteredWords);
// Array ["exuberant", "destruction", "present"]
```

The filter function doesn't work properly yet. It's true that it checks whether the `searchTerm` is anywhere present in our story item's title, however, it's too opinionated about the letter case. Try to search for "react" and you will not get the filtered "React" story in your rendered list. In order to fix this problem, we have to lower case the story's title and the `searchTerm`.

```javascript{5-7}
const App = () => {
  ...

  const searchedStories = stories.filter(function(story) {
    return story.title
      .toLowerCase()
      .includes(searchTerm.toLowerCase());
  });

  ...
};
```

If you try the feature in your browser, you should be able to search for "eact", "React", or "react" and see one of two displayed stories. You have made it! Your application has an impressive interactive feature now.

The remaining section shows a couple of refactoring steps you *could* make, but which are not mandatory. They are only there to show you your options. We will keep the final refactored version in the end though; so it makes sense to understand the steps and also to keep them.

As learned before, we can make the function more concise by using a JavaScript arrow function.

```javascript{4}
const App = () => {
  ...

  const searchedStories = stories.filter(story => {
    return story.title
      .toLowerCase()
      .includes(searchTerm.toLowerCase());
  });

  ...
};
```

In addition, we could turn the return statement into an immediate return, because no business logic happens before the return:

```javascript{4-6}
const App = () => {
  ...

  const searchedStories = stories.filter(story =>
    story.title.toLowerCase().includes(searchTerm.toLowerCase())
  );

  ...
};
```

That's all to the refactoring steps of the inlined filter function. There are many variations to it -- and it's not always simple to keep a good balance between reaable and conciseness -- however, I feel like keeping it concise whenever possible keeps it most of the time readable as well.

After all, you have learned how to manipulate state in React. You have used the Search component's callback handler in the App component to update state. The current state is used as a filter for the list. Since we had the callback handler in place, we were able to use the information of the Search component in the App component and indirectly in the List component for the filtered list.

### Exercises:

* Confirm your [source code for the last section](https://codesandbox.io/s/github/the-road-to-learn-react/hackernews-client/tree/hs/Lifting-State-in-React).

## React Controlled Components

Controlled components are not neccessary React components but HTML elements in general. Here you will learn how to turn the Search component and its input field into a controlled component.

Let's go through a scenario that illustrates why we should follow the concept of controlled components throughout our entire React application. After applying the following change -- giving the `searchTerm` an initial state --, can you spot the mistake in your browser?

```javascript{4}
const App = () => {
  const stories = [ ... ];

  const [searchTerm, setSearchTerm] = React.useState('react');

  ...
};
```

Although the list has been filtered accordingly to the initial search, the input field doesn't show the initial `searchTerm`. The desired behaviour would be to have the input field reflecting the actual `searchTerm` used from the initial state. But it's only reflected through the filtered list.

We need to convert the Search component with its input field into a controlled component. So far, the input field doesn't know anything about the `searchTerm`, it only uses the change event to let us know about a change. However, actually the input field has a `value` attribute.

```javascript{12,25}
const App = () => {
  const stories = [ ... ];

  const [searchTerm, setSearchTerm] = React.useState('react');

  ...

  return (
    <div>
      <h1>My Hacker Stories</h1>

      <Search search={searchTerm} onSearch={handleSearch} />

      ...
    </div>
  );
};

const Search = props => (
  <div>
    <label htmlFor="search">Search: </label>
    <input
      id="search"
      type="text"
      value={props.search}
      onChange={props.onSearch}
    />
  </div>
);
```

Now the input field should start off with the correct initial state for the `searchTerm`. Also when we change the `searchTerm`, we force the input field to always use the value from React's state (via props). Before, the input field managed natively (HTML spec) its own internal state.

We didn't only learn about controlled components here, but also, taking all the previous sections as learning steps into consideration, another concept called **unidirectional data flow**.

```javascript
UI -> Side-Effect -> State -> UI -> ...
```

A React application and its components start off with an initial state which may be or be not passed down as props to other components. It's rendered for the first time. Once a side-effect happens, like a user interacting with an input field or data gets loaded from a remote API, the change is captured in React's state. Once state has been changed, all the components affected by the modified state or the implictly modified props are re-rendering.

In the previous sections, you have also learned something important about React's component lifecycle. In the beginning, all components get instantiated from top to bottom of the component hierarchy. This includes all hooks (e.g. `useState`) which are instantiated with their initial values (e.g. initial state). From there, the UI awaits side-effects like user interactions. Once state is changed somewhere (e.g. current state changed via state updater function from `useState`), all components affected by modified state/props render again.

Every run through a component's function takes the *recent value* (e.g. current state) from the hooks and *doesn't* reinitialize them again (e.g. initial state). This seems like an odd behavior, because it would be just normal to assume that the `useState` hooks function just re-initializes again with its initial value. But it doesn't. Hooks initialize only once when the component renders for the first time. Afterward, they React tracks them internally with their recent values.

### Exercises:

* Confirm your [source code for the last section](https://codesandbox.io/s/github/the-road-to-learn-react/hackernews-client/tree/hs/React-Controlled-Components).
* Read more about [controlled components in React](https://www.robinwieruch.de/react-controlled-components/).
* Play around with uses of `console.log()` in your React components and see who renders when; initially and after the input field has been changed.

## Props Handling (Advanced)

Props are passed from parent to child down the component tree. Since we are dealing all the time with props to transport information from A to B, sometimes via component C, it would be great to know some tricks on how to make this more convenient for us.

*Note: All the following refactorings are optional recommendations. They are just there to learn about the different JavaScript/React patterns; but if they give you a headache, don't bother too much about them. You can build React applications perfectly without these patterns. See it as a cherry on top when learning about advanced React.*

React props are an JavaScript object, otherwise one wouldn't be able to access `props.list` or `props.onSearch` in our React components. Since `props` is an object, we can apply one or two JavaScript tricks on it.

```javascript
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
```

This feature is called [object destructuring in JavaScript](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Destructuring_assignment). If one needs to access multiple properties of an object, it can be more elegant accessing them in one line of code instead of multiple lines. Let's transfer this knowledge to React props in our Search component:

```javascript{2,10-11}
const Search = props => {
  const { search, onSearch } = props;

  return (
    <div>
      <label htmlFor="search">Search: </label>
      <input
        id="search"
        type="text"
        value={search}
        onChange={onSearch}
      />
    </div>
  );
};
```

That's the bare bones destructuring of the `props` object in a React component, so that its properties can be used in the component without using the `props` object every time.

However, now we had to refactor the Search component's arrow function from concise body into block body; just to access the properties of `props` in between. This will happen quite often if we follow this pattern. In case of the destructuring the `props`, let's take it one step further by destructuring the `props` right away in the function signature and thus ommitting the block body again:

```javascript{1,7-8}
const Search = ({ search, onSearch }) => (
  <div>
    <label htmlFor="search">Search: </label>
    <input
      id="search"
      type="text"
      value={search}
      onChange={onSearch}
    />
  </div>
);
```

Only rarely `props` themselves are used in React components. It's rather all the information coming with the `props` object used as a container that's actually used in the component. By destructuring the `props` object right away in the function signature, we can conventienly access all information without dealing with the container of this information.

Let's deal with another scenario where its less about the `props` object but more about an object that comes from it. The same lessons learned will be applicable for the `props` object as well though. First, we will extract a new Item component from the List component:

```javascript{1-2,4-13}
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
```

We already applied the previously lesson learned by destructuring the `props` object in the function signature of each component. Let's take it one step further, because the incoming `item` of the Item component can be seen the same way as the `props`, because it's never directly used in the Item component.

There are three potential versions of handling this situation. The first version would be to perform a *nested destructuring* in the component's function signature:

```javascript{3-9,13,15-17}
// version 1 (final)
const Item = ({
  item: {
    title,
    url,
    author,
    num_comments,
    points,
  },
}) => (
  <div>
    <span>
      <a href={url}>{title}</a>
    </span>
    <span>{author}</span>
    <span>{num_comments}</span>
    <span>{points}</span>
  </div>
);
```

Nested destructuring introduces lots of clutter through indendations in the function signature though. Therefore it's not always the most readable option, but in some situations it's useful to know about it.

Let's go through the second version of handling this situation:

```javascript{6-10,14}
// version 2 (I)
const List = ({ list }) =>
  list.map(item => (
    <Item
      key={item.objectID}
      title={item.title}
      url={item.url}
      author={item.author}
      num_comments={item.num_comments}
      points={item.points}
    />
  ));

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
```

Can you spot the problem? Even though the Item component's function signature is more concise again, all the clutter ended up in the List component instead, because every property is passed individually to the Item component. Let's improve this version by using [JavaScript's spread operator](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Spread_syntax):

```javascript
const person = {
  firstName: 'Robin',
  lastName: 'Wieruch',
};

const details = {
  year: 1988,
  country: 'Germany',
};

const personWithDetails = { ...person, ...details };

console.log(personWithDetails);
// {
//   firstName: "Robin",
//   lastName: "Wieruch",
//   year: 1988,
//   country: "Germany"
// }
```

Instead of passing each property one by one via props from List to Item component, one could use JavaScript's spread operator for passing all the object's key/value pairs as attribute/value pairs to the JSX element:

```javascript{3}
// version 2 (II)
const List = ({ list }) =>
  list.map(item => <Item key={item.objectID} {...item} />);

const Item = ({ title, url, author, num_comments, points }) => (
  ...
);
```

Last but not least, there is one more little improvement left in this version to make it whole. One could use [JavaScript's rest parameters](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/rest_parameters):

```javascript
const person = {
  firstName: 'Robin',
  lastName: 'Wieruch',
  year: 1988,
};

const { year, ...personWithoutYear } = person;

console.log(personWithoutYear);
// {
//   firstName: "Robin",
//   lastName: "Wieruch"
// }
```

The JavaScript rest operator is the last part in an object destructuring. It shouldn't be mistaken with the spread operator even though it has the same syntax. Usually the rest operator happens on the right side of an assignment and the spread operator on the left side on an assignment. Now it could be used in our List component to separate the `objectID` from the item, because the `objectID` is only used as `key` and isn't used (so far) in the Item component. Only the remaining (rest) item gets spreaded as attribute/value pairs into the Item component (as before):

```javascript{3-4}
// version 2 (III/final)
const List = ({ list }) =>
  list.map(({ objectID, ...item }) => (
    <Item key={objectID} {...item} />
  ));
```

Even though this version is very concise, it comes with all these advanced JavaScript features which may be not known by everyone. A lot of fancy stuff happens in just a few lines. One needs to ask oneself: Is less always better?

The third and last way of handling this siutation, would be keeping it like it has been before:

```javascript
// version 3 (final)
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
```

Which is on a personal note my favorite way of doing it when working with other people on a React project. It's not as concise as version 2, but it's more readable, gives the Item component a smaller API surface, and doesn't add too many fancy JavaScript features (spread operator, rest operator). But that's just a personal opinion and maybe your taste differs.

All these versions have their trade-offs. When refactoring a component, you should always aim for readability, especially when working in a team of people. If working with a team, make sure everyone is one the same page for a common React code style. We went through several versions for dealing with props now, so it's up to you which version suits you best.

### Exercises:

* Confirm your [source code for the last section](https://codesandbox.io/s/github/the-road-to-learn-react/hackernews-client/tree/hs/Props-Handling).
* Read more about [JavaScript's destructuring assignment](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Destructuring_assignment).
* Think about the difference(s) of JavaScript array destructuring -- which we have used for React's `useState` hook -- and object destructuring.
* Read more about [JavaScript's spread operator](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Spread_syntax).
* Read more about [JavaScript's rest parameters](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/rest_parameters).
* Get a good sense about what's related to JavaScript (e.g. spread operator, rest parameters, destructuring) and what's related to React (e.g. props) from the last lessons learned.
* Continue to use your favorite way of dealing with React's props. If undecided, take the last version used in the previous section.

## React Side-Effects

There is a neat little feature we could add to our Search component to learn conveniently about another React hook. What if the Search component could remember the most recent search interaction and the application would open up with it in the browser after starting the application for another time?

First, we use the local storage of the browser to store the `searchTerm` accompanied by an identifier. And second, we would use the stored value, but only if there is a value, to set the initial state of the `searchTerm`. Otherwise the initial state will default to an empty string as before:

```javascript{5,11}
const App = () => {
  ...

  const [searchTerm, setSearchTerm] = React.useState(
    localStorage.getItem('search') || ''
  );

  const handleSearch = event => {
    setSearchTerm(event.target.value);

    localStorage.setItem('search', event.target.value);
  };

  ...
);
```

When using the input field and refreshing the browser tab, the browser should remember the latest search term now. Using the local storage in React can be seen as a side-effect, because we interact outside of React's domain by using the browser's API.

There is one flaw though. The handler should be mainly concerned about its primary objective: updating the state. But now it got this side-effect in there. If we would use the `setSearchTerm` at another place in our application, we would break the previously implemented feature.

Let's fix this by dealing with the side-effect at a dedicated place. We will use React's useEffect Hook to trigger the side-effect every time the `searchTerm` changes:

```javascript{8-10}
const App = () => {
  ...

  const [searchTerm, setSearchTerm] = React.useState(
    localStorage.getItem('search') || ''
  );

  React.useEffect(() => {
    localStorage.setItem('search', searchTerm);
  }, [searchTerm]);

  const handleSearch = event => {
    setSearchTerm(event.target.value);
  };

  ...
);
```

React's useEffect Hook takes two argument: The first argument is a function where the side-effect takes place. In our case, storing the `searchTerm` into the browser's local storage. The second argument is a dependency array of variables. If one variable changes, the function for the side-effect is called. In our case, the function is called every time the `searchTerm` changes.

If the dependency array of React's useEffect is an empty array, the function for the side-effect is only called once after the component has been rendered for the first time. After all, the hook gives one control to opt-in into React's component lifecycyle. It can be triggered when mounting the component for the first time, but also when updating one of its dependencies.

The last change of using React `useEffect` instead of dealing with the side-effect in the handler has made the application more robust. Because *whenever* and *wherever* `searchTerm` is updated via `setSearchTerm`, one can be assured that the local storage will be kept in sync with it.

### Exercises:

* Confirm your [source code for the last section](https://codesandbox.io/s/github/the-road-to-learn-react/hackernews-client/tree/hs/React-Side-Effects).
* Read more about React's useEffect Hook ([0](https://reactjs.org/docs/hooks-effect.html), [1](https://reactjs.org/docs/hooks-reference.html#useeffect)).
* Give the first argument's function a `console.log()` and play around with React's useEffect Hook's dependency array. Check the logging for an empty dependency array too.

## React Custom Hooks (Advanced)

So far, you have learned about two hooks in React: useState and useEffect. They are the most popular ones. Whereas the former is used to make your application interactive, the latter is a way to opt-in into the lifecycle of your components.

But these hooks are not the only ones. Eventually you will get to know other hooks coming with React -- on this learning journey or from other resources -- because we don't cover all of them exhaustively. However, let's take time to learn about another important aspect of React hooks: building custom hooks yourself.

We will use the two hooks that we already have in place to create a new custom hook called `useSemiPersistentState`. It's not fully persistent, because clearing the local storage of the browser would delete also the relevant data from local storage for this application.

Let's start by extracting all relevant implementation details into this new custom hook:

```javascript{1-9}
const useSemiPersistentState = () => {
  const [searchTerm, setSearchTerm] = React.useState(
    localStorage.getItem('search') || ''
  );

  React.useEffect(() => {
    localStorage.setItem('search', searchTerm);
  }, [searchTerm]);
};

const App = () => {
 ...
};
```

So far, it's just a function around our previously in App component used `useState` and `useEffect` hooks. Before we can use it, let's return the values needed in our App component from this custom hook:

```javascript{10}
const useSemiPersistentState = () => {
  const [searchTerm, setSearchTerm] = React.useState(
    localStorage.getItem('search') || ''
  );

  React.useEffect(() => {
    localStorage.setItem('search', searchTerm);
  }, [searchTerm]);

  return [searchTerm, setSearchTerm];
};
```

We are following two conventions of React's built-in hooks here. First, the naming convention which puts the "use" prefix in front of every hook name. And second, the returned values are returned as an array. Now we can use the custom hook with its returned values in the App component:

```javascript{4}
const App = () => {
  const stories = [ ... ];

  const [searchTerm, setSearchTerm] = useSemiPersistentState();

  const handleSearch = event => {
    setSearchTerm(event.target.value);
  };

  const searchedStories = stories.filter(story =>
    story.title.toLowerCase().includes(searchTerm.toLowerCase())
  );

  return (
    ...
  );
};
```

However, one goal of a custom hook should be reusability. All of this custom hook's internals are still considered about the search domain. But the hook should be just about a value which is set in state and also synchronize in local storage. Let's adjust the namings therefore:

```javascript{2-3,7-8,10}
const useSemiPersistentState = () => {
  const [value, setValue] = React.useState(
    localStorage.getItem('value') || ''
  );

  React.useEffect(() => {
    localStorage.setItem('value', value);
  }, [value]);

  return [value, setValue];
};
```

Even though we just deal with an abstracted "value" within the custom hook, when using it in the App component, we can name the returned value and updater function anything domain related (e.g. `searchTerm` and `setSearchTerm`) due to the array destructuring. Hence the design decision to let React hooks return arrays.

There is one thing still off with this custom hook. Using the custom hook more than one time in a React application would lead to an overwrite of the "value"-allocated item in the local storage if used from multiple origins. Hence we want to pass in a key ourselves.

```javascript{1,3,7-8,17}
const useSemiPersistentState = key => {
  const [value, setValue] = React.useState(
    localStorage.getItem(key) || ''
  );

  React.useEffect(() => {
    localStorage.setItem(key, value);
  }, [value, key]);

  return [value, setValue];
};

const App = () => {
  ...

  const [searchTerm, setSearchTerm] = useSemiPersistentState(
    'search'
  );

  ...
};
```

Since the key comes from the outside, the custom hook assumes that it could change and that's why it needs to be included as dependency in the dependency array of the `useEffect` hook. Otherwise the side-effect would run with an outdated key the next time.

Another improvement would be giving the custom hook the initial state from the outside:

```javascript{1,3,13-14}
const useSemiPersistentState = (key, initialState) => {
  const [value, setValue] = React.useState(
    localStorage.getItem(key) || initialState
  );

  ...
};

const App = () => {
  ...

  const [searchTerm, setSearchTerm] = useSemiPersistentState(
    'search',
    'React'
  );

  ...
};
```

We are done with the custom hook. Extracting this functionality into a custom hook was a welcome exercise to create a hook yourself. If you are not comfortable with it, you can revert the changes and use the `useState` and `useEffect` hook as before in the App component.

However, knowing more about custom hooks gives you lots of new options. A custom hook can encapsulate non trivial implementation details that should be kept away from a component, can be used in more than one React component, and can even be open sourced as a external library. If you search around with your favorite search engine, you will see that there are already hundreds of React hooks out that could be used in your application without you worrying about its implementation details. You could use their features right away.

### Exercises:

* Confirm your [source code for the last section](https://codesandbox.io/s/github/the-road-to-learn-react/hackernews-client/tree/hs/React-Custom-Hooks).
* Read more about [React Hooks](https://www.robinwieruch.de/react-hooks) to get a good understanding of them. They are the bread and butter in React function components, so it's important to double down ([0](https://reactjs.org/docs/hooks-overview.html), [1](https://reactjs.org/docs/hooks-custom.html)) on them.

## React Fragments

You may have noticed one caveat when rendering JSX, maybe especially when we created a dedicated Search component. There, we had to introduce a wrapping HTML element for being able to render it.

```javascript
const Search = ({ search, onSearch }) => (
  <div>
    <label htmlFor="search">Search: </label>
    <input
      id="search"
      type="text"
      value={search}
      onChange={onSearch}
    />
  </div>
);
```

Normally the JSX returned by a React component has to have only one wrapping top-level element. If we would want to render multiple top-level elements side by side, we would have to wrap them into an array instead. Since it's a list of elements, we have to give every sibling element React's infamous `key` attribute:

```javascript{1-4,6,11-12}
const Search = ({ search, onSearch }) => [
  <label key="1" htmlFor="search">
    Search:{' '}
  </label>,
  <input
    key="2"
    id="search"
    type="text"
    value={search}
    onChange={onSearch}
  />,
];
```

This would be one way of having multiple top-level elements in your JSX. However, most often it doesn't turn out very readable and verbose with the additional key attributes. Another solution would be using a React fragment:

```javascript{2,10}
const Search = ({ search, onSearch }) => (
  <>
    <label htmlFor="search">Search: </label>
    <input
      id="search"
      type="text"
      value={search}
      onChange={onSearch}
    />
  </>
);
```

A fragment wraps other elements while it doesn't add anything to the actual rendered output. Opening your browser for inspecting the elements should result into seeing both Search element's, input field and label, side by side without any other element. After all, if you don't like to have the wrapping `<div>` or `<span>` elements, you can substitute them with an empty tag which are allowed in JSX and which don't introduce any intermediate element in your rendered HTML.

### Exercise:

* Confirm your [source code for the last section](https://codesandbox.io/s/github/the-road-to-learn-react/hackernews-client/tree/hs/React-Fragments).
* Read more about [React fragments](https://reactjs.org/docs/fragments.html).

## Reusable React Component

Have a closer look at the Search component. What does the component have that's related to the domain of search? The label element got the text "Search: ", the id/htmlFor attributes have both the `search` identifier, the value is called `search`, and the callback handler is called `onSearch`.

So far, it's pretty much tied to some search feature which makes it less reusable for the rest of the application except if we would implement a search feature somewhere else. Also then it would introduce a bug if two of these Search components would be rendered side by side, because the htmlFor/id combination would be duplicated which would break the focus on label click.

Since the Search component doesn't have any actual search functionality, it's almost no effort to generalize the other search domain tied properties in order to make the component more valuable for the rest of the application as a more reusable component.

Let's pass an additional `id` and `label` prop to the Search component, rename the actual value and callback handler to something more abstract, and finally rename the component itself accordingly:

```javascript{8-10,12,20,22-23,25,28}
const App = () => {
  ...

  return (
    <div>
      <h1>My Hacker Stories</h1>

      <LabelledInput
        id="search"
        label="Search"
        value={searchTerm}
        onInputChange={handleSearch}
      />

      ...
    </div>
  );
};

const LabelledInput = ({ id, label, value, onInputChange }) => (
  <>
    <label htmlFor={id}>{label}</label>
    &nbsp;
    <input
      id={id}
      type="text"
      value={value}
      onChange={onInputChange}
    />
  </>
);
```

It's not reusable to the fullest extend yet, because what if someone wants an input field for a number or phone number? The `type` attribute of the input field needs to be accessible from the outside too:

```javascript{5,13}
const LabelledInput = ({
  id,
  label,
  value,
  type = 'text',
  onInputChange,
}) => (
  <>
    <label htmlFor={id}>{label}</label>
    &nbsp;
    <input
      id={id}
      type={type}
      value={value}
      onChange={onInputChange}
    />
  </>
);
```

From the App component, no `type` prop is passed to the LabelledInput component and thus it's not specified from the outside. Hence the [default parameter](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/Default_parameters) from the function signature takes over for the input field.

With a few changes, we turned a specialized Search component into a more reusable component. It was just a matter of generalizing the namings of the internal implementation details and giving the new component a greater API surface to provide all the neccessary information from the outside. So far, We are not using the component somewhere else at the moment, but we have increased the likelihood of using it somewhere else to a great extent.

### Exercise:

* Confirm your [source code for the last section](https://codesandbox.io/s/github/the-road-to-learn-react/hackernews-client/tree/hs/Reusable-React-Component).
* Read more about [Reusable React Components](https://www.robinwieruch.de/react-reusable-components).

## React Component Composition

Maybe you have wondered all the time what would happen if one would use a React element like a HTML element with an opening and closing tag. What happens to the part in between?

```javascript{12-14}
const App = () => {
  ...

  return (
    <div>
      <h1>My Hacker Stories</h1>

      <LabelledInput
        id="search"
        value={searchTerm}
        onInputChange={handleSearch}
      >
        Search
      </LabelledInput>

      ...
    </div>
  );
};
```

Instead of using the `label` prop from before, we just inserted the "Search"-text in between of the component element's tags. In the LabelledInput component, you have access to this information via React's `children` prop. Instead of using the `label` prop, use the `children` to render everything that has been passed from above.

```javascript{6,9}
const LabelledInput = ({
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
```

Now the React component's elements behave similar to normal HTML. Everything that's passed between a component's elements can be accessed as `children` in the component. Sometimes when using a React component, you want to have more freedom from the outside what to render in the inside of a component:

```javascript{13}
const App = () => {
  ...

  return (
    <div>
      <h1>My Hacker Stories</h1>

      <LabelledInput
        id="search"
        value={searchTerm}
        onInputChange={handleSearch}
      >
        <strong>Search:</strong>
      </LabelledInput>

      ...
    </div>
  );
};
```

Having this React feature at your hands, gives you the possibilty to compose React components into each other. We have seen it with a JavaScript string and with a string wrapped in a HTML `<strong>` element, but it doesn't end here. You can pass multiple components or whole component hierarchies via React children too.

### Exercise:

* Confirm your [source code for the last section](https://codesandbox.io/s/github/the-road-to-learn-react/hackernews-client/tree/hs/React-Component-Composition).
* Read more about React Component Composition ([0](https://www.robinwieruch.de/react-component-composition), [1](https://reactjs.org/docs/composition-vs-inheritance.html)).

## Imperative React

React is inherently declarative; starting with JSX and ending with hooks. In JSX you are telling React *what* to render and not *how* to render it. In a React side-effect hook, you are telling when to achieve *what* instead of *how* to achieve it.

However, sometimes you want to access the rendered elements of your JSX imperatively. Use cases for imperative React are:

* read/write access to elements via the DOM API. Examples:
  * measure (read) an element's width or height
  * setting (write) an input field's focus state
* implementation of more complex animations. Examples:
  * setting transitions
  * orchestrating transitions
* integration of third-party libraries. Examples:
  * D3 is a popular imperative chart library

Because imperative programming in React often turns out verbose and counterintuitive, we will walk through a very small example, by setting the focus of an input field imperatively, just for the sake of learning about it.

The most straight forward and declarative way would be to simply set the input field's auto focus attribute:

```javascript{9}
const LabelledInput = ({ ... }) => (
  <>
    <label htmlFor={id}>{children}</label>
    &nbsp;
    <input
      id={id}
      type={type}
      value={value}
      autoFocus
      onChange={onInputChange}
    />
  </>
);
```

This works, but only if one component of these highly reusable components is rendered at once. For instance, if the App componen would render two of them, only the last rendered component would receive the auto focus on render. However, since we are having a (reusable) React component here, we can pass a dedicated prop and let the developer decide whether this input should have an auto focus or not:

```javascript{11}
const App = () => {
  ...

  return (
    <div>
      <h1>My Hacker Stories</h1>

      <LabelledInput
        id="search"
        value={searchTerm}
        isFocused
        onInputChange={handleSearch}
      >
        <strong>Search:</strong>
      </LabelledInput>

      ...
    </div>
  );
};
```

Using just `isFocused` as attribute is equivalent to `isFocused={true}`. Within the component, use the new prop for the input field's `autoFocus` attribute:

```javascript{6,16}
const LabelledInput = ({
  id,
  value,
  type = 'text',
  onInputChange,
  isFocused,
  children,
}) => (
  <>
    <label htmlFor={id}>{children}</label>
    &nbsp;
    <input
      id={id}
      type={type}
      value={value}
      autoFocus={isFocused}
      onChange={onInputChange}
    />
  </>
);
```

That's it. The feature should work yet it's still a declarative implementation. We are telling React *what* to do and not *how* to do it. Even though it's possible to do it with the declarative approach, let's refactor this scenario to an imperative approach. We want to execute the `focus()` method programmatically via the input field's DOM API when it renders:

```javascript{9-10,12-18,24,26}
const LabelledInput = ({
  id,
  value,
  type = 'text',
  onInputChange,
  isFocused,
  children,
}) => {
  // B1
  const inputRef = React.useRef();

  // A
  React.useEffect(() => {
    if (isFocused) {
      // C
      inputRef.current.focus();
    }
  }, [isFocused]);

  return (
    <>
      <label htmlFor={id}>{children}</label>
      &nbsp;
       {/* B2 */}
      <input
        ref={inputRef}
        id={id}
        type={type}
        value={value}
        onChange={onInputChange}
      />
    </>
  );
};
```

All the essential steps are marked with comments that are explained step by step:

* (A) First, opt-in into React's lifecycle with React's useEffect Hook; performing the focus on the input field when the component renders (or its dependencies change).
* (B) Second, create a `ref` with React's useRef hook (B1). This `ref` object is a persisted value which stays intact over the lifetime of a React component. It comes with a property called `current`. The `ref` is passed to the input field's JSX reserved `ref` attribute (B2) and automatically assigns the element instance to the `current` property.
* (C) And third, since, as mentioned before, the `ref` is passed to the input field's `ref` attribute, its `current` property gives access to the element. Execute its focus programmtically as a side-effect.

This was just an example on how to move from declarative to imperative programming in React. Sometimes it isn't possible to go the declarative way, so you have to take the imperative route. Anyway, if you can stick to the declarative approach of doing things in React, stick to it.

### Exercises:

* Confirm your [source code for the last section](https://codesandbox.io/s/github/the-road-to-learn-react/hackernews-client/tree/hs/Imperative-React).
* Read more about [React's useRef Hook](https://reactjs.org/docs/hooks-reference.html#useref).

## Inline Handler in JSX

The list of stories we have so far is only a static variable. We are able to filter the rendered list with the search feature, but the list itself stays intact if we remove the filter. It's just a fluctuant change through a third party. We cannot manipulate the list for real yet.

In order to get control over the list, make it stateful via React's useState Hook by using it as initial state for it. The returned values are the current stories (`stories`) and the stories updater function (`setStories`):

```javascript{1-10,13}
const initialStories = [
  {
    title: 'React',
    ...
  },
  {
    title: 'Redux',
    ...
  },
];

const App = () => {
  const [stories, setStories] = React.useState(initialStories);

  ...
};
```

We are not using the custom `useSemiPersistentState` hook yet, because we don't want to open the browser every time with the manipulated list. We always want to start with the initial list.

The application should behave the same, because the `stories`, now returned from `useState`, are still filtered into `searchedStories` and displayed in the List. Let's move on to a feature where we will manipulate the list by removing an item from it.

```javascript{6-12,24}
const App = () => {
  const [stories, setStories] = React.useState(initialStories);

  ...

  const handleRemoveStory = item => {
    const newStories = stories.filter(
      story => item.objectID !== story.objectID
    );

    setStories(newStories);
  };

  ...

  return (
    <div>
      <h1>My Hacker Stories</h1>

      ...

      <hr />

      <List list={searchedStories} onRemoveItem={handleRemoveStory} />
    </div>
  );
};
```

The callback handler in the App component receives as argument an item (to be removed) and filters based on this information the current stories by removing all items not meeting the condition. The returned stories are then set as new state.

The List component just passes the function to its child component. It's not using this new information it receives itself but just passes it on:

```javascript{1,6}
const List = ({ list, onRemoveItem }) =>
  list.map(item => (
    <Item
      key={item.objectID}
      item={item}
      onRemoveItem={onRemoveItem}
    />
  ));
```

Finally we can use the incoming function in another handler in the Item component for passing the `item` to it. In addition, a button element is used to trigger the actual event.

```javascript{1-4,14-18}
const Item = ({ item, onRemoveItem }) => {
  const handleRemoveItem = () => {
    onRemoveItem(item);
  };

  return (
    <div>
      <span>
        <a href={item.url}>{item.title}</a>
      </span>
      <span>{item.author}</span>
      <span>{item.num_comments}</span>
      <span>{item.points}</span>
      <span>
        <button type="button" onClick={handleRemoveItem}>
          Dismiss
        </button>
      </span>
    </div>
  );
};
```

We could have decided to only pass the item's `objectID`, because that's everything we need up in the App component's callback handler, however, we are not sure what other information the handler may need in the future. It could be that it needs more than an identifier of the item to actually remove it. So if we call the handler `onRemoveItem`, it better be the item being passed and not only its identifier.

For the sake of completeness, since a handler is just a function, and in this case it doesn't return anything, we could remove the block body for it.

```javascript{2-3}
const Item = ({ item, onRemoveItem }) => {
  const handleRemoveItem = () =>
    onRemoveItem(item);

  ...
};
```

However, this change may not make things more readable when accumulating more and more handlers in a function component. On a personal note, sometimes I even refactor handlers in a function component form an arrow function back to a normal function statement, just to make the component more explorable:

```javascript{2-4}
const Item = ({ item, onRemoveItem }) => {
  function handleRemoveItem() {
    onRemoveItem(item);
  }

  ...
};
```

But this section isn't called **inline handler** without any reason. So far, we have applied all the gathered knowledge from previous sections about props, handlers, callback handlers, and state. An inline handler would allow us to execute the function right in the JSX. There are two solutions to using the incoming function as an inline handler:

```javascript{10}
const Item = ({ item, onRemoveItem }) => (
  <div>
    <span>
      <a href={item.url}>{item.title}</a>
    </span>
    <span>{item.author}</span>
    <span>{item.num_comments}</span>
    <span>{item.points}</span>
    <span>
      <button type="button" onClick={onRemoveItem.bind(null, item)}>
        Dismiss
      </button>
    </span>
  </div>
);
```

JavaScript's bind method on a function allows us to bind arguments directly to the function that should be used when executing the function. The bind method returns a new function with the bound argument in place.

Using the bind method isn't popular in React though, because, I guess, the bind method isn't much used in general. Another solution, and a more popular one, would be using a wrapping arrow function which allows us to sneak in arguments (e.g. `item`):

```javascript{10}
const Item = ({ item, onRemoveItem }) => (
  <div>
    <span>
      <a href={item.url}>{item.title}</a>
    </span>
    <span>{item.author}</span>
    <span>{item.num_comments}</span>
    <span>{item.points}</span>
    <span>
      <button type="button" onClick={() => onRemoveItem(item)}>
        Dismiss
      </button>
    </span>
  </div>
);
```

You may call it the "quick and dirty"-solution, because sometimes we are just lazy humans who don't want to refactor a function component's function back to a block body in order to define an appropriate handler between function signature and return statement. Even though this version seems more concise than the previous two versions, it *may* be more difficult to reason about, because JavaScript logic may be just hidden in JSX. It becomes more verbose if the wrapping arrow function encapsulates more than one line of implementation logic by using a block body instead of a concise body.

All three handler versions, whereas two of them are inline handler, are acceptable to use. Whereas the first version with the non inlined handler moves the implementation details into the function component's block body, the inline handler move the implementation details into the JSX. The inline handler version with the bind method doesn't add much implemention logic though and the version with the wrapping arrow function should avoid to add anymore implementation logic.

### Exercises:

* Confirm your [source code for the last section](https://codesandbox.io/s/github/the-road-to-learn-react/hackernews-client/tree/hs/Inline-Handler-in-JSX).
* Go through the concepts of handler, callback handler, and inline handler one more time yourself.

## React Asynchronous Data

We have two interactions now. It's possible to search the list and to remove items from the list. Whereas the first interaction is only a fluctuant interference through a third-party state (`searchTerm`) that's applied on the list, the second interaction is a non reverseable deletion of an item from the list.

Not always we are able to have our data from the start. Sometimes we have to render a component first before fetching data from a third-party API and displaying it. In the following, we will simulate this kind of assynchronous data in the application. Later this asycnrhonous data gets fetched from a real remote API.

We start off with a function that returns a promise -- with data once it resolves -- in its shorthand version. The from the promise returned object holds the previous list of stories in another object:

```javascript{3-4}
const initialStories = [ ... ];

const getAsyncStories = () =>
  Promise.resolve({ data: { stories: initialStories } });
```

Now in the App component, instead of using the `initialStories`, use an empty array for the initial state. We want to start off with an empty list of stories but simulate fetching these stories asynchronously. Then in a new `useEffect` hook call, the function and resolve the returned promise. Due to the empty dependency array, the side-effect only runs once after the component rendered for the first time.

```javascript{2,4-8}
const App = () => {
  const [stories, setStories] = React.useState([]);

  React.useEffect(() => {
    getAsyncStories().then(result => {
      setStories(result.data.stories);
    });
  }, []);

  ...
};
```

Even though the data should arrive kinda asynchrously when we start the application, it appears to be rendered immediatley. Let's change this by giving it a bit of a delay. First, remove the shorthand version for the promise:

```javascript{2-4}
const getAsyncStories = () =>
  new Promise(resolve =>
    resolve({ data: { stories: initialStories } })
  );
```

And second, when resolving the promise in a `then()` block, delay it for a few seconds:

```javascript{3-6}
const getAsyncStories = () =>
  new Promise(resolve =>
    setTimeout(
      () => resolve({ data: { stories: initialStories } }),
      2000
    )
  );
```

Once you start the application again, you should see a delayed rendering of the list. The initial state for the stories is an empty array now. However, after the App component rendered, the side-effect hook runs once to fetch the asynchronous data. After resolving the promise and setting the arriving data in the component's state, the component renders again and displays the list of asynchrously loaded stories.

### Exercises:

* Confirm your [source code for the last section](https://codesandbox.io/s/github/the-road-to-learn-react/hackernews-client/tree/hs/React-Asynchronous-Data).
* Read more about [JavaScript Promises](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise).

## React Conditional Rendering

Dealing with asynchronous data in React leaves us with conditional states: either there is data or there is no data. Fortunatelty this case is already covered, becuse our initial state is an empty list rather than `null`. If it would be null, we would need to deal with this issue in our JSX. However, since it's `[]`, we `filter()` over an empty array which leaves us with an empty array which leads to just rendering nothing in the List component's `map()` function.

However, in a real world case there are more than two condotional states for asynchronous data. What about showing your users a loading indicator when delayed data is loaded:

```javascript{3,6,10}
const App = () => {
  const [stories, setStories] = React.useState([]);
  const [isLoading, setIsLoading] = React.useState(false);

  React.useEffect(() => {
    setIsLoading(true);

    getAsyncStories().then(result => {
      setStories(result.data.stories);
      setIsLoading(false);
    });
  }, []);

  ...
};
```

With [JavaScript's ternary operator](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Conditional_Operator),  we can inline this conditional state as a conditional rendering in JSX:

```javascript{10-12,17}
const App = () => {
  ...

  return (
    <div>
      ...

      <hr />

      {isLoading ? (
        <p>Loading ...</p>
      ) : (
        <List
          list={searchedStories}
          onRemoveItem={handleRemoveStory}
        />
      )}
    </div>
  );
};
```

Asynchronous data comes with error handling too. It doesn't happen in our simulated environment, however, if we start fetching data from another third-party API, there could be errors if somethign goes wrong. Thus introduce another state for error handling and deal with it in the error handling `catch()` block when resolving the promise:

```javascript{4,14}
const App = () => {
  const [stories, setStories] = React.useState([]);
  const [isLoading, setIsLoading] = React.useState(false);
  const [isError, setIsError] = React.useState(false);

  React.useEffect(() => {
    setIsLoading(true);

    getAsyncStories()
      .then(result => {
        setStories(result.data.stories);
        setIsLoading(false);
      })
      .catch(() => setIsError(true));
  }, []);

  ...
};
```

Then, give a user feedback in case something went wrong in the application with another conditional rendering. This time, it's either render something or nothing. So instead of having a ternary operator where one side returns `null`, it's a shorthand to use the logical `&&` operator instead:

```javascript{10}
const App = () => {
  ...

  return (
    <div>
      ...

      <hr />

      {isError && <p>Something went wrong ...</p>}

      {isLoading ? (
        <p>Loading ...</p>
      ) : (
        ...
      )}
    </div>
  );
};
```

In JavaScript a `true && 'Hello World'` always evaluates to 'Hello World'. A `false && 'Hello World'` always evaluates to false. In React you can make use of that behaviour. If the condition is true, the expression after the logical `&&` operator will be the output. If the condition is false, React ignores and skips the expression.

Conditional rendering is not only a case for asynchronous data though. The simplest example of conditional rendering would be a boolean flag state which can be toggled with a button. If the boolean flag is true, render something, if it is false, don't render anything.

Overall this feature is quite powerful, because it gives you the ability to conditionally render JSX. It's yet another tool in React to make your UI more dynamic. And as you have seen, it is needed for more complex control flows like asynchronous data.

### Exercises:

* Confirm your [source code for the last section](https://codesandbox.io/s/github/the-road-to-learn-react/hackernews-client/tree/hs/React-Conditional-Rendering).
* Read more about [conditional rendering in React](https://www.robinwieruch.de/conditional-rendering-react/).

## React Advanced State

All state management in this application makes heavily use of React's useState Hook. More sophisticated state management gives you React's useReducer Hook though.

Since knowing about reducers in JavaScript splits the community in half, we will not go extensivly through the concept of a reducer here, but the exercises of this section should give you plenty of learning material about them.

First, introduce a reducer function outside of your components. A reducer function always receives `state` and `action`. Based on these two arguments, a reducer always returns a new state:

```javascript{1-7}
const storiesReducer = (state, action) => {
  if (action.type === 'SET_STORIES') {
    return action.payload;
  } else {
    throw new Error();
  }
};
```

Commonly a reducer `action` is associated with a `type`. If this type matches a condition in the reducer, do something. If it isn't covered by the reducer, throw an error just to get notified as developer that your implementation isn't covered.

In the case of the `storiesReducer`, it just covers one `type` and returns the `payload` of the incoming action without using the current state to compute the new state. The new state is simply the `payload`.

Second, in the App component, exchange `useState` for `useReducer` for managing the `stories`. The new hook receives a reducer function and an initial state as arguments and returns an array with two items. Whereas the first item is the *current state*, the second item is the *state updater function* (also called *dispatch function*):

```javascript{2-5}
const App = () => {
  const [stories, dispatchStories] = React.useReducer(
    storiesReducer,
    []
  );

  ...
};
```

Third, the new dispatch function can be used instead of the `setStories` function which has been returned from `useState` previosuly. Instead of setting state explicitly with the state updater function from `useState`, the `useReducer` state updater function dispatches an action for the reducer. The action comes with a in the reducer matching `type` and a payload:

```javascript{9-12,25-28}
const App = () => {
  ...

  React.useEffect(() => {
    setIsLoading(true);

    getAsyncStories()
      .then(result => {
        dispatchStories({
          type: 'SET_STORIES',
          payload: result.data.stories,
        });
        setIsLoading(false);
      })
      .catch(() => setIsError(true));
  }, []);

  ...

  const handleRemoveStory = item => {
    const newStories = stories.filter(
      story => item.objectID !== story.objectID
    );

    dispatchStories({
      type: 'SET_STORIES',
      payload: newStories,
    });
  };

  ...
};
```

Everything should appear the same in the browser, however, a reducer and React's `useReducer` hook are managing the state for the stories now. Let's bring this concept of a reducer to a minimal version by handling more than one state transition.

So far, the `handleRemoveStory` handler computes the new stories. It's valid to move this logic into the reducer function and just tell the reducer with an action what it should do. It's a case for moving from imperative to declarative programming again. Instead of doing it ourselves by saying *how it should be done*, we are telling the reducer *what to do* instead.

```javascript{5-8}
const App = () => {
  ...

  const handleRemoveStory = item => {
    dispatchStories({
      type: 'REMOVE_STORY',
      payload: item,
    });
  };

  ...
};
```

Now the reducer function has to cover this new case in a new conditional state transition. If the condition for removing a story is met, the reducer has all the implementation details to remove the story. The action gives all the neccessary information, an item's identifier`, to remove the story from the current state and to return a new list of filtered stories as state.

```javascript{4-8}
const storiesReducer = (state, action) => {
  if (action.type === 'SET_STORIES') {
    return action.payload;
  } else if (action.type === 'REMOVE_STORY') {
    return state.filter(
      story => action.payload.objectID !== story.objectID
    );
  } else {
    throw new Error();
  }
};
```

All these if else statements will clutter when adding more state transitions into one reducer function. Refactoring it to a switch statement for all the state transitions makes it more readable:

```javascript{2-11}
const storiesReducer = (state, action) => {
  switch (action.type) {
    case 'SET_STORIES':
      return action.payload;
    case 'REMOVE_STORY':
      return state.filter(
        story => action.payload.objectID !== story.objectID
      );
    default:
      throw new Error();
  }
};
```

That's a minimal version of a reducer in JavaScript. It covers two state transitions, show how to compute current state and action into a new state, and uses some business logic as well. Now we are able to set a list of stories as state for the asynchronoysly arriving data and to remove a story from the list of stories with one state managing reducer and its associated `useReducer` hook.

If you haven't fully grasped the concept of a reducer in JavaScript and the usage of React's useReducer Hook, make sure to go through the linked resources in the exercises. After all, reducers make your application more predictable as we will learn in the next section.

### Exercises:

* Confirm your [source code for the last section](https://codesandbox.io/s/github/the-road-to-learn-react/hackernews-client/tree/hs/React-Advanced-State).
* Read more about [reducers in JavaScript](https://www.robinwieruch.de/javascript-reducer).
* Read more about reducers and useReducer in React ([0](https://www.robinwieruch.de/react-usereducer-hook), [1](https://reactjs.org/docs/hooks-reference.html#usereducer)).

## React Impossible States

Perhaps you have noticed a disjoint between the single states in the App component -- which somehow seem to belong together -- due to all the `useState` hooks. Technically all the states related to the asynchronous data belong together, which doesn't only include the stories as actual data, but also their loading and error states.

There is nothing wrong with multiple `useState` hooks in one React component. However, once you see multiple state updater functions being called one after another, it should make you suspicious, because all these conditional states can lead to so called *impossible states*. An impossible state of a component can lead to an undesired behavior in the UI. We already have introduced one impossible state throughout the previous sections, can you spot it?

The impossible state happens in case of an error for the asynchrous data. If there is an error, the state for the error is set, but the state for the loading indicator isn't revoked. In the UI, this would lead to a never ending loading indicator and an error message side by side. However, it may be better showing only the error message and hiding the loading indicator. To be honest, impossible staes are not easy to spot, which makes them infamous for bugs in the UI.

Fortunately we can do better by moving state which belongs together from multiple `useState` (and `useReducer`) hooks to one `useReducer` hook. Therefore, take the following `useState` hooks:

```javascript
const App = () => {
  const [stories, dispatchStories] = React.useReducer(
    storiesReducer,
    []
  );

  const [isLoading, setIsLoading] = React.useState(false);
  const [isError, setIsError] = React.useState(false);

  ...
};
```

And merge them into the one `useReducer` hook for one unified state management for this complex state object:

```javascript{4}
const App = () => {
  const [stories, dispatchStories] = React.useReducer(
    storiesReducer,
    { data: [], isLoading: false, isError: false }
  );

  ...
};
```

Next, everything related to asynchronous data fetching needs to use the new dispatch function for state transtitions:

```javascript{8,13,17}
const App = () => {
  const [stories, dispatchStories] = React.useReducer(
    storiesReducer,
    { data: [], isLoading: false, isError: false }
  );

  React.useEffect(() => {
    dispatchStories({ type: 'STORIES_FETCH_INIT' });

    getAsyncStories()
      .then(result => {
        dispatchStories({
          type: 'STORIES_FETCH_SUCCESS',
          payload: result.data.stories,
        });
      })
      .catch(() => dispatchStories({ type: 'STORIES_FETCH_FAILURE' }));
  }, []);

  ...
};
```

Since we had to introduce new types for state transitions, we have to handle them in the reducer function:

```javascript{3-28}
const storiesReducer = (state, action) => {
switch (action.type) {
    case 'STORIES_FETCH_INIT':
      return {
        ...state,
        isLoading: true,
        isError: false,
      };
    case 'STORIES_FETCH_SUCCESS':
      return {
        ...state,
        isLoading: false,
        isError: false,
        data: action.payload,
      };
    case 'STORIES_FETCH_FAILURE':
      return {
        ...state,
        isLoading: false,
        isError: true,
      };
    case 'REMOVE_STORY':
      return {
        ...state,
        data: state.data.filter(
          story => action.payload.objectID !== story.objectID
        ),
      };
    default:
      throw new Error();
  }
};
```

For every state transition, we return a *new state* object which contains all the key/value pairs from the *current state* object (via JavaScript's spread operator) and the new overwriting properties. For instance, `STORIES_FETCH_FAILURE` resets the `isLoading` but sets the `isError` boolean flags yet keeps all the other state instact (e.g. `stories`). That's how we get around the bug that we have introduced earlier, because an error should remove the loading state.

Notice how the `REMOVE_STORY` action operates on the `state.data` and not only on the plain `state` anymore. The state is a complex object with data, loading and error states rather than just a list of stories now.

```javascript{9,17,19}
const App = () => {
  const [stories, dispatchStories] = React.useReducer(
    storiesReducer,
    { data: [], isLoading: false, isError: false }
  );

  ...

  const searchedStories = stories.data.filter(story =>
    story.title.toLowerCase().includes(searchTerm.toLowerCase())
  );

  return (
    <div>
      ...

      {stories.isError && <p>Something went wrong ...</p>}

      {stories.isLoading ? (
        <p>Loading ...</p>
      ) : (
        <List
          list={searchedStories}
          onRemoveItem={handleRemoveStory}
        />
      )}
    </div>
  );
};
```

In conclusion, we moved from unreliable state transitions with multiple `useState` hooks to predictable state transitions with React's useReducer Hook. Not only does the state object managed by the reducer encapsulate everything related to the stories including loading and error state but also implementation details like removing a story from the list of stories.

### Exercises:

* Confirm your [source code for the last section](https://codesandbox.io/s/github/the-road-to-learn-react/hackernews-client/tree/hs/React-Impossible-States).
* Double down on the previous linked tutorials about reducers in JavaScript and React. The concept of a reducer isn't easy to grasp, but it simplifies lots of things in the long run.
* Read more about [when to use useState or useReducer in React](https://www.robinwieruch.de/react-usereducer-vs-usestate).

## Data Fetching with React

We are already fetching data, but it's still sample data coming from a Promise we set up ourselves. All these preparation of the application and accumulating more knowledge about asynchrnous React and advanced state management helps us to fetch data from a third-party API for real. We will use the [Hacker News API](https://hn.algolia.com/api) for requesting popular tech stories about our industry. If you haven't heard about [Hacker News](https://news.ycombinator.com/), it's worth to check out once a day for a tech newsflash.

```javascript{2,10-11,15}
// A
const API_ENDPOINT = 'https://hn.algolia.com/api/v1/search?query=';

const App = () => {
  ...

  React.useEffect(() => {
    dispatchStories({ type: 'STORIES_FETCH_INIT' });

    fetch(`${API_ENDPOINT}react`) // C, B
      .then(response => response.json()) // D
      .then(result => {
        dispatchStories({
          type: 'STORIES_FETCH_SUCCESS',
          payload: result.hits, // E
        });
      })
      .catch(() =>
        dispatchStories({ type: 'STORIES_FETCH_FAILURE' })
      );
  }, []);

  ...
};
```

Instead of using the `initialStories` array and `getAsyncStories` function, we will fetch the data directly from the API. First, the `API_ENDPOINT` (A) is used for fetching popular tech stories for a certain topic (here: query). In this case, we fetch stories about React (B). Second, the native browser's [fetch API](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API) is used to make this request (C). When using the fetch API, the response needs to be translated into JSON (D). Last but not least, the returned result follows a different data structure (E) which we send as payload like before to our component's state.

In the previous code, we used [JavaScript's Template Literals](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Template_literals) for a string interpolation. When this feature wasn't available in JavaScript, one would have used the + operator on strings instead:

```javascript
const greeting = 'Hello';

const welcome = greeting + ' React';
console.log(welcome);
// Hello React

const anotherWelcome = `${greeting} React`;
console.log(anotherWelcome);
// Hello React
```

Considering the latest changes for the App component, there wasn't much to implement for the data fetching. But that's only true because we came to this solution step by step with the previous sections. Managing asynchronous data as state in React makes this happen.

### Exercises:

* Confirm your [source code for the last section](https://codesandbox.io/s/github/the-road-to-learn-react/hackernews-client/tree/hs/Data-Fetching-with-React).
* Read more about [JavaScript's Template Literals](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Template_literals).

## Data Re-Fetching in React

So far, the App component is fetching a list of stories once with a pre-defined query (here: 'react'). Afterward, a user is able to search the stories on the client-side. Now we will move this feature from client-side to server-side searching, but using the actual `searchTerm` as a dynamic query for the API request.

First, remove the `searchedStories`, because we will receive the stories searched from the API right away. Then pass only the regular stories to the List component:

```javascript{11}
const App = () => {
  ...

  return (
    <div>
      ...

      {stories.isLoading ? (
        <p>Loading ...</p>
      ) : (
        <List list={stories.data} onRemoveItem={handleRemoveStory} />
      )}
    </div>
  );
};
```

And second, instead of using a hardcoded search term like before, use the actual `searchTerm` from the component's state. If `searchTerm` is an empty string, do nothing:

```javascript{5,9}
const App = () => {
  ...

  React.useEffect(() => {
    if (searchTerm === '') return;

    dispatchStories({ type: 'STORIES_FETCH_INIT' });

    fetch(`${API_ENDPOINT}${searchTerm}`)
      .then(response => response.json())
      .then(result => {
        dispatchStories({
          type: 'STORIES_FETCH_SUCCESS',
          payload: result.hits,
        });
      })
      .catch(() =>
        dispatchStories({ type: 'STORIES_FETCH_FAILURE' })
      );
  }, [searchTerm]);

  ...
};
```

You have changed the feature from a client-side to a server-side search. Instead of filtering a pre-defined list of stories on the client, the `searchTerm` is used to fetch a server-side filtered list.

In addition, let's implement data refetching. If the `searchTerm` changes, run the side-effect for the data fetching again. If `searchTerm` is not present (e.g. null, empty string, undefined), do nothing (as a more generalized condition):

```javascript{5,20}
const App = () => {
  ...

  React.useEffect(() => {
    if (!searchTerm) return;

    dispatchStories({ type: 'STORIES_FETCH_INIT' });

    fetch(`${API_ENDPOINT}${searchTerm}`)
      .then(response => response.json())
      .then(result => {
        dispatchStories({
          type: 'STORIES_FETCH_SUCCESS',
          payload: result.hits,
        });
      })
      .catch(() =>
        dispatchStories({ type: 'STORIES_FETCH_FAILURE' })
      );
  }, [searchTerm]);

  ...
};
```

The server-side search happens for the initial data fetching, but also for ever other data fetching if the `searchTerm` changes. The feature is fully server-side powered now.

Re-fetching all data every time someone types into the input field isn't optimal. We will change this behavior in one of the next sections. Because of this API stressing implementatuon, you may experience getting errors from the API instead of the result if you bother it too often with your requests.

### Exercise:

* Confirm your [source code for the last section](https://codesandbox.io/s/github/the-road-to-learn-react/hackernews-client/tree/hs/Data-Re-Fetching-in-React).

## Memoized Handler in React (Advanced)

The previous sections have taught you about handler, callback handler and inline handler in React. There is another kind of handler, a memoized handler, which can be applied on top of handler and callback handler. For the sake of learning about a memoized handler, we will move all the data fetching logic into a standalone function outside of the side-effect (A). Then, wrap it into a `useCallback` hook (B), and invoke it in the `useEffect` hook (C):

```javascript{5,21,24-25}
const App = () => {
  ...

  // A
  const handleFetchStories = React.useCallback(() => {
    if (!searchTerm) return;

    dispatchStories({ type: 'STORIES_FETCH_INIT' });

    fetch(`${API_ENDPOINT}${searchTerm}`)
      .then(response => response.json())
      .then(result => {
        dispatchStories({
          type: 'STORIES_FETCH_SUCCESS',
          payload: result.hits,
        });
      })
      .catch(() =>
        dispatchStories({ type: 'STORIES_FETCH_FAILURE' })
      );
  }, [searchTerm]); // E

  React.useEffect(() => {
    handleFetchStories(); // C
  }, [handleFetchStories]); // D

  ...
};
```

The application behaves still the same, only the implementation logic has been refactored. Instead of using the data fetching logic anonymously in a side-effect, you have made it available as a function for the application.

Let's dive deeper into why React's `useCallback` Hook is needed here. This hook creates a memoized function once and every time its dependency array (E) changes. As a result, the `useEffect` hook runs again (C), because it depends on the newly created function (D).

```javascript
1. change: searchTerm
2. implicit change: handleFetchStories
3. run: side-effect
```

Think about the following for a moment: What would happen if we didn't create a memoized function with React's `useCallback` Hook; which would mean that with *every* re-rendering of the App component a new `handleFetchStories` function would be created.

Correct, the `handleFetchStories` function would be created every time and would be executed in the `useEffect` hook to fetch data. Then the fetched data is stored as state in the component. Because the state of the component changed, the component re-renders and creates a new `handleFetchStories` function. Again the side-effect would be triggered. We would be stuck in an endless loop.

```javascript
1. define: handleFetchStories
2. run: side-effect
3. update: state
4. re-render: component
1. re-define: handleFetchStories
2. run: side-effect
...
```

Therefore, the `useCallback` hook changes the function only once the search term changed. And that's exaclty the time we want to trigger a re-fetch of the data, because after typing something new in the input field, we want to see the new data displayed in our list.

By moving the function for the data fetching outside of the `useEffect` hook, it becomes reusable for other parts of the application. Even though we will not use it (yet) somewhere else, it was a neat step for the sake of explaining the `useCallback` hook. Now the `useEffect` hook runs implcitly when the `searchTerm` changes, because the `handleFetchStories` is re-defined every time the `searchTerm` changes and thus, because the `useEffect` hook depends on the `handleFetchStories`, the side-effect for data fetching runs again.

### Exercise:

* Confirm your [source code for the last section](https://codesandbox.io/s/github/the-road-to-learn-react/hackernews-client/tree/hs/Memoized-Handler-in-React).
* Read more about [React's useCallback Hook](https://reactjs.org/docs/hooks-reference.html#usecallback).
* Read more about [useMemo vs useCallback](https://www.robinwieruch.de/react-usememo-vs-usecallback).

## Explicit Data Fetching with React

Re-fetching all data every time someone types into the input field isn't optimal. Since we are using a third-party API for fetching the data, its internals are out of our react. Eventually we will hit a [rate limiting](https://en.wikipedia.org/wiki/Rate_limiting) which will not return data anymore but an error.

In the following, we will change the implementation details from implicit to explicit data (re)fetching. In other words, we will not refetch data every time someone types into the input field, but only if someone clicks a confirmation button. We will implement this in three steps. First, we will add a button element for the confirmation to the JSX:

```javascript{12,17-23}
const App = () => {
  ...

  return (
    <div>
      <h1>My Hacker Stories</h1>

      <LabelledInput
        id="search"
        value={searchTerm}
        isFocused
        onInputChange={handleSearchInput}
      >
        <strong>Search:</strong>
      </LabelledInput>

      <button
        type="button"
        disabled={!searchTerm}
        onClick={handleSearchSubmit}
      >
        Submit
      </button>

      ...
    </div>
  );
};
```

Second, both handler, input and button handler, get their implementation logic for updating the component's state. Whereas the input field handler still updates the `searchTerm`, the button handler sets the API URL derived from the *current* `searchTerm` as another new state:

```javascript{7-9,13,17-19}
const App = () => {
  const [searchTerm, setSearchTerm] = useSemiPersistentState(
    'search',
    'React'
  );

  const [url, setUrl] = React.useState(
    `${API_ENDPOINT}${searchTerm}`
  );

  ...

  const handleSearchInput = event => {
    setSearchTerm(event.target.value);
  };

  const handleSearchSubmit = () => {
    setUrl(`${API_ENDPOINT}${searchTerm}`);
  };

  ...
};
```

And third, instead of running the side-effect for data fetching on every `searchTerm` change -- which would happen with every change of the input field -- the `url` is used which is only set expliclty by a user when confirming the search:

```javascript{7,18}
const App = () => {
  ...

  const handleFetchStories = React.useCallback(() => {
    dispatchStories({ type: 'STORIES_FETCH_INIT' });

    fetch(url)
      .then(response => response.json())
      .then(result => {
        dispatchStories({
          type: 'STORIES_FETCH_SUCCESS',
          payload: result.hits,
        });
      })
      .catch(() =>
        dispatchStories({ type: 'STORIES_FETCH_FAILURE' })
      );
  }, [url]);

  React.useEffect(() => {
    handleFetchStories();
  }, [handleFetchStories]);

  ...
};
```

Before the `searchTerm` was used for two cases: updating the input field's state and activating the side-effect for fetching data. Now it's only used for the former. A second state called `url` got introduced for triggering the side-effect which only happens when a user clicks the confirmation button.

### Exercises:

* Confirm your [source code for the last section](https://codesandbox.io/s/github/the-road-to-learn-react/hackernews-client/tree/hs/Explicit-Data-Fetching-with-React).
* Why is `useState` instead of `useSemiPersistentState` used for the `url` state management?
* Why is there no check for an empty `searchTerm` in the `handleFetchStories` function anymore?

## Third-Party Libraries in React

Before, we introduced the native fetch API to perform a request to the Hacker News API. The browser allows you to use this native fetch API. However, not all browsers support this, older browsers especially. Once you start testing your application in a headless browser environment (where there is no browser, it is mocked), there can be issues with the fetch API. There are a couple of ways to make fetch work in older browsers (polyfills) and in tests (isomorphic-fetch), but these concepts are bit off-task for the purpose of learning React.

One alternative is to substitute the native fetch API with a stable library such as axios, which performs asynchronous requests to remote APIs. In this section, we will discover how to substitute a library, a native API of the browser in this case, with another library from the npm registry.

Below, the native fetch API is substituted with axios. First, we install axios on the command line:

```javascript
npm install axios
```

Second, import axios in your App component's file:

```javascript{2}
import React from 'react';
import axios from 'axios';

...
```

You can use axios instead of `fetch`, and its usage looks almost identical to the native fetch API. It takes the URL as argument and returns a promise. You don't have to transform the returned response to JSON anymore, since axios wraps the result into a data object in JavaScript. Just make sure to adapt your code to the returned data structure:

```javascript{7-8,12}
const App = () => {
  ...

  const handleFetchStories = React.useCallback(() => {
    dispatchStories({ type: 'STORIES_FETCH_INIT' });

    axios
      .get(url)
      .then(result => {
        dispatchStories({
          type: 'STORIES_FETCH_SUCCESS',
          payload: result.data.hits,
        });
      })
      .catch(() =>
        dispatchStories({ type: 'STORIES_FETCH_FAILURE' })
      );
  }, [url]);

  ...
};
```

In this code, we call axios `axios.get()` for an explicit HTTP GET request. You can use other HTTP methods such as HTTP POST with `axios.post()` too. With these examples alone, we can see that axios is a powerful library to perform requests to remote APIs. I recommend you use it instead of the native fetch API when requests become complex, when working with older browser, or when you want to get into testing.

### Exercises:

* Confirm your [source code for the last section](https://codesandbox.io/s/github/the-road-to-learn-react/hackernews-client/tree/hs/Third-Party-Libraries-in-React).
* Read more about [popular libraries in React](https://www.robinwieruch.de/react-libraries).
* Read more about [axios](https://github.com/axios/axios).

## Async/Await in React

Often you will be working with asynchronous data in React. Therefore, I believe it's good to know about an alternative syntax for handling promises: async/await. The following refactoring of the `handleFetchStories` function (without error handling) shows how to use it:

```javascript{4,7}
const App = () => {
  ...

  const handleFetchStories = React.useCallback(async () => {
    dispatchStories({ type: 'STORIES_FETCH_INIT' });

    const result = await axios.get(url);

    dispatchStories({
      type: 'STORIES_FETCH_SUCCESS',
      payload: result.data.hits,
    });
  }, [url]);

  ...
};
```

If you want to use async/await, you need to give your function the `async` keyword. Once you start using the `await` keyword, everything reads like synchronous code: Everything after the 'await' keyword isn't executed as long as the promise hasn't been resolved successfully. The code will indeed wait there. Once the promise resolves successfully, everything after will be executed next.

```javascript{7,14-16}
const App = () => {
  ...

  const handleFetchStories = React.useCallback(async () => {
    dispatchStories({ type: 'STORIES_FETCH_INIT' });

    try {
      const result = await axios.get(url);

      dispatchStories({
        type: 'STORIES_FETCH_SUCCESS',
        payload: result.data.hits,
      });
    } catch {
      dispatchStories({ type: 'STORIES_FETCH_FAILURE' });
    }
  }, [url]);

  ...
};
```

If you want to include error handling like before, the `try` and `catch` blocks are there to help. In case something goes wrong in the `try` block, which is often refered to as the *happy path* as we had it before, the code will jump into the `catch` block for handling the error (called *unhappy path*).

Both options, `then`/`catch` blocks or async/await with `try`/`catch` blocks, are valid for dealing with asynchronous data in JavaScript (and React). Since async/await gains more popularity these days, I will keep this version for the next sections. However, it's up to you which fits you best.

### Exercise:

* Confirm your [source code for the last section](https://codesandbox.io/s/github/the-road-to-learn-react/hackernews-client/tree/hs/Async-Await-in-React).
* Read more about [data fetching in React](https://www.robinwieruch.de/react-hooks-fetch-data).
* Read more about [async/await in JavaScript](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/async_function).

## Forms in React

A while ago we introduced a new button for fetching data explicitly on a button click. Here we will advance its usage by giving it a proper HTML form which should encapsulate the button but also the input field for the search term in addition to its label.

Forms aren't much different in React's JSX than in normal HTML. We will implement one in only two steps of refactoring and additional HTML/JavaScript. First, wrap the input field and button into a HTML form element:

```javascript{8,18,21}
const App = () => {
  ...

  return (
    <div>
      <h1>My Hacker Stories</h1>

      <form onSubmit={handleSearchSubmit}>
        <LabelledInput
          id="search"
          value={searchTerm}
          isFocused
          onInputChange={handleSearchInput}
        >
          <strong>Search:</strong>
        </LabelledInput>

        <button type="submit" disabled={!searchTerm}>
          Submit
        </button>
      </form>

      <hr />

      ...
    </div>
  );
};
```

Instead of passing the `handleSearchSubmit` handler to the button, it's used in the new form element. The button receives a new `type` attribute called `submit` which indicates that the form element handles the click and not the button.

And second, because the handler is used for the form event, it has to execute `preventDefault` on React's synthetic event. This prevents a HTML form's native behavior which would natively lead to a reload of the browser.

```javascript{4,7}
const App = () => {
  ...

  const handleSearchSubmit = event => {
    setUrl(`${API_ENDPOINT}${searchTerm}`);

    event.preventDefault();
  };

  ...
};
```

Everything should work as before with the neat benefit of being able to execute the search feature with the "Enter"-key of your keyboard. In the next two steps, we will only split out the component as its standalone SearchForm component:

```javascript{1-20}
const SearchForm = ({
  searchTerm,
  onSearchInput,
  onSearchSubmit,
}) => (
  <form onSubmit={onSearchSubmit}>
    <LabelledInput
      id="search"
      value={searchTerm}
      isFocused
      onInputChange={onSearchInput}
    >
      <strong>Search:</strong>
    </LabelledInput>

    <button type="submit" disabled={!searchTerm}>
      Submit
    </button>
  </form>
);
```

The new component is used by the App component. The App component still manages the state for the form, because the state is used in the App component and partly passed as props (here `stories.data`) to the List component:

```javascript{8-12}
const App = () => {
  ...

  return (
    <div>
      <h1>My Hacker Stories</h1>

      <SearchForm
        searchTerm={searchTerm}
        onSearchInput={handleSearchInput}
        onSearchSubmit={handleSearchSubmit}
      />

      <hr />

      {stories.isError && <p>Something went wrong ...</p>}

      {stories.isLoading ? (
        <p>Loading ...</p>
      ) : (
        <List list={stories.data} onRemoveItem={handleRemoveStory} />
      )}
    </div>
  );
};
```

As you have seen, forms aren't much different in React. Whenever you end up with input fields and one button to submit data from these input fields, give your HTML more structure by wrapping it into a form element with a `onSubmit` handler. The button which executes the submission only has to have the "submit" `type`.

### Exercise:

* Confirm your [source code for the last section](https://codesandbox.io/s/github/the-road-to-learn-react/hackernews-client/tree/hs/Forms-in-React).
* Try the code without `preventDefault`.
* Read more about [preventDefault for Events in React](https://www.robinwieruch.de/react-preventdefault).
