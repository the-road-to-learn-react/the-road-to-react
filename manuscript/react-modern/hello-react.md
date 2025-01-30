# Fundamentals of React

In the initial phase of this learning journey, we'll explore the essential foundations of React, guiding you through the creation of your first React project. As we progress, we'll dive deeper into React's capabilities, implementing practical features such as client- and server-side searching, remote data fetching, and advanced state management. This hands-on approach mirrors the development of a real-world web application. By the end, you'll have a fully functional React application that seamlessly interacts with real-world data.

## Hello React

Single-page applications ([SPA](https://bit.ly/3BZOL1o)) have gained popularity with first-generation SPA frameworks like Angular (by Google), Ember, Knockout, and Backbone. These frameworks made it easier to build web applications that went beyond vanilla JavaScript and jQuery. React, introduced by Facebook in 2013, emerged as another solution for SPAs, offering a powerful way to build modern web applications in JavaScript.

Let's take a trip back in time before SPAs existed. In the past, websites and web applications were server-rendered. When a user accessed a URL in a browser, a request was sent to a web server, which returned an HTML file along with its associated CSS and JavaScript files. After some network delay, the user would see the rendered HTML and could begin interacting with it. Each subsequent page transition repeated this process. In this model, the server handled most essential tasks, while the client played a minimal role, primarily rendering pages. Basic HTML and CSS structured and styled the application, while JavaScript--often in the form of jQuery--enabled interactions (e.g., toggling a dropdown) or advanced styling (e.g., positioning a tooltip).

In contrast, SPA frameworks shifted the focus from the server to the client. In SPAs, the server primarily delivers JavaScript over the network, along with a minimal HTML file. The browser executes the linked JavaScript files, rendering the entire application dynamically using HTML and CSS while relying on JavaScript for interactions. In its most extreme form, a user visiting a URL receives a small HTML file and a larger JavaScript file. After a brief network and rendering delay, JavaScript renders the content in the browser. Subsequent page transitions no longer require additional server requests for new files; instead, the initially loaded JavaScript dynamically renders new pages.

React, along with other SPA solutions, played a pivotal role in this transformation. Essentially, an SPA is a structured bundle of JavaScript, organized into folders and files, forming a complete application. An SPA framework like React provides the tools to architect this JavaScript-driven application. When a user visits a web application's URL, the JavaScript-based application is delivered once over the network. From that point on, React--or any other SPA framework--takes over, rendering everything in the browser as HTML and handling user interactions with JavaScript.

With React's rise, the concept of components became central. Each component encapsulates its visual and functional aspects using HTML, CSS, and JavaScript. Once defined, components can be combined into a hierarchy to build a complete application. While React primarily focuses on components as a library, its flexible ecosystem allows it to function as a framework. With a streamlined API, a stable yet evolving ecosystem, and a supportive community, React is ready to welcome you with open arms! :-)

### Exercises

* Read more about [Websites and Web Applications](https://www.robinwieruch.de/web-applications/).
* Watch [React.js: The Documentary](https://bit.ly/3xrvxkI).
* Read more about [JavaScript fundamentals needed for React](https://www.robinwieruch.de/javascript-fundamentals-react-requirements/).
* Optionally, if you need a motivational boost:
  * Read more about [how to learn a framework](https://www.robinwieruch.de/how-to-learn-framework/).
  * Read more about [how to learn React](https://www.robinwieruch.de/learn-react-js/).