# Fundamentals of React

In the first part of this learning experience, we'll cover the fundamentals of React, after which we'll create our first React project. Then we'll explore new aspects of React by implementing real features, the same as developing an actual web application. By the end we'll have a working React application with features like client and server-side searching, remote data fetching, and advanced state management.

## Hello React

Single-page applications ([SPA](https://en.wikipedia.org/wiki/Single-page_application)) have become increasingly popular with first generation SPA frameworks like Angular (by Google), Ember, Knockout, and Backbone. Using these frameworks made it easier to build web applications which advanced beyond vanilla JavaScript and jQuery. React, yet another solution for SPAs, was released by Facebook later in 2013. All of them are used to create entire web applications in JavaScript.

For a moment, let's go back in time before SPAs existed: In the past, websites and web applications were rendered from the server. A user visits a URL in a browser and requests one HTML file and all its associated HTML, CSS, and JavaScript files from a web server. After some network delay, the user sees the rendered HTML in the browser (client) and starts to interact with it. Every additional page transition (meaning: visiting another URL) would initiate this chain of events again. In this version from the past, essentially everything crucial is done by the server, whereas the client plays a minimal role by just rendering page by page. While barebones HTML and CSS was used to structure the application, just a little bit of JavaScript was thrown into the mix to make interactions (e.g. toggling a dropdown) or advanced styling (e.g. positioning a tooltip) possible. A popular JavaScript library for this kind of work was jQuery.

In contrast, modern JavaScript shifted the focus from the server to the client. The most extreme version of it: A user visits a URL and requests one *minimal HTML file* and *one associated JavaScript file*. After some network delay, the user sees the *by JavaScript rendered HTML* in the browser and starts to interact with it. Every additional page transition *wouldn't* request more files from the web server, but would use the initially requested JavaScript to render the new page. Also every additional interaction by the user is handled on the client too. In this modern version, the server delivers mainly JavaScript across the wire with one minimal HTML file. The HTML file then executes all the linked JavaScript on the client-side to render the entire application with HTML and JavaScript for its interactions.

React, among the other SPA solutions, makes this possible. Essentially a SPA is one bulk of JavaScript, which is neatly organized in folders and files, to create a whole application whereas the SPA framework gives you all the tools to architect it. This JavaScript focused application is delivered once over the network to your browser when a user visits the URL for your web application. From there, React, or any other SPA framework, takes over for rendering everything in the browser and for dealing with user interactions.

With the rise of React, the concept of components became popular. Every component defines its look and feel with HTML, CSS and JavaScript. Once a component is defined, it can be used in a hierarchy of components for creating an entire application. Even though React has a strong focus on components as a library, the surrounding ecosystem makes it a flexible framework. React has a slim API, a stable yet thriving ecosystem, and a welcoming community. We are happy to welcome you :-)

### Exercises

* Read more about [why I moved from Angular to React](https://www.robinwieruch.de/reasons-why-i-moved-from-angular-to-react/).
* Read more about [how to learn a framework](https://www.robinwieruch.de/how-to-learn-framework/).
* Read more about [how to learn React](https://www.robinwieruch.de/learn-react-js).
* Read more about [why frameworks matter](https://www.robinwieruch.de/why-frameworks-matter).
* Scan through [JavaScript fundamentals needed for React](https://www.robinwieruch.de/javascript-fundamentals-react-requirements) -- without worrying too much about the React -- to test yourself about several JavaScript features used in React.