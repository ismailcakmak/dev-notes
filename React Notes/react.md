React is both performant and declerative way of programming web apps.

You can use pure JS to create and mainpulate the DOM. but it is very imperative way of coding.
HTML is a bit more declerative
React is very declerative way

#### Ceating react project

REACT:

npx create-react-app my-todo-app
npm start

REACT NATIVE:



### Writing React

JSX
- XML-like syntax extension of JavaScript
- Transpiles to JavaScript



Lowercase tags are treated as HTML/SVG tags, uppercase are treated
as custom components. But what is a component?

Components are just functions:
- Returns a node (something React can render, e.g. a <div />)
- Receives an object of the properties that are passed to the element
- you can pass arguments to the custom components, you write it as inline key-value pair. you should put the values in {} since react will understand it is a js code will be passed.
- any changes in "props" will cause rerender of the returned node. 

Note : aynı şekilde componentlere props atama HTML de de varmış ama react ta farklı olarak passlediğimiz object herhangi bir js value olabilir, htmlde ise sadece string passlenebiliyor.


Here for example, see that:
- App2 is a component, since it returns a node (a piece of code) that react can render.
- App2 is decleared, first letter is in uppercase.
- it is being used in App component by uppercasing the first letter `<App2/>` since it is
- custom component, not a html tag.
- Also see that, App2 is being passed some properties in App component. It is in line, key-value pair. And it is passed as an object to App2 function. the object is passed to component is "props", then you use keys withing this "props" object in your function to access values.


```js
function App2 (props) {
  return (
    <div>
      <h2>{props.name}</h2>
    </div>
  )
}

function App() {
  return (
    <div>
        <App2 name={"ismail"}/>
        <a>
          Learn React
        </a>
    </div>
  );
}

export default function() {
  return <App/>
};

```
