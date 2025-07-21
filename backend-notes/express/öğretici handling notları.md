# case 1
```js
app.post('/notes', (request, response)=>{
    const body = request.body
    const note = body.note

    if (note===undefined){
        response.status(400).json({"error" : "note field should be exist"})
    }

    if (note==="" || note.trim() === ""){
        response.status(400).json({"error" : "note field should not be empty"})
    }

    noteRepository.createNote()
    ...
})
```


diyelim ki böyle bir post endpointiniz var ve clientdan gelen note objesini db ye yazmadan önce gerçekten gelip gelmediğini, boş gelmişse vs gibi kontrollerini yapmanız gerekiyor.


js'de undefined diye bir type olduğundan, body.note ifadesi body objesinde note field'i olmazsa undefined döndürecek fakat programı crashlatmayacak. note değişkenine "undefined" tipini assign edip çalışmaya devam edecek.

kodun ilerleyen kısımlarında falsy kontrolü (!note) yaparak bunu handle edebiliriz.

eğer note field'ı aslında varsa fakat "" ise bunu da falsy kontrolüyle handle edebiliriz

eğer note field'ı varsa fakat full whitespace karakterlerinden oluşuyorsa bunu da note.trim() yaparak yine "" haline getirdiğimiz için falsy kontorlüyle handle edebiliriz.

sonuc olarak şu şekilde bir kontrolü tüm bu caselerin handle edilmesi için yeterli olacak : 

```js
app.post('/notes', (request, response) => {
    const body = request.body
    const note = body.note

    if (!note || note.trim() === "") {
        return response.status(400).json({"error": "note field is required and cannot be empty"})
    }

    noteRepository.createNote()
    // ...
})
```

---

eğer istersek bunları ayrı ayrı şu şekilde de handle edebiliriz : 

```js
app.post('/notes', (request, response) => {
    const body = request.body
    const note = body.note

    if (note === undefined) {
        return response.status(400).json({"error": "note field is required"})
    }

    if (note === "" || note.trim() === "") {
        return response.status(400).json({"error": "note field cannot be empty"})
    }

    noteRepository.createNote()
    // ...
})
```