## Setting up a React Project

In The Road to React, we'll use [Vite](https://bit.ly/3BsG1TH) to set up our React application. Vite--a French word meaning quick--is a modern build tool for contemporary web frameworks (e.g. React). It comes with sensible defaults (read: built-in configuration) while remaining highly extensible for specific use cases (e.g. SVGs, linting, TypeScript, server-side rendering).

The core of Vite consists of:

* A development server, which allows you to run your React application locally (read: development environment).
* A bundler, which generates highly optimized files for production-ready deployment (read: production environment).

For React beginners, the key benefit of Vite is that it enables you to focus solely on learning React without being distracted by complex tooling. This makes Vite the perfect partner for getting started with React.

There are two ways to create a React project with Vite:

* Using an [online template](https://bit.ly/3RPAZWz) -- You can choose either React (recommended for this book) or React with TypeScript (for advanced users, requiring manual type implementation). This option lets you work online without setting up a local environment.
* Setting up Vite locally (recommended) -- This method involves creating a React project with Vite on your local machine and working in your preferred IDE (e.g. VSCode).

Since the online template works out of the box, we'll focus on setting up Vite on your local machine in this section. In a previous section, you installed Node and npm. The latter allows you to install third-party dependencies (read: libraries, frameworks, etc.) from the command line.

To get started, open your command line tool and navigate to the folder where you want to create your React project. Here's a quick crash course on command-line navigation:

* use `pwd` (on Windows: `cd`) to display the current folder
* use `ls` (on Windows: `dir`) to display all folders and files in the current folder
* use `mkdir <folder_name>` to create a folder
* use `cd <folder_name>` to move into a folder
* use `cd ..` to move outside of a folder

After navigating to the folder where you want to create your React project, enter the following command. We'll refer to this project as *hacker-stories*, but feel free to choose any name you like.

{title="Command Line",lang="text"}
~~~~~~~
npm create vite@latest hacker-stories -- --template react
~~~~~~~

Optionally, you can choose a React + TypeScript project if you feel confident. Check Vite's installation website for instructions on setting up a React + TypeScript project. This book includes a TypeScript section later; however, it does not provide step-by-step guidance on converting JavaScript to TypeScript. Instead, at the end of each section, you'll find an alternative TypeScript implementation.

Next, follow the command line instructions to navigate into the project folder, install all third-party dependencies, and run the project locally on your machine:

{title="Command Line",lang="text"}
~~~~~~~
cd hacker-stories
npm install
npm run dev
~~~~~~~

The command line will output a URL where your project is running in the browser. Open the browser, navigate to the provided URL, and verify that the React project is displayed correctly.

Additionally, please check in your *package.json* file whether you are on the latest React version. At the time of writing, Vite comes with React 18, but there is already React 19 out there. If you want to use React 19, you can manually upgrade React in your project.

{title="Command Line",lang="text"}
~~~~~~~
npm install react@latest react-dom@latest
~~~~~~~

If you are using React with TypeScript, you also need to update the React's types:

{title="Command Line",lang="text"}
~~~~~~~
npm install --save-dev @types/react@latest @types/react-dom@latest
~~~~~~~

We will continue developing this project in the next sections; however, for the remainder of this section, we will explore the project structure and scripts (e.g. `npm run dev`).

### Exercises:

* Read more about [how to start a React project](https://www.robinwieruch.de/react-starter/).

## Project Structure

First, let's open the application in an editor/IDE. If you're using VSCode, simply type `code .` in the command line. The following folder structure (or a variation of it, depending on the Vite version) should be displayed:

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
--eslint.config.js
--index.html
--package-lock.json
--package.json
--README.md
--vite.config.js
~~~~~~~

This is a breakdown of the most important folders and files:

* **package.json:** This file shows you a list of all third-party dependencies (read: node packages which are located in the *node_modules/* folder) and other essential project configurations related to Node/npm.
* **package-lock.json:** This file indicates npm how to break down (read: resolve) all node package versions and their internal third-party dependencies. We'll not touch this file.
* **node_modules/:** This folder contains all node packages that have been installed. Since we used Vite to create our React application, there are various node modules (e.g. React) already installed for us. We'll not touch this folder.
* **.gitignore:** This file indicates all folders and files that shouldn't be added to your git repository when using git, as such files and folders should be located only on your local machine. The *node_modules/* folder is one example. It is enough to share the *package.json* and *package-lock.json* files with other developers in the team, so they can install dependencies on their end with `npm install` without having to share the entire *node_modules/* folder with everybody.
* **vite.config.js:** A file to configure Vite. If you open it, you should see Vite's React plugin showing up there. If you would be running Vite with another web framework, the other framework's Vite plugin would show up. In the end, there are many more things that can optionally be set up here.
* **public/:** This folder holds static assets for the project like a [favicon](https://bit.ly/3QvRupG) which is used for the browser tab's thumbnail when starting the development server or building the project for production.
* **index.html:** The HTML that is displayed in the browser when starting the project. If you open it, you shouldn't see much content though. However, you should see a script tag which links to your source folder where all the React code is located to output HTML/CSS in the browser.

In the beginning, everything you need is located in the *src/* folder. The main focus is on the *src/App.jsx* file, where React components are implemented. This file will serve as the foundation for your application, but later on, you may want to split your React components into multiple files, with each file managing one or more components. We'll get to that point eventually.

Additionally, you'll find a src/main.jsx file, which serves as the entry point to the React world. You'll become more familiar with this file in later sections. There are also src/index.css and src/App.css files to style your overall application and components, both of which come with default styles when you open them. You'll modify these later as well.

## npm Scripts

After you have learned about the folder and file structure of your React project, let's go through the available commands. All your project-specific commands can be found in your *package.json* file under the `scripts` property. They may look similar to these depending on your Vite version:

{title="package.json",lang="javascript"}
~~~~~~~
"dev": "vite",
"build": "vite build",
"lint": "eslint .",
"preview": "vite preview"
~~~~~~~

These scripts are executed with the `npm run <script>` command in an IDE-integrated terminal or your standalone command line tool. The commands are as follows:

{title="Command Line",lang="text"}
~~~~~~~
# Runs the application locally for the browser
npm run dev

# Lint the application locally for code style errors
npm run lint

# Builds the application for production
npm run build
~~~~~~~

Another command from the previous npm scripts called `preview` can be used to run the production-ready build on your local machine for testing purposes. In order to make it work, you have to execute `npm run build` before running `npm run preview`. Essentially `npm run dev` and `npm run preview` (after `npm run build`) should give the identical output in the browser. However, the former is not optimized for production and should exclusively be used for the local development of the application.

### Exercises:

* Read more about [Vite](https://bit.ly/3BsG1TH).
* Exercise npm scripts:
  * Start your React application with `npm run dev` on the command line and check it out in the browser.
    * Exit the command on the command line by pressing `Control + C`.
  * Run the `npm run build` script and verify that a *dist/* folder was added to your project. Note that the build folder can be used later on to deploy your application. Afterward, run `npm run preview` to see the production-ready application in the browser.
* Every time we change something in our source code throughout the coming sections, make sure to check the output in your browser for getting visual feedback. Use `npm run dev` to keep your application running.
* Optional: If you use git and GitHub, add and commit your changes after every section of the book.
