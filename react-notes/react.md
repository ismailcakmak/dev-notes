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


In React, the individual things rendered in braces must be primitive values, such as numbers or strings. Yani şöyle düşünebiliriz, curly braces içine yazdığım şeyler js engine ile runlandığında primitive valuelar returnlemesi gerekiyor. 

---

never mutate state directly in React.

---

react ta stateler updatelenirken, primitive value ise direk kendisini veebilirsiniz, çünkü zaten onların değerleri kopyalanıyor ve yeni bir değişken oluşturuluyor.

örneğin böyle bir state'i :
```jsx
const [counter, setCounter] = useState(0);
```

şu şekilde direk updateleuebiliriz : 
```js
setCounter(counter+1)
```


fakat diyelim ki state imiz bir obje (literal object, array vs) :

```js
const [coords, setCoords] = useState({x:0, y:0});
```
diyelim ki state değerleri aynı kalıyor olsa bile updatelememiz gerekli
burda ... kullanarak yeni bir obje ile updatelememiz gerekir :
```js
setState({...})
```
eğer bir property yi updateleyecek ise şöyle yaparız :

```js
setState({... y:1});
``` 

Rules of Hooks

read them from here : https://react.dev/warnings/invalid-hook-call-warning#breaking-rules-of-hooks


---

"event handler" ile "callback" aynı şeydir.

ve event handler her zaman fonksiyon almalıdır. 
Örneğin aşağıdaki örnekte, onClick ın aldığı event handler bir fonksiyon değil, console.log returnr değeri olarak undefined döndürür. bu yüzden de bu button a basmak hiçbir iş yapmaz.

```jsx
<button onClick={console.log("clicked)}>button</button>
```

Not : bu kod çalıştırıldığında console "clicked" yazdırırlır çünkü bu kod
ilk render edilirken invokelanır.


### Handling form inputs

There are 2 way of handling form inputs.

1- controlled component 

bunda formdaki her input'un value'su ve onChange fonskiyonu oluşturulur. value state olur bu sayede her yeni harf/sayı vs girişinde kullanıcı input kısmının içinde ne yazdığını görür.
form submit edildiğinde bu state değişkenleri kullanılır ne yapılacaksa yapılır.

```js
const App = (props) => {
  const [notes, setNotes] = useState(props.notes)
  const [showAll, setShowAll] = useState(true)
  const [newNote, setNewNote] = useState('') // Add this state

  const addNote = (event) => {
    event.preventDefault()

    const noteObject = {
      content: newNote, // Use the state value instead of FormData
      important: Math.random() > 0.5,
      id: notes.at(-1).id + 1
    }
      
    setNotes([...notes, response.data])
    setNewNote('') // Clear the input after successful submission
  }

  const handleNoteChange = (event) => {
    setNewNote(event.target.value)
  }

  // In your return statement, modify the form:
  <form onSubmit={addNote}>
    <input 
      type="text"
      value={newNote}
      onChange={handleNoteChange}
    />
    <button type="submit">save</button>
  </form>
```

2- uncontrolled component

bu da direk formun callback fonksiyonundan event e erişerek input ların verisine erişme yoludur.


```js

const App = (props) => {

  const addNote = (event) => {
    event.preventDefault()
    const formData = new FormData(event.target)
    const { note } = Object.fromEntries(formData)
      
    const noteObject = {
      content: note,
      important: Math.random() > 0.5,
      id: notes.at(-1).id + 1
    }

    setNotes([...notes, response.data])
    event.target.reset() // This will clear the form
  }

  return(
    // In your return statement, modify the form:
    <form onSubmit={addNote}>
      <input 
        type="text"
        value={newNote}
        onChange={handleNoteChange}
      />
      <button type="submit">save</button>
    </form>
  )
}

```

Best practice, controlled compoenent methodunu kullanmak,
çünkü : 
- it follows React's best practices for form handling
- it easier to implement features like validation



### ways of adding id into elements that has connection with db

kendimiz obje oluştururken id oluşturup db ye kaydetmek için öyle gönderebilriiz. bunu frontendde yapacaksak :
- global bir değişken tutarak
- Use a library like `uuid` 
- Use a timestamp-based approach

fakat örneğin global bir objeyi increment ettirme methodunda
program kapatılıp açıldığında en son hangi id de kaldığımızı kaybedebiliriz.

Recommended way ise bunu backend de halletmektir

1. Let the backend handle ID generation (recommended approach)

```jsx
const addNote = (event) => {
  event.preventDefault();    
  const formData = new FormData(event.target);
  const { note } = Object.fromEntries(formData); 
      
  const noteObject = {
    content: note,
    important: Math.random() > 0.5
    // Remove id field - let the backend generate it
  }

  axios
    .post('http://localhost:3001/notes', noteObject)
    .then((response) => {
      console.log(response);
      setNotes([...notes, response.data]);
      event.target.reset();
    })
}
```

The backend should generate and return the note with a unique ID in its response, which will then be added to your notes state.