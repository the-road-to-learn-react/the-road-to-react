## TypeScript in React

TypeScript for JavaScript and React has many benefits for developing robust applications. Instead of getting type errors on runtime on the command line or browser, TypeScript integration presents them during compile time inside the IDE. It shortens the feedback loop for a developer, while it improves the developer experience. In addition, the code becomes more self-documenting and readable, because every variable is defined with a type. Also moving code blocks or performing a larger refactoring of a codebase becomes much more efficient. Statically typed languages like TypeScript are trending because of their benefits over dynamically typed languages like JavaScript. It's useful to learn more [about TypeScript](https://bit.ly/3G0l3vL) whenever possible.

### TypeScript Setup

To use TypeScript in React (with Vite), install TypeScript and its dependencies into your application using the command line:

{title="Command Line",lang="text"}
~~~~~~~
npm install typescript @types/react @types/react-dom --save-dev
npm install @typescript-eslint/eslint-plugin --save-dev
npm install @typescript-eslint/parser --save-dev
~~~~~~~

Add three TypeScript configuration files; one for the browser environment, one for the Node environment, and one to merge both configurations:

{title="Command Line",lang="text"}
~~~~~~~
touch tsconfig.json tsconfig.app.json tsconfig.node.json
~~~~~~~

In the TypeScript file for the browser environment include the following configuration:

{title="tsconfig.app.json",lang="javascript"}
~~~~~~~
{
  "compilerOptions": {
    "tsBuildInfoFile": "./node_modules/.tmp/tsconfig.app.tsbuildinfo",
    "target": "ES2020",
    "useDefineForClassFields": true,
    "lib": ["ES2020", "DOM", "DOM.Iterable"],
    "module": "ESNext",
    "skipLibCheck": true,

    /* Bundler mode */
    "moduleResolution": "bundler",
    "allowImportingTsExtensions": true,
    "isolatedModules": true,
    "moduleDetection": "force",
    "noEmit": true,
    "jsx": "react-jsx",

    /* Linting */
    "strict": true,
    "noUnusedLocals": true,
    "noUnusedParameters": true,
    "noFallthroughCasesInSwitch": true,
    "noUncheckedSideEffectImports": true
  },
  "include": ["src"]
}
~~~~~~~

Then In the TypeScript file for the Node environment include some more configuration:

{title="tsconfig.node.json",lang="javascript"}
~~~~~~~
{
  "compilerOptions": {
    "tsBuildInfoFile": "./node_modules/.tmp/tsconfig.node.tsbuildinfo",
    "target": "ES2022",
    "lib": ["ES2023"],
    "module": "ESNext",
    "skipLibCheck": true,

    /* Bundler mode */
    "moduleResolution": "bundler",
    "allowImportingTsExtensions": true,
    "isolatedModules": true,
    "moduleDetection": "force",
    "noEmit": true,

    /* Linting */
    "strict": true,
    "noUnusedLocals": true,
    "noUnusedParameters": true,
    "noFallthroughCasesInSwitch": true,
    "noUncheckedSideEffectImports": true
  },
  "include": ["vite.config.ts"]
}
~~~~~~~

Finally merge both configurations into the main TypeScript configuration file:

{title="tsconfig.json",lang="javascript"}
~~~~~~~
{
  "files": [],
  "references": [
    { "path": "./tsconfig.app.json" },
    { "path": "./tsconfig.node.json" }
  ]
}
~~~~~~~

If you have a ESLint configuration, you need to adapt it to TypeScript too:

{title="eslint.config.js",lang="javascript"}
~~~~~~~
import js from "@eslint/js";
import globals from "globals";
import reactHooks from "eslint-plugin-react-hooks";
import reactRefresh from "eslint-plugin-react-refresh";
import tseslint from "typescript-eslint";

export default tseslint.config(
  { ignores: ["dist"] },
  {
    extends: [js.configs.recommended, ...tseslint.configs.recommended],
    files: ["**/*.{ts,tsx}"],
    languageOptions: {
      ecmaVersion: 2020,
      globals: globals.browser,
    },
    plugins: {
      "react-hooks": reactHooks,
      "react-refresh": reactRefresh,
    },
    rules: {
      ...reactHooks.configs.recommended.rules,
      "react/prop-types": "off",
      "react-refresh/only-export-components": [
        "warn",
        { allowConstantExport: true },
      ],
    },
  }
);
~~~~~~~

Next, rename all JavaScript files (*.jsx*) to TypeScript files (*.tsx*).

