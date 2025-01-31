# A Roadmap for React

So far, you have learned everything you need to know about React's fundamentals. With this knowledge, you *could* take a break from this book and create a React application yourself. Questions will inevitably arise, but you can always find answers by revisiting the fundamentals covered in this book. After all, learning React.js (and anything else) is best done by getting your hands dirty. My favorite approach: learn the fundamentals, learn how to connect to an API, find an API that delivers data aligned with your personal interests, and build something with it! Strengthening your knowledge of the fundamentals is key to what comes next.

From here, there are several paths you can take in your learning journey. First, you can continue reading this book. The next sections primarily cover three aspects: advanced React features, organizational topics for every React project, and React's ecosystem. You will learn about topics such as performance optimizations, folder and file structures in React projects, static typing with TypeScript, and styling in React. However, I have intentionally kept the selection minimal to avoid overwhelming you -- otherwise, this book would become never-ending. If you wish to explore certain topics more deeply, I recommend the following resources.

React's ecosystem is vast. Every year, I summarize [all the essential yet popular libraries](https://www.robinwieruch.de/react-libraries) that can be used in React for various purposes. You can browse the list and experiment with libraries that could enhance your project. I have also written dedicated tutorials for some of them.

There is one advanced React feature that I have intentionally excluded from this book: React Context. The reason is simple -- it is an advanced feature that would unnecessarily complicate our minimal application without clearly demonstrating the problem it solves. To an intermediate developer, it might seem like premature optimization; to a beginner, it could feel like an obstacle. However, you will likely encounter it at some point, so I don't want you to miss it. If you want to learn about it, I highly recommend reading more about [React Context](https://www.robinwieruch.de/react-context/) and [React's useContext Hook](https://www.robinwieruch.de/react-usecontext-hook/), along with a [guide on combining multiple hooks in a real application](https://www.robinwieruch.de/react-state-usereducer-usestate-usecontext/) (a recommended read).

React also offers various component patterns. You have already learned about [component composition](https://www.robinwieruch.de/react-component-composition/) and [reusable components](https://www.robinwieruch.de/react-reusable-components/). If you haven't yet explored the referenced tutorials on these patterns, I encourage you to do so. There are additional patterns, but covering them all in a small application wouldn't do them justice. That's why I have written about them separately -- for example, [React Render Prop Components](https://www.robinwieruch.de/react-render-props/) and [React Higher-Order Components](https://www.robinwieruch.de/react-higher-order-components/). However, with the rise of React Hooks, these patterns are used less frequently today, though you may still encounter them in larger React applications.

Additionally, in this book, you used Vite to bootstrap your project. If you are interested in [setting up a React project from scratch](https://www.robinwieruch.de/minimal-react-webpack-babel-setup/) using tools like Webpack and Babel (which power JavaScript build pipelines), I encourage you to go through the process. Even though Webpack is no longer as popular, understanding how it works provides valuable insight into what happens under the hood in third-party tools like Vite.

Last but not least, I encourage you to check out my course, [The Road to Next](https://www.road-to-next.com/). While React + Vite follow a client-side library approach, Next.js is a full-fledged framework for React. It comes with many built-in features such as server-side rendering, routing, and more. The course will guide you through the fundamentals of Next.js, helping you build a real-world application. It's a great way to deepen your understanding of React and its ecosystem while gaining experience in full-stack development.

Moreover, React includes features such as [Server Components](https://tinyurl.com/283wewxw) and [Server Functions](https://tinyurl.com/4en9pmj9), which (at the time of writing) can only be used in a full-stack framework like Next.js. So, I highly recommend exploring Next.js and its features to gain a broader understanding of React's ecosystem and [how it is becoming a full-stack framework](https://www.robinwieruch.de/react-full-stack-framework/).

Here is how I would prioritize the above resources:

* continue with The Road to React
* explore the React ecosystem
* check out **The Road to Next** for full-stack development
  * including: Server Components and Server Functions

Optionally, you can:

* recap component patterns
* set up a React project from scratch
* learn about React Context and useContext

Now, we've reached the middle of The Road to React, and I hope you have enjoyed it so far. If you liked the book, it would mean a lot to me if you shared it with friends who are interested in learning React. Also, a **review on** [Amazon](https://amzn.to/2JHlP42) or [Goodreads](https://tinyurl.com/4bhcssu7) would be greatly appreciated.

From this point forward, you can continue reading to learn about an opinionated selection of React's ecosystem, organizational recommendations, and more built-in React features (e.g., performance optimizations). Toward the end of the book, you will find additional sections that help you implement advanced features in your React application. In summary, I hope the knowledge you've gained so far -- along with the referenced materials and upcoming chapters -- helps you become a great React developer.

**Important:** The following chapters do not follow a strictly linear path. While they all build upon the application you have created, they will diverge in different directions. You can try merging them into your current project, which usually works -- except for styling sections, where you will have to choose one approach. If this becomes overwhelming, consider these two alternatives:

1. Copy and paste your current application and use separate copies for different paths.
2. Read a chapter (path), apply the changes, and optionally revert them afterward to start fresh with the next chapter.

Below is a summary of the paths available in this book:

- Styling in React: Each section in this chapter presents an alternative path.
- React Maintenance: This chapter follows a linear path through its sections.
- TypeScript in React: This chapter follows a linear path through its sections.
- Testing in React: This chapter follows a linear path through its sections.
- React Project Structure: This chapter follows a linear path through its sections.
- Real-World React (Advanced): This chapter follows a linear path through its sections.

Each chapter builds upon your current application, but no chapter inherits changes from others.

![](images/react-roadmap.png)