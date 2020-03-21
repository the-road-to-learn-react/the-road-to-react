## Requirements

To follow this book you'll need to be familiar with the basics of web development, i.e how to use HTML, CSS, and JavaScript. It also helps to understand [APIs](https://www.robinwieruch.de/what-is-an-api-javascript/), as they will be covered thoroughly.

### Editor and Terminal

I have provided [a setup guide](https://www.robinwieruch.de/developer-setup/) to get you started with general web development. For this learning experience, you will need a text editor (e.g. Sublime Text) and a command line tool (e.g. iTerm) or an IDE (e.g. Visual Studio Code). I recommend Visual Studio Code (also called VS Code) for beginners, as it's an all-in-one solution that provides an advanced editor with an integrated command line tool, and because it's a popular choice among web developers.

Throughout this learning experience I will use the term *command line*, which will be used synonymously for the terms *command line tool*, *terminal*, and *integrated terminal*. The same applies for the terms *editor*, *text editor*, and *IDE*, depending on what you decided to use for your setup.

Optionally, I recommend managing projects in GitHub while we conduct the exercises in this book, and I've provided a [short guide](https://www.robinwieruch.de/git-essential-commands/) on how to use these tools. Github has excellent version control, so you can see what changes were made if you make a mistake or just want a more direct way to follow along. It's also a great way to share your code later with other people.

If you don't want to set up the editor/terminal combination or IDE on your local machine, [CodeSandbox](https://codesandbox.io/), an online editor, is also a viable alternative. While CodeSandbox is a great tool for sharing code online, a local machine setup is a better tool for learning the different ways to create a web application. Also, if you want to develop applications professionally, a local setup will be required.

### Node and NPM

Before we can begin, we'll need to have [node and npm](https://nodejs.org/en/) installed. Both are used to manage libraries (node packages) you will need along the way. These node packages can be libraries or whole frameworks. We'll install external node packages via npm (node package manager).

You can verify node and npm versions in the command line using the `node --version` command. If you don't receive output in the terminal indicating which version is installed, you need to install node and npm:

{title="Command Line",lang="text"}
~~~~~~~
node --version
*vXX.YY.ZZ
npm --version
*vXX.YY.ZZ
~~~~~~~

If you have already installed Node and npm, make sure that your installation is the most recent version. If you're new to npm or need a refresher, this [npm crash course](https://www.robinwieruch.de/npm-crash-course) I created will get you up to speed.

## Setting up a React Project

In the Road to React, we'll use [create-react-app](https://github.com/facebook/create-react-app) to bootstrap your application. It's an opinionated yet zero-configuration starter kit for React introduced by Facebook in 2016, which is [recommended for beginners by 96% of React users](https://twitter.com/dan_abramov/status/806985854099062785). In *create-react-app*, the tools and configurations evolve in the background, while the focus remains on the application's implementation.

After installing Node and npm, use the command line to type the following command in a dedicated folder for your project. We'll refer to this project as *hacker-stories*, but you may choose any name you like:

{title="Command Line",lang="text"}
~~~~~~~
npx create-react-app hacker-stories
~~~~~~~

Navigate into your new folder after the setup has finished:

{title="Command Line",lang="text"}
~~~~~~~
cd hacker-stories
~~~~~~~

Now we can open the application in an editor or IDE. For Visual Studio Code, you can simply type `code .` on the command line. The following folder structure, or a variation of it depending on the *create-react-app* version, should be presented:

{title="Project Structure",lang="text"}
~~~~~~~
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
~~~~~~~

This is a breakdown of the most important folders and files:

* **README.md:** The *.md* extension indicates the file is a markdown file. Markdown is a lightweight markup language with plain text formatting syntax. Many source code projects come with a *README*.md file that gives instructions and useful information about the project. When we push projects to platforms like GitHub, the *README.md* file usually displays information about the content contained in its repositories. Because you used create-react-app, your *README.md* should be the same as the official [create-react-app GitHub repository](https://github.com/facebook/create-react-app).
* **node_modules/:** This folder contains all node packages that have been installed via npm. Since we used create-react-app, a couple of node modules are already installed. We'll not touch this folder, since node packages are usually installed and uninstalled with npm via the command line.
* **package.json:** This file shows you a list of node package dependencies and other project configurations.
* **package-lock.json:** This file indicates npm how to break down all node package versions. We'll not touch this file.
* **.gitignore:** This file displays all files and folders that shouldn't be added to your git repository when using git, as such files and folders should be located only in your local project. The *node_modules/* folder is one example. It is enough to share the *package.json* file with others, so they can install dependencies on their end with `npm install` without your entire dependency folder.
* **public/:** This folder holds development files, such as *public/index.html*. The index file is displayed on *localhost:3000* when the app is in development or on a domain that is hosted elsewhere. The default setup handles relating this *index.html* with all the JavaScript from *src/*.

In the beginning, everything you need is located in the *src/* folder. The main focus lies on the *src/App.js* file which is used to implement React components. It will be used to implement your application, but later you might want to split up your components into multiple files, where each file maintains one or more components on its own.

Additionally, you will find a *src/App.test.js* file for your tests, and a *src/index.js* as an entry point to the React world. You will get to know both files intimately in later sections. There is also a *src/index.css* and a *src/App.css* file to style your general application and components, which comes with the default style when you open them. You will modify them later as well.

After you have learned about the folder and file structure of your React project, let's go through the available commands to get it started. All your project specific commands can be found in your *package.json* under the *scripts* property. They may look similar to these:

{title="package.json",lang="javascript"}
~~~~~~~
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
~~~~~~~

These scripts are executed with the `npm run <script>` command in an IDE-integrated terminal or command line tool. The `run` can be omitted for the `start` and `test` scripts. The commands are as follows:

{title="Command Line",lang="text"}
~~~~~~~
# Runs the application in http://localhost:3000
npm start

# Runs the tests
npm test

# Builds the application for production
npm run build
~~~~~~~

Another command from the previous npm scripts called `eject` shouldn't be used for this learning experience. It's a one way operation. Once you eject, you can't go back. Essentially this command is only there to make all the build tool and configuration from create-react-app accessible if you are not satisfied with the choices or if you want to change something. Here we will keep all the defaults though.

### Exercises:

* Read a bit more through React's [create-react-app documentation](https://github.com/facebook/create-react-app) and [getting started guide](https://create-react-app.dev/docs/getting-started).
  * Read more about [the supported JavaScript features in create-react-app](https://create-react-app.dev/docs/supported-browsers-features).
* Read more about [the folder structure in create-react-app](https://create-react-app.dev/docs/folder-structure).
  * Go through all of your React project's folders and files one by one.
* Read more about [the scripts in create-react-app](https://create-react-app.dev/docs/available-scripts).
  * Start your React application with `npm start` on the command line and check it out in the browser.
    * Exit the command on the command line by pressing `Control + C`.
  * Run the `npm test` script.
  * Run the `npm run build` script and verify that a *build/* folder was added to your project (you can remove it afterward). Note that the build folder can be used later on to [deploy your application](https://www.robinwieruch.de/deploy-applications-digital-ocean/).
* Every time we change something in our code throughout the coming learning experience, make sure to check the output in your browser for getting visual feedback.