{title="Command Line",lang="text"}
~~~~~~~
mv src/main.jsx src/main.tsx
mv src/App.jsx src/App.tsx
~~~~~~~

And in your *index.html* file, reference the new TypeScript file instead of a JavaScript file:

{title="index.html",lang="html"}
~~~~~~~
<!doctype html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <link rel="icon" type="image/svg+xml" href="/vite.svg" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Vite + React</title>
  </head>
  <body>
    <div id="root"></div>
# leanpub-start-insert
    <script type="module" src="/src/main.tsx"></script>
# leanpub-end-insert
  </body>
</html>
~~~~~~~

You may also need a new vite-env.d.ts file in your project's root with the following content:

{title="index.html",lang="javascript"}
~~~~~~~
/// <reference types="vite/client" />
~~~~~~~

Restart your development server on the command line. You may encounter compile errors in the browser and editor/IDE. If you don't see any errors in your editor/IDE when opening the renamed TypeScript files (e.g. *src/App.tsx*), try installing a TypeScript plugin for your editor or a TypeScript extension for your IDE. Usually you should see red lines under all the values where TypeScript definitions are missing.

### Type Safety for Functions and Components

The application should still start, however, we are missing type definitions in the *src/main.tsx* and *src/App.tsx* files. Let's start with the former one, because this is only a little change:

{title="src/main.tsx",lang="javascript"}
~~~~~~~
# leanpub-start-insert
createRoot(document.getElementById("root")!).render(
# leanpub-end-insert
  <StrictMode>
    <App />
  </StrictMode>
);
~~~~~~~

Without this change, TypeScript should output us the following error: *Argument of type 'HTMLElement | null' is not assignable to parameter of type 'Element | DocumentFragment'.*. It can be translated as: "The returned HTML element from `getElementById()` could be null if there is no such HTML element, but `createRoot()` expects it to be an Element." Because we know for sure that there is a HTML element with this specific identifier in the *index.html* file, we are replying TypeScript with "I know better" by using a so called type assertion (here: `as` keyword) in TypeScript.

Next, we'll add [type safety](https://bit.ly/3jhm6xi) for the entire *src/App.tsx* file. When looking at a custom React hook plainly from a programming language perspective, it is just another function. In TypeScript a function's input (and optionally output) has to be type safe though. Let's start by making our `useStorageState()` hook type safe where we are telling the function to expect two arguments as string primitives:

{title="src/App.tsx",lang="javascript"}
~~~~~~~
# leanpub-start-insert
const useStorageState = (key: string, initialState: string) => {
# leanpub-end-insert
  ...
};
~~~~~~~

Also, we can tell the function to return an array (`[]`) with a first value (current state) of type `string` and a second value (state updater function) that takes a new value (new state) of type `string` to return nothing (`void`):

{title="src/App.tsx",lang="javascript"}
~~~~~~~
const useStorageState = (
  key: string,
  initialState: string
# leanpub-start-insert
): [string, (newValue: string) => void] => {
# leanpub-end-insert
  ...

  return [value, setValue];
};
~~~~~~~

Since TypeScript could already infer this type from React's useState Hook, we could simply remove the return type again. However, we need to declare the returned array as TypeScript `const`, because otherwise the order of the entries in the array would not be known to other parts of the application:

{title="src/App.tsx",lang="javascript"}
~~~~~~~
# leanpub-start-insert
const useStorageState = (key: string, initialState: string) => {
# leanpub-end-insert
  const [value, setValue] = React.useState(
    localStorage.getItem(key) || initialState
  );

  React.useEffect(() => {
    localStorage.setItem(key, value);
  }, [value, key]);

# leanpub-start-insert
  return [value, setValue] as const;
# leanpub-end-insert
};
~~~~~~~

Related to React though, considering the previous type safety improvements for the custom hook, we hadn't to add types to the internal React hooks in the function's body. That's because **type inference** works most of the time for React hooks out of the box. If the *initial state* of a React useState Hook is a JavaScript string primitive, then the returned *current state* will be inferred as a string and the returned *state updater function* will only take a string as an argument and return nothing:

{title="Code Playground",lang="javascript"}
~~~~~~~
const [value, setValue] = React.useState('React');
// value is inferred to be a string
// setValue only takes a string as argument
~~~~~~~

However, if the *initial state* would be `null` initially, we would have to tell TypeScript all of React's useState Hook potential types (here with a so called union type in TypeScript where `|` makes a union of two or more types). A [TypeScript generic](https://www.robinwieruch.de/typescript-generics/) is used to tell the function (here: a React hook) about it:

