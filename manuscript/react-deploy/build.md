# Deploying a React Application

Now it's time to get out into the world with your React application. There are many ways to deploy a React application to production, and many competing providers that offer this service. We'll keep it simple here by narrowing it down on one provider, after which you'll be equipped to check out other hosting providers on your own.

## Build Process

So far, everything we've done has been the *development stage* of the application, when the development server handles everything: packaging all files to one application and serving it on localhost on your local machine. As a result, our code isn't available for anyone else.

The next step is to take your application to the *production stage* by hosting it on a remote server, called deployment, making it accessible for users of your application. Before an application can go public, it needs to be packaged as one essential application. Redundant code, testing code, and duplications are removed. There is also a process called minification at work which reduces the code size once more.

Fortunately, optimizations and packaging, also called bundling, comes with the build tools in Vite. First, build your application on the command line:

{title="Command Line",lang="text"}
~~~~~~~
npm run build
~~~~~~~

This creates a new *dist/* folder in your project with the bundled application. You could take this folder and deploy it on a hosting provider now, but we'll use a local server to mimic this process before engaging in the real thing. On the command line, serve your application with this Vite's local HTTP server:

{title="Command Line",lang="text"}
~~~~~~~
npm run preview
~~~~~~~

A URL is presented that provides access to your optimized, packaged and hosted application. It's sent through a local IP address that can be made available over your local network, meaning we're hosting the application on our local machine.

### Exercises:

* Optional: [Leave feedback for this section](https://forms.gle/hFsut8q7eYsWfYL7A).