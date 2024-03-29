# A Roadmap for React

So far, you have learned everything that you need to know about React's fundamentals. With this knowledge, you *could* take a break from this book and create a minimal React application yourself. There will for sure be questions popping up, but you can always seek answers by going through the fundamentals of this book again. After all, learning React.js (and any other programming language, framework, library) is best done by getting your hands dirty. My favorite approach: Learn the fundamentals, learn how to connect to an API, find an API that delivers data which aligns with your personal interests, and build something with it! Fortifying your knowledge of the fundamentals is key to what comes next.

Continuing from here, there are several paths you can take for your learning experience. First of all, you can continue reading this book. At its core, the next sections will primarily touch three aspects: advanced features of React, organizational topics for every React project, and React's ecosystem. You will learn about topics such as performance optimizations, folder/file structures of a React project, static types with TypeScript, and styling in React. However, in my opinion, I kept the selection to a non-overwhelming minimum, because otherwise this book would be a never ending story. If you have the desire to dive deeper into various subjects though, I can give you the following material below.

React's ecosystem is huge. Every year I sum up [all the essential yet popular libraries](https://www.robinwieruch.de/react-libraries) that can be used in React for various aspects. You can poke around in the list and try different libraries that could improve your own project. For some of them I have written dedicated tutorial series too, such as [React Router](https://www.robinwieruch.de/react-router/) and [React Table Library](https://www.robinwieruch.de/react-table-component/). While the former is the most popular routing library for React, the latter is a library which I open sourced myself to create data tables in React and which runs in production for several of my freelance clients.

Next there is one advanced feature in React that we didn't touch in the book and will not touch in the rest of it. I kept it outside because of one reason: it's an advanced feature which would bloat our minimal application without really showing what problem it solves. So it would end up being a premature optimization in the eyes of every intermediate developer or an obstacle in the eyes of every novice developer. However, you may come across it eventually, so I don't want you to miss it: React Context. If you want to learn about it, I highly recommend you to read more about [React Context](https://www.robinwieruch.de/react-context/) and [React's useContext Hook](https://www.robinwieruch.de/react-usecontext-hook/) in addition to actually using it for a [more advanced pattern by combining multiple hooks in a real application](https://www.robinwieruch.de/react-state-usereducer-usestate-usecontext/) (recommended read).

React comes with many patterns for components too. You have learned about [component composition](https://www.robinwieruch.de/react-component-composition/) and [reusable components](https://www.robinwieruch.de/react-reusable-components/) already. If you haven't read the referenced tutorials about these patterns, I encourage you to catch up with them. There are more patterns though, and covering all of them here wouldn't give the patterns justice in a small application. That's why I covered them, e.g. [React Render Prop Components](https://www.robinwieruch.de/react-render-props/) and [React Higher-Order Components](https://www.robinwieruch.de/react-higher-order-components/), extensively in other material of mine. With the succession of React Hooks though, these patterns are less used these days; however, in larger React applications you will most likely encounter them.

Furthermore, you have used Vite to bootstrap your project in this book. If you are interested in [setting up a React project from scratch](https://www.robinwieruch.de/minimal-react-webpack-babel-setup/) yourself, by using tools such as Webpack and Babel which power JavaScript build pipelines, I encourage you to go through the process. You will learn lots about what's going on under the hood in a third-party tool like Vite.

Now we've reached the middle of the Road to React, and I hope you enjoyed reading it so far. In case you liked it, it would mean a lot to me if you would share the book with your friends who are interested in learning React. Also, a **review on** [Amazon](https://amzn.to/2JHlP42) or [Goodreads](https://www.goodreads.com/book/show/37503118-the-road-to-learn-react) would be very much appreciated.

From here on, you can continue reading the book to learn about an opinionated selection of React's ecosystem, organizational recommendations, and more built-in features of React (e.g. performance optimizations). At the end of the book, you will find even more sections helping you to implement advanced features for your current React application. In summary, I hope all of the prior learnings, the referenced material, plus the following sections of the book help you to become a great React developer.

**Important:** The following chapters with their sections do not follow a linear path anymore. While all of them build up on the application that you have got right now, they will fork into different directions. You can try on your own to merge them all into your current application (which works most of the times, but not for the styling sections where you have to choose a path). If this gets too overwhelming, there are two alternatives to this approach:

1. Copy and paste the current application and use one copy for each individual path:
2. Read a chapter (path) with its sections (potential subpaths), apply the changes, and optionally revert the changes after the learning to start with a clean slate with the next chapter.

Let me summarize the paths you can with this book below:

- Styling in React: Each section in this chapter demonstrates an **alternative path**.
- React Maintenance: The chapter follows a **linear path** with its sections.
- TypeScript in React: The chapter follows a **linear path** with its sections.
- Testing in React: The chapter follows a **linear path** with its sections.
- React Project Structure: The chapter follows a **linear path** with its sections.
- Real World React (Advanced): The chapter follows a **linear path** with its sections.

All chapters will build upon your current application. No other chapter will inherit the changes from the other chapters though.

![](images/react-roadmap.png)