{title="Code Playground",lang="javascript"}
~~~~~~~
const [value, setValue] = React.useState<string | null>(null);
// value has to be either a string or null
// setValue only takes a string or null as argument
~~~~~~~

If adding type safety becomes an aftermath for a React application and its components, like in our case, there are multiple ways on how to approach it. We will start with the props and state for the leaf components of our application. For example, the Item component receives a story (here: `item`) and a callback handler function (here: `onRemoveItem`). Starting out very verbose, we could add the inlined types for both function arguments as we did before:

{title="src/App.tsx",lang="javascript"}
~~~~~~~
const Item = ({
  item,
  onRemoveItem,
# leanpub-start-insert
}: {
  item: {
    objectID: string;
    url: string;
    title: string;
    author: string;
    num_comments: number;
    points: number;
  };
  onRemoveItem: (item: {
    objectID: string;
    url: string;
    title: string;
    author: string;
    num_comments: number;
    points: number;
  }) => void;
}) => (
# leanpub-end-insert
  <li>
    ...
  </li>
);
~~~~~~~

There are two problems: the code is verbose, and it has duplicates (see: `item`). Let's get rid of both problems by defining a custom `Story` type outside the component, at the top of *src/App.jsx*:

{title="src/App.tsx",lang="javascript"}
~~~~~~~
# leanpub-start-insert
type Story = {
  objectID: string;
  url: string;
  title: string;
  author: string;
  num_comments: number;
  points: number;
};
# leanpub-end-insert

...

const Item = ({
  item,
  onRemoveItem,
# leanpub-start-insert
}: {
  item: Story;
  onRemoveItem: (item: Story) => void;
}) => (
# leanpub-end-insert
  <li>
    ...
  </li>
);
~~~~~~~

The `item` is of type `Story` and the `onRemoveItem` function takes an `item` of type `Story` as an argument and returns nothing. Next, clean up the code by defining the props of the Item component as type outside of it:

{title="src/App.tsx",lang="javascript"}
~~~~~~~
# leanpub-start-insert
type ItemProps = {
  item: Story;
  onRemoveItem: (item: Story) => void;
};
# leanpub-end-insert

# leanpub-start-insert
const Item = ({ item, onRemoveItem }: ItemProps) => (
# leanpub-end-insert
  <li>
    ...
  </li>
);
~~~~~~~

From here, we can navigate up the component tree into the List component and apply the same type definitions for the props. First try it yourself and then check out the following implementation:

{title="src/App.tsx",lang="javascript"}
~~~~~~~
# leanpub-start-insert
type ListProps = {
  list: Story[];
  onRemoveItem: (item: Story) => void;
};
# leanpub-end-insert

# leanpub-start-insert
const List = ({ list, onRemoveItem }: ListProps) => (
# leanpub-end-insert
  <ul>
    ...
  </ul>
);
~~~~~~~

The `onRemoveItem` function is typed twice for the `ItemProps` and `ListProps` now. To be more accurate, you *could* extract this to a standalone defined `OnRemoveItem` TypeScript type and reuse it for both `onRemoveItem` prop type definitions. Note, however, that development becomes increasingly difficult as components are split up into different files. That's why we will keep the duplication here.

Next we can repurpose the `Story` type for other components. For instance, add the `Story` type to the callback handler in the `App` component:

{title="src/App.tsx",lang="javascript"}
~~~~~~~
const App = () => {
  ...

# leanpub-start-insert
  const handleRemoveStory = (item: Story) => {
# leanpub-end-insert
    dispatchStories({
      type: 'REMOVE_STORY',
      payload: item,
    });
  };

  ...
};
~~~~~~~

The reducer function manages the `Story` type as well, without really touching it due to `state` and `action` types. As the application's developer, we know both objects and their shapes that are passed to this reducer function:

{title="src/App.tsx",lang="javascript"}
~~~~~~~
# leanpub-start-insert
type StoriesState = {
  data: Story[];
  isLoading: boolean;
  isError: boolean;
};

type StoriesAction = {
  type: string;
  payload: any;
};

const storiesReducer = (
  state: StoriesState,
  action: StoriesAction
) => {
# leanpub-end-insert
  ...
};
~~~~~~~

The `Action` type with its `string` and `any` (TypeScript **wildcard**) type definitions are still too broad; and we gain no real type safety through it, because actions are not distinguishable. We can do better by specifying each action as a TypeScript type and using a union type (here: `StoriesAction`) for the final type safety:

