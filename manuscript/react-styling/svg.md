## SVGs in React

To create a modern React application, we'll likely need to use SVGs. Instead of giving every button element text, for example, we might want to make it lightweight with an icon. In this section, we'll use a scalable vector graphic (SVG) as an icon in one of our React components.

This section builds on the "CSS in React" we covered earlier, to give the SVG icon a good look and feel right away. It's acceptable to use a different styling approach, or no styling at all, though the SVG might look off without it.

This icon as SVG is taken from [Flaticon's Freepick](https://www.flaticon.com/authors/freepik). Many of the SVGs on this website are free to use, though they require you to mention the author. You can download the icon from [here](https://www.flaticon.com/free-icon/check_109748) as SVG and put it in your project as *src/check.svg*. Downloading the file is the recommended way, however, for the sake of completion, this is the verbose SVG definition:

{title="Code Playground",lang="html"}
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

Because we're using create-react-app again , we can  import SVGs (similar to CSS) as React components right away. In *src/App.js*, use the following syntax for importing the SVG:

{title="src/App.js",lang="javascript"}
~~~~~~~
import React from 'react';
import axios from 'axios';

import './App.css';
# leanpub-start-insert
import { ReactComponent as Check } from './check.svg';
# leanpub-end-insert
~~~~~~~

We are importing an SVG, and this works for many different uses for SVGs (e.g. logo, background). Instead of a button text, pass the SVG component as a `height` and `width` attribute:

{title="src/App.js",lang="javascript"}
~~~~~~~
const Item = ({ item, onRemoveItem }) => (
  <div className="item">
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
  </div>
);
~~~~~~~

Regardless of the styling approach you are using, you can give your SVG icon in the button a hover effect too. In the basic CSS approach, it would look like the following in the *src/App.css* file:

{title="src/App.css",lang="css"}
~~~~~~~
.button:hover > svg > g {
  fill: #ffffff;
  stroke: #ffffff;
}
~~~~~~~

The create-react-app project makes using SVGs straightforward, with no extra configuration needed. This is different if you create a React project from scratch with build tools like Webpack, because you have to take care of it yourself. Anyway, SVGs make your application more approachable, so use them whenever it suits you.

### Exercises:

* Confirm your [source code for the last section](https://codesandbox.io/s/github/the-road-to-learn-react/hacker-stories/tree/hs/CSS-in-React-SVG).
  * Confirm the [changes from the last section](https://github.com/the-road-to-learn-react/hacker-stories/compare/hs/CSS-in-React...hs/CSS-in-React-SVG?expand=1).
* Read more about [SVGs in create-react-app](https://create-react-app.dev/docs/adding-images-fonts-and-files).
* Read more about [SVG background patterns in React](https://www.robinwieruch.de/react-svg-patterns).
* Add another SVG icon in your application.
