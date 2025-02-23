
## Anti-pattern: Array Indexes as Keys

react da bir liste vs oluşturacağımız zaman listenin her bir elementine key vermemiz gerekir.
eğer vermezsek react agabey kendisi en azından array ın indexlerini verir.
fakat bu robust bir yöntem değil, çünkü react virtual dom ile normal domu karşılaştırıken bu keylere bakarak karşılaştırır.
yani mesela listedeki 4.indexteki elemanı çıkarıp sonra bir şey ekledin diyelim, hala 4.indexte bir eleman olduğu için ve toplamda 
listedeki eleman sayısı değişmediği için bu bir update olaark algılanmaz ve dom updatelenmez.

bunun için en iyi yol, id leri kendimiz vermek. bunu az sayıda eleman varsa her elemana idyi elle vererek yapabiliriz.
veya glboal bir değişken tuutp onu incerment ederrek yaabiliriz.
veya shortid import edip onu kullanabilriiz, aynı mantık sadece global pollution yapmamış oluruz.

Yapılmaması gereken : 
```js
{
  todos.map((todo, index) => <Todo {...todo} key={index} />);
}

```

Bu iyi :
```js
todoCounter = 1;

function createNewTodo(text) {
  return {
    completed: false,
    id: todoCounter++,
    text,
  };
}

```

Bu daha iyi :

```js
var shortid = require("shortid");

function createNewTodo(text) {
  return {
    completed: false,
    id: shortid.generate(),
    text,
  };
}

```


Sources :
```
https://robinpokorny.com/blog/index-as-a-key-is-an-anti-pattern/
https://fullstackopen.com/en/part2/rendering_a_collection_modules#anti-pattern-array-indexes-as-keys
```

## it is okay to keys on the sub custom items

örneğin Note componentimiz var diyelim ve bir de notes arrayimiz var,
bu notes arrayimizdeki notları Notes a passleyip, render edeceğiz diyelim.

Burda note lar bir <ul> altında olacağı için notelarrın hepsinin bir id si olmalı, fakat bu id ler
illa <il> de olmak zournda değil bu notta anlatmak istediğimiz bu. ul in altnda hangi component varsa ona passlenmesi yeterli.

örneğin şuna bakabilirsiniz : 

```js
const Note = ({ note }) => {
  return (
    <li>{note.content}</li>
  ) 
}

const App = ({ notes }) => {
  return (
    <div>
      <h1>Notes</h1>
      <ul>

        {notes.map(note => 
          <Note key={note.id} note={note} />
        )}
      </ul>
    </div>
  )
}
```



## react styling


Örneğin p yi şu şekillerde boldlayabiliriz :


```js
const Content = ({parts})=>{
  return(
    <div>
        {parts.map((part) => <Part part={part} key={part.id}/>)}
        <p style={{ fontWeight: 'bold' }}>
          total of {parts.reduce((acc,item)=>acc+item.exercises, 0)} exercises
        </p>
    </div>
  )
}
```

```js
// In your CSS file
.bold-text {
  font-weight: bold;
}

// In your Component
const Content = ({parts})=>{
  return(
    <div>
        {parts.map((part) => <Part part={part} key={part.id}/>)}
        <p className="bold-text">
          total of {parts.reduce((acc,item)=>acc+item.exercises, 0)} exercises
        </p>
    </div>
  )
}
```

## dont use + for string concatenation

using the + operator for string concatenation in JavaScript/React is generally considered less ideal since it can lead to unexpected type coercion.

use template literals instead



## use console.log("props", prop)

using this :
```js
console.log("props" + props)
```
gives not explanatory descrption.


therefore use this :
```js
console.log("props", props)
```
