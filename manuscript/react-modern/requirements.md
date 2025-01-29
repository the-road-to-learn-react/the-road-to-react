## Requirements

To navigate this book effectively, you should have a basic understanding of web development, including HTML, CSS, and JavaScript. Familiarity with [APIs](https://www.robinwieruch.de/what-is-an-api-javascript/) is helpful, as they will be covered later. Additionally, you'll need the following coding tools to follow along.

### Editor and Terminal

For this learning experience, I recommend using an [IDE](http://bit.ly/3OWCnan), such as Visual Studio Code (VSCode), especially for beginners. It provides an advanced editor with an integrated terminal and is the most popular choice among web developers. I've created a [setup guide](https://www.robinwieruch.de/developer-setup/) to help you get started with general web development. It includes all the details and is kept separate from this book since it offers options for both Windows and macOS users. Optionally, you can also check out my complete [macOS setup guide](https://www.robinwieruch.de/mac-setup-web-development/).

If you prefer not to set up an editor and terminal on your local machine, [CodeSandbox](https://codesandbox.io) is a viable online alternative. While **CodeSandbox** is great for sharing code, a local setup is a better learning environment for building web applications. If you plan to develop applications professionally, a local setup will eventually be required.

Throughout this book, I will use *command* line as a general term for *command line tool*, *terminal*, and *integrated terminal*. Similarly, *editor*, *text editor*, and *IDE* will be used interchangeably, depending on your setup.

Additionally, I recommend using **GitHub** to manage projects as we go through the exercises. I've provided a [short guide](https://www.robinwieruch.de/git-essential-commands/) on using Git and GitHub. Version control is invaluable--it allows you to track changes, undo mistakes, and follow along more effectively. It's also a great way to share your code with others.

### Node and NPM

Before we begin, you'll need to install [Node and NPM](https://nodejs.org/en/). These tools help manage the libraries (Node packages) required throughout this book. These packages can range from small utilities to entire frameworks. We'll use npm (Node Package Manager) to install them.

To check if Node and npm are installed, run the following commands in your terminal:

{title="Command Line",lang="text"}
~~~~~~~
node --version
*vXX.YY.ZZ
npm --version
*vXX.YY.ZZ
~~~~~~~

If no version numbers appear, you'll need to install Node and npm. If you already have them installed, ensure you're using the latest version. If you're new to npm or need a refresher, my [npm crash course](https://www.robinwieruch.de/npm-crash-course/) will get you up to speed.

### Exercises:

* Optional: Read more about [yarn](https://yarnpkg.com/) and [pnpm](https://pnpm.io/). Both can be used as a replacement for npm. However, I do not recommend using them as a beginner. This exercise should only make sure that you know about the alternatives.