{title="src/App.tsx",lang="javascript"}
~~~~~~~
# leanpub-start-insert
type StoriesFetchInitAction = {
  type: 'STORIES_FETCH_INIT';
}

type StoriesFetchSuccessAction = {
  type: 'STORIES_FETCH_SUCCESS';
  payload: Story[];
}

type StoriesFetchFailureAction = {
  type: 'STORIES_FETCH_FAILURE';
}

type StoriesRemoveAction = {
  type: 'REMOVE_STORY';
  payload: Story;
}

type StoriesAction =
  StoriesFetchInitAction
  | StoriesFetchSuccessAction
  | StoriesFetchFailureAction
  | StoriesRemoveAction;
# leanpub-end-insert

const storiesReducer = (
  state: StoriesState,
  action: StoriesAction
) => {
  ...
};
~~~~~~~

The reducer's current state, action, and returned state (inferred) are type safe now. For example, if you would dispatch an action to the reducer with an action type that's not defined, you would get an error from TypeScript. Or if you would pass something else than a story to the action which removes a story, you would get a type error as well. Now let's shift our focus to the SearchForm component, which has callback handlers with events:

{title="src/App.tsx",lang="javascript"}
~~~~~~~
# leanpub-start-insert
type SearchFormProps = {
  searchTerm: string;
  onSearchInput: (event: React.ChangeEvent<HTMLInputElement>) => void;
  searchAction: (formData: FormData) => void;
};
# leanpub-end-insert

const SearchForm = ({
  searchTerm,
  onSearchInput,
  searchAction,
# leanpub-start-insert
}: SearchFormProps) => (
# leanpub-end-insert
  ...
);
~~~~~~~

Often using `React.SyntheticEvent` instead of `React.ChangeEvent` or `React.FormEvent` is usually sufficient. However, most often your applications requires a more specific type. Next, going up to the App component again, we apply the same type for the callback handler there:

{title="src/App.tsx",lang="javascript"}
~~~~~~~
const App = () => {
  ...

  const handleSearchInput = (
# leanpub-start-insert
    event: React.ChangeEvent<HTMLInputElement>
# leanpub-end-insert
  ) => {
    setSearchTerm(event.target.value);
  };

  ...
};
~~~~~~~

All that's left is the InputWithLabel component. Before handling this component's props, let's take a look at the `ref` from React's useRef Hook. Unfortunately, the return value isn't inferred:

{title="src/App.tsx",lang="javascript"}
~~~~~~~
const InputWithLabel = ({ ... }) => {
# leanpub-start-insert
  const inputRef = React.useRef<HTMLInputElement>(null);
# leanpub-end-insert

  React.useEffect(() => {
    if (isFocused && inputRef.current) {
      inputRef.current.focus();
    }
  }, [isFocused]);
~~~~~~~

We made the returned `ref` type safe and typed it as read-only, because we only execute the `focus` method on it (read). React takes over for us there, setting the DOM element to the `current` property.

Lastly, we will apply type safety checks for the InputWithLabel component's props. Note the `children` prop with its React specific type and the **optional types** are signaled with a question mark:

{title="src/App.tsx",lang="javascript"}
~~~~~~~
# leanpub-start-insert
type InputWithLabelProps = {
  id: string;
  value: string;
  type?: string;
  onInputChange: (event: React.ChangeEvent<HTMLInputElement>) => void;
  isFocused?: boolean;
  children: React.ReactNode;
};
# leanpub-end-insert

const InputWithLabel = ({
  id,
  value,
  type = 'text',
  onInputChange,
  isFocused,
  children,
# leanpub-start-insert
}: InputWithLabelProps) => {
# leanpub-end-insert
  ...
};
~~~~~~~

Both the `type` and `isFocused` properties are optional. Using TypeScript, you can tell the compiler that these don't need to be passed to the component as props. The `children` prop has a lot of TypeScript type definitions that could be applicable to this concept, the most universal of which is `React.ReactNode` from the React library.

Our entire React application is finally typed by TypeScript, making it easy to spot type errors on compile time. When adding TypeScript to your React application, start by adding type definitions to your function's arguments. These functions can be vanilla JavaScript functions, custom React hooks, or React function components. Only when using React is it important to know specific types for form elements, events, and JSX.

### Exercises:

* Compare your source code against the author's [source code](https://tinyurl.com/4v7ecp5w).
  * Recap all the [source code changes](https://tinyurl.com/8umbbynt) from this section.
* While you continue with the learning experience in the following sections, remove or keep your types with TypeScript. If you do the latter, add new types whenever you get a compile error.