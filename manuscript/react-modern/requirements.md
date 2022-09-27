## Requirements

To follow this book, you'll need to be familiar with the basics of web development, i.e how to use HTML, CSS, and JavaScript. It also helps to understand [APIs](https://www.robinwieruch.de/what-is-an-api-javascript/), as they will be covered in this learning experience. Along with these skills, you'll need the following tools to code with me while reading this book.

### Editor and Terminal

I have provided [a setup guide](https://www.robinwieruch.de/developer-setup/) to get you started with general web development. For this learning experience, you will need a text editor (e.g. Sublime Text) and a command line tool (e.g. iTerm). As an alternative, I recommend using an IDE, for example **Visual Studio Code** (also called VSCode), for beginners, as it's an all-in-one solution that provides an advanced editor with an integrated command line tool, and because it's a popular choice among web developers.

If you don't want to set up the editor/terminal combination or IDE on your local machine, **[CodeSandbox](https://codesandbox.io)**, an online editor, is also a viable alternative. While CodeSandbox is a great tool for sharing code online, a local machine setup is a better tool for learning the different ways to create a web application. Also, if you want to develop applications professionally, a local setup will be required.

Throughout this learning experience, I will use the term *command line*, which will be used synonymously for the terms *command line tool*, *terminal*, and *integrated terminal*. The same applies to the terms *editor*, *text editor*, and *IDE*, depending on what you decided to use for your setup.

Optionally, I recommend managing projects on **GitHub** while we conduct the exercises in this book, and I've provided a [short guide](https://www.robinwieruch.de/git-essential-commands/) on how to use these tools. Github has excellent version control, so you can see what changes were made if you make a mistake or just want a more direct way to follow along. It's also a great way to share your code later with other people.

### Node and NPM

Before we can begin, we'll need to have **[Node and NPM](https://nodejs.org/en/)** installed. Both are used to manage libraries (node packages) that you will need along the way. These node packages can be libraries or whole frameworks. We'll install external node packages via npm (node package manager).

You can verify node and npm versions on the command line using the `node --version` and `npm --version` commands. If you don't receive output in the terminal indicating which version is installed, you need to install node and npm:

{title="Command Line",lang="text"}
~~~~~~~
node --version
*vXX.YY.ZZ
npm --version
*vXX.YY.ZZ
~~~~~~~

If you have already installed Node and npm, make sure that your installation is the most recent version. If you're new to npm or need a refresher, this [npm crash course](https://www.robinwieruch.de/npm-crash-course/) I created will get you up to speed.