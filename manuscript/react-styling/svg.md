## SVGs in React

To create a modern React application, we'll likely need to use SVGs. Instead of giving every button element text, for example, we might want to make it lightweight with an icon. In this section, we'll use a scalable vector graphic (SVG) as an icon in one of our React components.

**Important:** This section builds on the "CSS in React" we covered earlier which helps us giving the SVG icon a good look and feel right away. It's acceptable to use a different styling approach (e.g. CSS Modules, Styled Components), or no styling at all, though the SVG might look off without it.

Vite does not come with SVG support. In order to allow SVGs in Vite, we have to install one of Vite's plugins with the help of the command line:

{title="Command Line",lang="text"}
~~~~~~~
npm install vite-plugin-svgr --save-dev
~~~~~~~

Next the new plugin for SVGs can be used for Vite's configuration:

{title="src/App.jsx",lang="javascript"}
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

That's it for the general setup. The following icon as SVG is taken from [Flaticon's Freepick](https://bit.ly/3E16SEz). Many of the SVGs on this website are free to use, though they require you to mention the author. You can download the icon from [here](https://bit.ly/2Z2EoeA) as SVG and put it in your project as *src/check.svg*. Downloading the file is the recommended way, however, for the sake of completion, this is the verbose SVG definition:

{title="src/check.svg",lang="html"}
~~~~~~~
<?xml version="1.0" encoding="iso-8859-1"?>
<!-- Generator: Adobe Illustrator 18.0.0, SVG Export Plug-In . SVG Version: 6.00 Build 0)  -->
<!DOCTYPE svg PUBLIC "-//W3C//DTD SVG 1.1//EN" "http://www.w3.org/Graphics/SVG/1.1/DTD/svg11.dtd">
<svg version="1.1" id="Capa_1" xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink" x="0px" y="0px"
   viewBox="0 0 297 297" style="enable-background:new 0 0 297 297;" xml:space="preserve">
  <g>
    <path d="M113.636,272.638c-2.689,0-5.267-1.067-7.168-2.97L2.967,166.123c-3.956-3.957-3.956-10.371-0.001-14.329l54.673-54.703
      c1.9-1.9,4.479-2.97,7.167-2.97c2.689,0,5.268,1.068,7.169,2.969l41.661,41.676L225.023,27.332c1.9-1.901,4.48-2.97,7.168-2.97l0,0
      c2.688,0,5.268,1.068,7.167,2.97l54.675,54.701c3.956,3.957,3.956,10.372,0,14.328L120.803,269.668
      C118.903,271.57,116.325,272.638,113.636,272.638z M24.463,158.958l89.173,89.209l158.9-158.97l-40.346-40.364L120.803,160.264
      c-1.9,1.902-4.478,2.971-7.167,2.971c-2.688,0-5.267-1.068-7.168-2.97l-41.66-41.674L24.463,158.958z"/>
  </g>
</svg>
~~~~~~~

Now we can import SVGs (similar to CSS) as React components right away. In *src/App.jsx*, use the following syntax for importing the SVG:

{title="src/App.jsx",lang="javascript"}
~~~~~~~
import * as React from 'react';
import axios from 'axios';

import './App.css';

# leanpub-start-insert
import { ReactComponent as Check } from './check.svg';
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

Regardless of the styling approach you are using, you can give your SVG icon in the button a hover effect too, because right now it doesn't look right for the hover state. In the basic CSS approach, it would look like the following in the *src/App.css* file:

{title="src/App.css",lang="css"}
~~~~~~~
.button:hover > svg > g {
  fill: #ffffff;
  stroke: #ffffff;
}
~~~~~~~

The Vite plugin makes using SVGs straightforward, with not much extra configuration needed. This is different if you create a React project from scratch with build tools like Webpack, because you have to take care of it yourself. Anyway, SVGs make your application more approachable, so use them whenever it suits you.

### Exercises:

* Compare your source code against the author's [source code](https://bit.ly/3DNI6v5).
  * Recap all the [source code changes from this section](https://bit.ly/3xLtOY6).
* Add another SVG icon in your application.
* Integrate the third-party library [react-icons](https://bit.ly/3nayoJ7) into your application and use its SVG symbols by importing them as React components right away.
* Optional: [Leave feedback for this section](https://forms.gle/3yGgMDR2VQ5WksSXA).