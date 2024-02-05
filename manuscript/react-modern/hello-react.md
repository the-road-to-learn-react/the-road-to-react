# Fundamentals of React

In the initial phase of this learning journey, we'll delve into the essential foundations of React, guiding you through the creation of your first React project. As we progress, we'll expand our exploration of React's capabilities, implementing practical features such as client and server-side searching, remote data fetching, and advanced state management. This hands-on approach mirrors the development of a real-world web application. By the end, you'll have a fully functional React application seamlessly interacting with real-world data.

## Hello React

Single-page applications ([SPA](https://bit.ly/3BZOL1o)) have become increasingly popular with first-generation SPA frameworks like Angular (by Google), Ember, Knockout, and Backbone. Using these frameworks made it easier to build web applications that advanced beyond vanilla JavaScript and jQuery. React, introduced by Facebook in 2013, is another solution for SPAs, offering yet another powerful framework for building modern web applications in JavaScript.

Let's take a trip back in time before the advent of SPAs: In the past, websites and web applications were server-rendered. When a user accessed a URL in a browser, a request was made to a web server, fetching one HTML file along with its associated HTML, CSS, and JavaScript files. After some network delay, the user would see the rendered HTML in the browser and could begin interacting with it. Each subsequent page transition would trigger this sequence of events again. In this earlier version, the server handled most essential tasks, while the client's role was minimal, primarily focused on rendering pages. Basic HTML and CSS structured and styled the application, with a touch of JavaScript, often in the form of the popular library jQuery, to enable interactions (e.g. toggling a dropdown) or advanced styling (e.g. positioning a tooltip).

In contrast, SPA frameworks shifted the focus from the server to the client. In the world of SPAs, the server primarily delivers JavaScript over the network, accompanied by a minimal HTML file. The HTML file then executes the linked JavaScript files on the client-side (browser) to render the entire application using HTML (and CSS), while still relying on JavaScript for interactions. In its most extreme manifestation, a user visiting a URL requests a small HTML file and a larger JavaScript file. Following a network and rendering delay, the user sees the HTML rendered by JavaScript in the browser. Subsequent page transitions do not necessitate additional file requests from the web server but instead utilize the initially requested JavaScript to render new pages.

React, along with other SPA solutions, played a pivotal role in making this transformation possible. Essentially, a SPA is a single, organized bundle of JavaScript, neatly structured into folders and files, creating an entire application. The SPA framework, such as React, provides the necessary tools to architect this JavaScript-focused application. When a user visits the URL for your web application, this JavaScript-centric application is delivered once over the network to their browser. Subsequently, React or any other SPA framework takes charge of rendering everything in the browser as HTML and managing user interactions with JavaScript.

With the ascent of React, the concept of components gained popularity. Each component defines its visual and functional aspects using HTML, CSS, and JavaScript. Once a component is established, it can be integrated into a hierarchy of components to construct a complete application. While React primarily focuses on components as a library, its adaptable ecosystem positions it as a flexible framework. Featuring a streamlined API, a flourishing yet stable ecosystem, and a supportive community, React is ready to welcome you with open arms! :-)

### Exercises

* Read more about [Websites and Web Applications](https://www.robinwieruch.de/web-applications/).
* Watch [React.js: The Documentary](https://bit.ly/3xrvxkI).
* Read more about [JavaScript fundamentals needed for React](https://www.robinwieruch.de/javascript-fundamentals-react-requirements/).
* Optionally, if you need a motivational boost:
  * Read more about [how to learn a framework](https://www.robinwieruch.de/how-to-learn-framework/).
  * Read more about [how to learn React](https://www.robinwieruch.de/learn-react-js/).
* Optional: [Leave feedback for this section](https://forms.gle/NTqhvyDaP1RjtanC6).