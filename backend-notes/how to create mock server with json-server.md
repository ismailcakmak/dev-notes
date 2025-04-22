
first install json-server :
```
npm install json-server
```

Then create a db.json file in your root dir :

```js
{
  "notes": [
    {
      "id": "1",
      "content": "HTML is easy",
      "important": true
    },
    {
      "id": "2",
      "content": "Browser can execute only JavaScript",
      "important": false
    },
    {
      "id": "3",
      "content": "GET and POST are the most important methods of HTTP protocol",
      "important": true
    }
  ]
}
```


Then run it : 
`npx json-server --port 3001 db.json`

Then you can request to `localhost:3001/notes` to get the notes object

