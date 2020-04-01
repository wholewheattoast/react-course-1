# Week 1 Recitation

## New Course Version Just Released
A [new version of this course](https://egghead.io/courses/the-beginner-s-guide-to-react) was released on Tuesday. When it was released, the version we are using was moved to a [different URL](https://egghead.io/playlists/the-beginner-s-guide-to-react-2017-99bf). I've updated all of the links in the documentation for this course to point to the original version. Many of the earlier lessons in the new version are identical or very similar to the old version. I am still evaluating the differences, however, and I will pull in any new content that I think would be useful to this version of the course, as we go. 

## Exercise Solutions

1. Copy the boilerplate code below, and use `React.createElement` and `ReactDOM.render` to display a `span` element, within the existing `<div id="root">` element, that has `className` and `children` content of your choosing (text or elements).

    ```html
      <div id="root"></div>
      <script crossorigin src="https://unpkg.com/react@16/umd/react.development.js"></script>
      <script crossorigin src="https://unpkg.com/react-dom@16/umd/react-dom.development.js"></script>
      <script crossorigin src="https://unpkg.com/babel-standalone@6.26.0/babel.js"></script>
      <script type="text/javascript">
        // Code goes here
        const rootElement = document.getElementById("root");
        const element = React.createElement("span", {
          className: "fa fa-spinner",
          children: "Loading...",
        })
        ReactDOM.render(element, rootElement);
      </script>
    ```
    Since you'll be using JSX most of the time, you're unlikely to need to use the `React.createElement` API very often, it's good to know it's there. It's also interesting to note that when you write JSX, this is the API Babel uses when it compiles the JSX into Javascript. To see this ins action, visit https://babeljs.io/repl and put the following code into the left side of the split screen. The right side will show the actual Javascript generated by the Babel-compiled JSX.

    ```jsx
    function Hello() {
      return (
        <div>
          Hello world
        </div>
      );
    }
    ```

    You can also see this compiled Javascript by looking inside a `<script>` tag within the `<head>` element of one of the HTML files.

2. Review the [React.createElement API documentation](https://reactjs.org/docs/react-api.html#createelement), and describe the purpose of the first argument to the `createElement` function.

    The first parameter of `React.createElement` represents the the type of HTML element (e.g. `div`, `section`, etc.), or React component (e.g. `TimeGreeting`) to be created.

3. What's the purpose of the [`ReactDOM.render`](https://reactjs.org/docs/react-dom.html#render) function?

    `ReactDOM.render` renders (or "mounts") an element or component into an existing container element.

4. Refactor your code from exercise 1 to use JSX instead of `React.createElement` and `ReactDOM.render` (See lesson 3).

    ```html
      <div id="root"></div>
      <script crossorigin src="https://unpkg.com/react@16/umd/react.development.js"></script>
      <script crossorigin src="https://unpkg.com/react-dom@16/umd/react-dom.development.js"></script>
      <script crossorigin src="https://unpkg.com/babel-standalone@6.26.0/babel.js"></script>
      <script type="text/babel">
        // Code goes here
        const rootElement = document.getElementById("root");
        const element = (
          <span className="fa fa-spinner">
            Loading...
          </span>
        );
        ReactDOM.render(element, rootElement);
      </script>
    ```

5. Using the boilerplate code from exercise 1, create a functional React component (see lesson 4) called `<TimeGreeting>`, which displays the current time along with a name, which is passed in using a prop called `name` (See notes below). e.g.

    **Note:** The solution to this exercise requires you to know how to use component props, which isn't introduces until lesson 5, which was a mistake on my part. If you didn't complete this exercise this week, consider coming back and completing it after watching lesson 5.

    ```html
      <div id="root"></div>
      <script crossorigin src="https://unpkg.com/react@16/umd/react.development.js"></script>
      <script crossorigin src="https://unpkg.com/react-dom@16/umd/react-dom.development.js"></script>
      <script crossorigin src="https://unpkg.com/babel-standalone@6.26.0/babel.js"></script>
      <script type="text/babel">
        // Code goes here
        function TimeGreeting(props) {
          const time = new Date().toLocaleTimeString();
          return (
            <div>
              Hello {props.name} the time is {time}
            </div>
          );
        }

        ReactDOM.render(
          <TimeGreeting name="Tony" />,
          document.getElementById("root"),
        );
      </script>
    ```

## Notes

## Using Browsersync HTTP Server
I've added a small script from the second version of this course (called `start`) into the root of the git repo for this course. You may find it easier to use this method of running your files instead of downloading and running each file individually.

In order to use this script, you'll need to [download](https://nodejs.org/en/download/) and install NodeJS if you don't already have it. You'll also want to have a copy of the [`react-course-1`](https://github.com/programmist/react-course-1) git repo on your machine. You can run the script by opening a command terminal, navigating to the `react-course-1` directory, and executing the command: `./start`. This may take several seconds to complete. Once you see `[Browsersync] Watching files...`, you're up and running. This will block your command terminal while running. In order to stop it, you can type `Ctrl + C` in the same command terminal.


Next, go to http://localhost:3000/ in your browser, and you should see a familiar file browser interface. Use this interface to navigate to the HTML file you'd like to run.

![image](https://user-images.githubusercontent.com/527082/77561423-ab802400-6e8c-11ea-95ba-c62962c56bd3.png)

## Rendering Two Side-by-Side Elements Using `<React.Fragment>`
When experimenting with writing a React component you may have come across this error message:

```
Uncaught SyntaxError: Inline Babel script: Adjacent JSX elements must be wrapped in an enclosing tag
```

This error occurs when your component returns two side-by-side elements with no parent wrapper element. This is because JSX syntax requires components to return a single element. However, this single, top-level element can contain as many child elements as you wish. Consider the following example:

```jsx
function Hello() {
  // Error
  return (
    <span>Hello</span>
    <span>World</span>
  );
}
```

This will not work because the `Hello` component is returning two adjacent elements at the top level, which are not wrapped by a parent element. You could fix this by wrapping them:

```jsx
function Hello() {
  return (
    <div>
      <span>Hello</span>
      <span>World</span>
    </div>
  );
wrapper }
```

However, for various reasons it is not always possible (or optimal) to have a parent element. In these cases, you can use the `React.Fragment` component as the wrapper.

```jsx
function Hello() {
  return (
    <React.Fragment>
      <span>Hello</span>
      <span>World</span>
    </React.Fragment>
  );
}
```

For an example of where this would be useful, consider a React component which is a list of items (`li` elements) within an HTML unordered list (`ul`) element. Since your component produces multple `li` elements you need to wrap them. However `div` elements are not allowed as direct child elements of a `ul` element, so you would need to use `React.Fragment` instead.

```jsx
<ul id="list"></ul>

...

<script type="text/babel">
  function ListItems() {
    return (
      <React.Fragment>
        <li>one</li>
        <li>two</li>
        <li>three</li>
      <React.Fragment>
    )
  }

  ReactDOM.render(
    <ListItems />,
    document.getElementById("list"),
  );
<script>
```

This will produce:

```html
<ul id="list">
  <li>one</li>
  <li>two</li>
  <li>three</li>
</ul>
```

Using a `div` as a wrapper instead would produce this, which is not valid HTML:

```html
<ul id="list">
  <div>
    <li>one</li>
    <li>two</li>
    <li>three</li>
  </div>
</ul>
```

**Note:** The `React.Fragment` component didn't exist when the original version of this course was created. Below are the links to the related lesson from the new versio nof this course, and the `React.Fragment` documentation.
- Lesson: https://egghead.io/lessons/react-render-two-elements-side-by-side-with-react-fragments
- Docs: https://reactjs.org/docs/fragments.html