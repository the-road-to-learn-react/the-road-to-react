## Linting with ESLint (Optional)

Linting is a process in programming where code is analyzed for potential errors, bugs, and style issues. It is an important step in the cycle of software development as it helps to identify problems early on and improves the overall quality of the code. Linting can also enforce specific coding standards, like having or not having always a semicolon at the end of a JavaScript statement, which makes code more consistent and easier to maintain.

[ESLint](http://bit.ly/3HAHuJg) is a popular linting tool for JavaScript. It allows developers to define a set of rules for their code, including coding style and syntax, and then checks the code against those rules. ESLint is highly configurable, with a large number of rules and plugins available, making it a versatile tool for any JavaScript project. It can be integrated into an IDE/editor or run as part of a build process, making it easy to use and a popular choice for many JavaScript developers.

We will make the case for using linting (general programming concept) with ESLint (JavaScript tool for linting) in this section, because it catches errors early. In your file for the App component, adjust the JSX with an inlined `"Hello World"` without using the `title` variable anymore:

{title="src/App.jsx",lang="javascript"}
~~~~~~~
import * as React from 'react';

const title = 'React';

function App() {
  return (
    <div>
# leanpub-start-insert
      <h1>Hello World</h1>
# leanpub-end-insert

      <label htmlFor="search">Search: </label>
      <input id="search" type="text" />
    </div>
  );
}

export default App;
~~~~~~~

If there is no Linting/ESLint setup, you will most likely not getting notified in your IDE or on your command line about the unused `title` variable. However, it would be good to catch such issues early, because sooner or later you may end up with lots of unused variables (and other code style issues) in your code base.

Now since we created the project with Vite, we can rely on Vite's plugins to integrate ESLint properly. On the command line, install the respective plugin:

{title="Command Line",lang="text"}
~~~~~~~
npm install vite-plugin-eslint --save-dev
~~~~~~~

Next we need to integrate the plugin in the project's configuration. Essentially Vite's configuration file, called *vite.config.js*, allows us to customize the development and build process of a Vite-based project. It gives us options such as setting the public path, configure plugins, and modify the build output. Additionally, the configuration file can be used to specify environment variables, set alias paths, and configure linting tools like ESLint. We will do the latter next by using the previously installed plugin:

{title="vite.config.jsx",lang="javascript"}
~~~~~~~
import { defineConfig } from 'vite';
import react from '@vitejs/plugin-react';
# leanpub-start-insert
import eslint from 'vite-plugin-eslint';
# leanpub-end-insert

// https://vitejs.dev/config/
export default defineConfig({
# leanpub-start-insert
  plugins: [react(), eslint()],
# leanpub-end-insert
});
~~~~~~~

Now Vite knows about ESLint, but we do not have the actual ESLint dependency installed yet. We will install it next on the command line:

{title="Command Line",lang="text"}
~~~~~~~
npm install eslint --save-dev
~~~~~~~

Last but not least, install one of ESLint's many standardized linting configurations for a React project on the command line:

{title="Command Line",lang="text"}
~~~~~~~
npm install eslint-config-react-app --save-dev
~~~~~~~

If you start your project on the command line again, you may see the following error showing up:

{title="Command Line",lang="text"}
~~~~~~~
[vite] Internal server error: No ESLint configuration found in
~~~~~~~

Therefore we will create an ESLint configuration file to define our linting rules:

{title="Command Line",lang="text"}
~~~~~~~
touch .eslintrc
~~~~~~~

While it is possible to define your own rules in this file, we will tell ESLint to use the previously installed standardized set of rules from the eslint-config-react-app dependency:

{title=".eslintrc",lang="text"}
~~~~~~~
{
  "extends": [
    "react-app"
  ]
}
~~~~~~~

The *.eslintrc* configuration file essentially specifies linting rules and settings for a project. It allows us to configure the behavior of ESLint, including specifying language version, setting rules, and using plugins. The configuration file can also extend (what we did) or overwrite rules from an extended configuration. Finally when starting the application on the command line, you will see the following warning popping up:

{title="Command Line",lang="text"}
~~~~~~~
3:7  warning  'title' is assigned a value but never used  no-unused-vars

✖ 1 problem (0 errors, 1 warning)
~~~~~~~

In your IDE (e.g. VSCode), you can also install the ESLint Extension (e.g. by Microsoft). Then you will see the following warning popping up in your file:

{title="IDE Output",lang="text"}
~~~~~~~
const title: "React"
'title' is assigned a value but never used
~~~~~~~

If the warning is not showing in your IDE up after installing the extension, you may want to restart the IDE. In conclusion, the use of ESLint in React projects is highly recommended for maintaining code quality and consistency. Since React is a widely used JavaScript library, there are a wide range of configurations like eslint-config-react-app (see `react-app` in ESLint configuration file) which you can take off the shelf. Using such a common sense configuration ensures that the written code meets specific coding standards.

### Exercises:

* Compare your source code against the author's [source code](http://bit.ly/3XjEgQx).
  * Recap all the [source code changes from this section](https://bit.ly/3Rz8Fca).
* Optional: Read more about [ESLint](http://bit.ly/3HAHuJg).
* Optional: Read more about [eslint-config-react-app](http://bit.ly/3kZaAKM).
* Read more about [configuring Vite](http://bit.ly/3HWvNOs).