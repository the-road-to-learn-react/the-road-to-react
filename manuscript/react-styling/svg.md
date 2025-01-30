## SVGs in React

To create a modern React application, we'll likely need to use SVGs. Instead of giving every button element text, for example, we might want to make it lightweight with an icon. In this section, we'll use a scalable vector graphic (SVG) as an icon in one of our React components.

**Important:** This section builds on the "CSS in React" we covered earlier which helps us giving the SVG icon a good look and feel right away. It's acceptable to use a different styling approach (e.g. CSS Modules, Styled Components), or no styling at all, though the SVG might look off without it.

Vite does not come with SVG support. In order to allow SVGs in Vite, we have to install one of Vite's plugins with the help of the command line:

{title="Command Line",lang="text"}
~~~~~~~
npm install vite-plugin-svgr --save-dev
~~~~~~~

Next the new plugin for SVGs can be used for Vite's configuration:

{title="vite.config.js",lang="javascript"}
~~~~~~~
import { defineConfig } from 'vite';
import react from '@vitejs/plugin-react';
# leanpub-start-insert
import svgr from 'vite-plugin-svgr';
# leanpub-end-insert

// https://vitejs.dev/config/
export default defineConfig({
# leanpub-start-insert
  plugins: [react(), svgr()],
# leanpub-end-insert
});
~~~~~~~

That's it for the general setup. We will use the following [SVG](https://bit.ly/3w4xNRz) from [Heroicons](https://heroicons.com/) and create a new *src/check.svg* file:

{title="src/check.svg",lang="html"}
~~~~~~~
<svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="1.5" stroke="currentColor" class="size-6">
  <path stroke-linecap="round" stroke-linejoin="round" d="m4.5 12.75 6 6 9-13.5" />
</svg>
~~~~~~~

Now we can import SVGs (similar to CSS) as React components right away. In *src/App.jsx*, use the following syntax for importing the SVG:

{title="src/App.jsx",lang="javascript"}
~~~~~~~
import * as React from 'react';
import axios from 'axios';
import './App.css';
# leanpub-start-insert
import Check from "./check.svg?react";
# leanpub-end-insert
~~~~~~~

Here we are importing an SVG to be used as icon. However, this works for many different uses cases such as logos and backgrounds. Now, instead of the button "Dismiss" text, pass the SVG component with a `height` and `width` attribute:

{title="src/App.jsx",lang="javascript"}
~~~~~~~
const Item = ({ item, onRemoveItem }) => (
  <li className="item">
    <span style={{ width: '40%' }}>
      <a href={item.url}>{item.title}</a>
    </span>
    <span style={{ width: '30%' }}>{item.author}</span>
    <span style={{ width: '10%' }}>{item.num_comments}</span>
    <span style={{ width: '10%' }}>{item.points}</span>
    <span style={{ width: '10%' }}>
      <button
        type="button"
        onClick={() => onRemoveItem(item)}
        className="button button_small"
      >
# leanpub-start-insert
        <Check height="18px" width="18px" />
# leanpub-end-insert
      </button>
    </span>
  </li>
);
~~~~~~~

The Vite plugin makes using SVGs straightforward, with not much extra configuration needed. This is different if you create a React project from scratch with build tools like Webpack, because you have to take care of it yourself. Anyway, SVGs make your application more approachable, so use them whenever it suits you.

### Exercises:

* Compare your source code against the author's [source code](https://tinyurl.com/3nf39wy3).
  * Recap all the [source code changes](https://tinyurl.com/mrsfx32y) from this section.
* Integrate the third-party library [react-icons](https://bit.ly/3nayoJ7) into your application and use its SVG symbols by importing them as React components right away.