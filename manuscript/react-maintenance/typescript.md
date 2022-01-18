## TypeScript in React

TypeScript for JavaScript and React has many benefits for developing robust applications. Instead of getting type errors on runtime in the command line or browser, TypeScript integration presents them during compile time inside the IDE. It shortens the feedback loop for a developer, while it improves the developer experience. The code becomes more self-documenting and readable, because every variable is defined with a type. Also moving code blocks or performing a larger refactoring of a codebase becomes much more efficient. Statically typed languages like TypeScript are trending because of their benefits over dynamically typed languages like JavaScript. It's useful to learn more [about TypeScript](https://bit.ly/3G0l3vL) whenever possible.

To use TypeScript in React, install TypeScript and its dependencies into your application using the command line. If you run into obstacles, follow the official TypeScript installation instructions for [create-react-app](https://bit.ly/3DYG880):

{title="Command Line",lang="text"}
~~~~~~~
npm install --save typescript @types/node @types/react
npm install --save typescript @types/react-dom @types/jest
~~~~~~~

Next, rename all JavaScript files (*.js*) to TypeScript files (*.tsx*).

{title="Command Line",lang="text"}
~~~~~~~
mv src/index.js src/index.tsx
mv src/App.js src/App.tsx
~~~~~~~

Restart your development server in the command line. You may encounter compile errors in the browser and IDE. If the latter doesn't work, try installing a TypeScript plugin for your editor, or extension for your IDE. After the initial TypeScript in React setup, we'll add [type safety](https://bit.ly/3jhm6xi) for the entire *src/App.tsx* file, starting with typing the arguments of the custom hook:

{title="src/App.tsx",lang="javascript"}
~~~~~~~
const useStorageState = (
# leanpub-start-insert
  key: string,
  initialState: string
# leanpub-end-insert
) => {
  const [value, setValue] = React.useState(
    localStorage.getItem(key) || initialState
  );

  React.useEffect(() => {
    localStorage.setItem(key, value);
  }, [value, key]);

  return [value, setValue];
};
~~~~~~~

Adding types to the function's arguments is more about Javascript than React. We are telling the function to expect two arguments, which are JavaScript string primitives. Also, we can tell the function to return an array (`[]`) with a `string` (state), and tell functions like the `state updater function` that takes a `value` to return nothing (`void`):

{title="src/App.tsx",lang="javascript"}
~~~~~~~
const useStorageState = (
  key: string,
  initialState: string
# leanpub-start-insert
): [string, (newValue: string) => void] => {
# leanpub-end-insert
  const [value, setValue] = React.useState(
    localStorage.getItem(key) || initialState
  );

  React.useEffect(() => {
    localStorage.setItem(key, value);
  }, [value, key]);

  return [value, setValue];
};
~~~~~~~

Related to React though, considering the previous type safety improvements for the custom hook, we hadn't to add types to the internal React hooks in the function's body. That's because **type inference** works most of the time for React hooks out of the box. If the *initial state* of a React useState Hook is a JavaScript string primitive, then the returned *current state* will be inferred as a string and the returned *state updater function* will only take a string as an argument and return nothing:

{title="Code Playground",lang="javascript"}
~~~~~~~
const [value, setValue] = React.useState('React');
// value is inferred to be a string
// setValue only takes a string as argument
~~~~~~~

If adding type safety becomes an aftermath for a React application and its components, there are multiple ways on how to approach it. We will start with the props and state for the leaf components of our application. For example, the Item component receives a story (here: `item`) and a callback handler function (here: `onRemoveItem`). Starting out very verbose, we could add the inlined types for both function arguments as we did before:

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

There are two problems: the code is verbose, and it has duplicates. Let's get rid of both problems by defining a custom `Story` type outside the component, at the top of *src/App.js*:

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

The `item` is of type `Story`; the `onRemoveItem` function takes an `item` of type `Story` as an argument and returns nothing. Next, clean up the code by defining the props of the Item component outside:

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

That's the most popular way to type React component's props with TypeScript. Fortunately, the return type of the function component is inferred. However, if you want to explicitly use it, you can do so with `JSX.Element`. From here, we can navigate up the component tree into the List component and apply the same type definitions for the props:

{title="src/App.tsx",lang="javascript"}
~~~~~~~
type Story = {
  ...
};

# leanpub-start-insert
type Stories = Array<Story>;
# leanpub-end-insert

...

# leanpub-start-insert
type ListProps = {
  list: Stories;
  onRemoveItem: (item: Story) => void;
};
# leanpub-end-insert

# leanpub-start-insert
const List = ({ list, onRemoveItem }: ListProps) => (
# leanpub-end-insert
  <ul>
    {list.map((item) => (
      <Item
        key={item.objectID}
        item={item}
        onRemoveItem={onRemoveItem}
      />
    ))}
  </ul>
);
~~~~~~~

The `onRemoveItem` function is typed twice for the `ItemProps` and `ListProps`. To be more accurate, you *could* extract this to a standalone defined `OnRemoveItem` TypeScript type and reuse it for both `onRemoveItem` prop type definitions. Note, however, that development becomes increasingly difficult as components are split up into different files. That's why we will keep the duplication here. Now, since we already have the `Story` and `Stories` types, we can repurpose them for other components. Add the `Story` type to the callback handler in the `App` component:

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
  data: Stories;
  isLoading: boolean;
  isError: boolean;
};
# leanpub-end-insert

# leanpub-start-insert
type StoriesAction = {
  type: string;
  payload: any;
};
# leanpub-end-insert

# leanpub-start-insert
const storiesReducer = (
  state: StoriesState,
  action: StoriesAction
) => {
# leanpub-end-insert
  ...
};
~~~~~~~

The `Action` type with its `string` and `any` (TypeScript **wildcard**) type definitions are still too broad; and we gain no real type safety through it, because actions are not distinguishable. We can do better by specifying each action TypeScript type as an **interface**, and using a **union type** (here: `StoriesAction`) for the final type safety:

{title="src/App.tsx",lang="javascript"}
~~~~~~~
# leanpub-start-insert
interface StoriesFetchInitAction {
  type: 'STORIES_FETCH_INIT';
}

interface StoriesFetchSuccessAction {
  type: 'STORIES_FETCH_SUCCESS';
  payload: Stories;
}

interface StoriesFetchFailureAction {
  type: 'STORIES_FETCH_FAILURE';
}

interface StoriesRemoveAction {
  type: 'REMOVE_STORY';
  payload: Story;
}

type StoriesAction =
  | StoriesFetchInitAction
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

The reducer's current state, action, and returned state (inferred) are type safe now. For example, if you would dispatch an action to the reducer with an action type that's not defined, you would get a type error. Or if you would pass something else than a story to the action which removes a story, you would get a type error as well.

Let's shift our focus to the SearchForm component, which has callback handlers with events:

{title="src/App.tsx",lang="javascript"}
~~~~~~~
# leanpub-start-insert
type SearchFormProps = {
  searchTerm: string;
  onSearchInput: (event: React.ChangeEvent<HTMLInputElement>) => void;
  onSearchSubmit: (event: React.FormEvent<HTMLFormElement>) => void;
};
# leanpub-end-insert

const SearchForm = ({
  searchTerm,
  onSearchInput,
  onSearchSubmit,
# leanpub-start-insert
}: SearchFormProps) => (
# leanpub-end-insert
  ...
);
~~~~~~~

Often using `React.SyntheticEvent` instead of `React.ChangeEvent` or `React.FormEvent` is usually enough. Going up to the App component again, we apply the same type for the callback handler there:

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

  const handleSearchSubmit = (
# leanpub-start-insert
    event: React.FormEvent<HTMLFormElement>
# leanpub-end-insert
  ) => {
    setUrl(`${API_ENDPOINT}${searchTerm}`);

    event.preventDefault();
  };

  ...
};
~~~~~~~

All that's left is the InputWithLabel component. Before handling this component's props, let's take a look at the `ref` from React's useRef Hook. Unfortunately, the return value isn't inferred:

{title="src/App.tsx",lang="javascript"}
~~~~~~~
const InputWithLabel = ({ ... }) => {
# leanpub-start-insert
  const inputRef = React.useRef<HTMLInputElement>(null!);
# leanpub-end-insert

  React.useEffect(() => {
    if (isFocused && inputRef.current) {
      inputRef.current.focus();
    }
  }, [isFocused]);
~~~~~~~

We made the returned `ref` type safe, and typed it as read-only because we only execute the `focus` method on it (read). React takes over for us there, setting the DOM element to the `current` property.

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

* Confirm your [source code](https://bit.ly/3vtF8p4).
  * Confirm the [changes](https://bit.ly/3pjxlJr).
* Dig into the [React + TypeScript Cheatsheet](https://bit.ly/3phdf2H), because most common use cases we faced in this section are covered there as well. There is no need to know everything from the top of your head.
* While you continue with the learning experience in the following sections, remove or keep your types with TypeScript. If you do the latter, add new types whenever you get a compile error.
* Optional: [Leave feedback for this section](https://forms.gle/Pyw2oUjXV85hwk2t6).