## Setting up a React Project

In the Road to React, we'll use [Vite](https://bit.ly/3BsG1TH) to set up our React application. Vite, a french word which translates to "quick", is a modern build tool for status quo web frameworks (e.g. React) which comes with sensible defaults (read: configuration) while staying highly extensible for specific use cases (e.g. SVG support, Lint support, TypeScript) later on. The essential core of Vite is a **development server**, which allows you to start your React application on your local machine (read: development environment), and a bundler, which outputs highly optimized files for a production-ready deployment (read: production environment). What matters for a React beginner here: getting started with React by just learning React while not getting distracted by any tooling around it. Therefore Vite is the perfect partner for learning React.

There are two ways to create your project with Vite. First, choosing an [online template](https://bit.ly/3RPAZWz), either React (recommended for this book) or React TypeScript (advanced, which means you implement the types for TypeScript yourself) for working on your project online without a local setup. Second, which is the way I would recommend, is creating a React project with Vite on your local machine for working on it in your local IDE/editor (e.g. VSCode).

Since the online template works out of the box, we will focus on the setup for your local machine in this section (recommended). In a previous section, you have installed Node and npm. The latter enables you to install third-party dependencies (read: libraries/frameworks/etc.) from the command line. So open up your command line tool and move to a folder where you want to create your React project. As a crash course for navigating on the command line:

* use `pwd` (Windows: `cd`) to display the current folder
* use `ls` (Windows: `dir`) to display all folders and files in the current folder
* use `mkdir <folder_name>` to create a folder
* use `cd <folder_name>` to move into a folder
* use `cd ..` to move outside of a folder

After navigating into a folder where you want to create your React project, type the following command. We'll refer to this project as *hacker-stories*, but you may choose any project name you like:

{title="Command Line",lang="text"}
~~~~~~~
npm create vite@latest hacker-stories -- --template react
~~~~~~~

Optionally you can also go with a React + TypeScript project if you feel confident (check Vite's installation website to follow their instructions for a React + TypeScript project). The book comes with a TypeScript section later on, however, it will not do any hand-holding throughout the sections for transforming JavaScript into TypeScript. Next, follow the instructions given on the command line for navigating into the folder, installing all the third-party dependencies of the project, and running it locally on your machine:

{title="Command Line",lang="text"}
~~~~~~~
cd hacker-stories
npm install
npm run dev
~~~~~~~

The command line should output a URL where you can find your project running in the browser. Open up the browser with the given URL and verify that you can see the React project running there. We will continue developing this project in the next sections, however, for the rest of this section, we will go through explaining the project structure and the scripts (e.g. `npm run dev`).

## Project Structure

First, let's open the application in an editor/IDE. For VSCode, you can simply type `code .` on the command line. The following folder structure, or a variation of it depending on the *Vite* version, should be presented:

{title="Project Structure",lang="text"}
~~~~~~~
hacker-stories/
--node_modules/
--public/
----vite.svg
--src/
----assets/
------react.svg
----App.css
----App.jsx
----index.css
----main.jsx
--.gitignore
--index.html
--package-lock.json
--package.json
--vite.config.js
~~~~~~~

This is a breakdown of the most important folders and files:

* **package.json:** This file shows you a list of all third-party dependencies (read: node packages which are located in the *node_modules/* folder) and other essential project configurations related to Node/npm.
* **package-lock.json:** This file indicates npm how to break down (read: resolve) all node package versions and their internal third-party dependencies. We'll not touch this file.
* **node_modules/:** This folder contains all node packages that have been installed. Since we used Vite to create our React application, there are various node modules (e.g. React) already installed for us. We'll not touch this folder.
* **.gitignore:** This file indicates all folders and files that shouldn't be added to your git repository when using git, as such files and folders should be located only on your local machine. The *node_modules/* folder is one example. It is enough to share the *package.json* and *package-lock.json* files with other developers in the team, so they can install dependencies on their end with `npm install` without having to share the entire *node_modules/* folder with everybody.
* **vite.config.js:** A file to configure Vite. If you open it, you should see Vite's React plugin showing up there. If you would be running Vite with another web framework, the other framework's Vite plugin would show up. In the end, there are many more things that can optionally be set up here.
* **public/:** This folder holds static assets for the project like a [favicon](https://bit.ly/3QvRupG) which is used for the browser tab's thumbnail when starting the development server or building the project for production.
**index.html:** The HTML that is displayed in the browser when starting the project. If you open it, you shouldn't see much content though. However, you should see a script tag which links to your source folder where all the React code is located to output HTML/CSS in the browser.

In the beginning, everything you need is located in the *src/* folder. The main focus lies on the *src/App.jsx* file which is used to implement React components. It will be used to implement your application, but later you might want to split up your React components into multiple files, where each file maintains one or more components on its own. We will arrive at this point eventually.

Additionally, you will find a *src/main.jsx* as an entry point to the React world. You will get to know this file intimately in later sections. There is also a *src/index.css* and a *src/App.css* file to style your overall application and components, which comes with the default style when you open them. You will modify them later as well.

## npm Scripts

After you have learned about the folder and file structure of your React project, let's go through the available commands. All your project-specific commands can be found in your *package.json* file under the `scripts` property. They may look similar to these depending on your Vite version:

{title="package.json",lang="javascript"}
~~~~~~~
{
  ...
  },
  "scripts": {
    "dev": "vite",
    "build": "vite build",
    "preview": "vite preview"
  },
  ...
}
~~~~~~~

These scripts are executed with the `npm run <script>` command in an IDE-integrated terminal or your standalone command line tool. The commands are as follows:

{title="Command Line",lang="text"}
~~~~~~~
# Runs the application locally for the browser
npm run dev

# Builds the application for production
npm run build
~~~~~~~

Another command from the previous npm scripts called `preview` can be used to run the production-ready build on your local machine for testing purposes. In order to make it work, you have to execute `npm run build` before running `npm run preview`. Essentially `npm run dev` and `npm run preview` (after `npm run build`) should give the identical output in the browser. However, the former is not optimized by a build for production and should exclusively be used for the local development of the application.

### Exercises:

* Read more about [Vite](https://bit.ly/3BsG1TH).
* Exercise npm scripts:
  * Start your React application with `npm run dev` on the command line and check it out in the browser.
    * Exit the command on the command line by pressing `Control + C`.
  * Run the `npm run build` script and verify that a *dist/* folder was added to your project. Note that the build folder can be used later on to [deploy your application](https://www.robinwieruch.de/deploy-applications-digital-ocean/). Afterward, run `npm run preview` to see the production-ready application in the browser.
* Every time we change something in our source code throughout the coming sections, make sure to check the output in your browser for getting visual feedback. Use `npm run dev` to keep your application running.
* Optional: If you use git and GitHub, add and commit your changes after every section of the book.
* Optional: [Leave feedback for this section](https://forms.gle/bvH2jcppsSA6p9i16).