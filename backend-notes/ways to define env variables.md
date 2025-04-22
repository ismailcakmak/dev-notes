
# 1- passing from command :

kodda variable i import ederiz : 

```js
const url = process.env.MONGODB_URI
```

ardından variable i terminalden pass layabilriz : 
```js
MONGODB_URI="your_connection_string_here" npm run dev
```


# 2- using a dedicated file

for node.js , it requires dotenv lib:

```
npm install dotenv
```

then define .env file :
```js
MONGODB_URI=mongodb+srv://fullstack:thepasswordishere@cluster0.a5qfl.mongodb.net/noteApp?retryWrites=true&w=majority&appName=Cluster0
PORT=3001
``` 

> DONT FORGET TO ADD .env to .gitignore

after that import your file to your code like this : 
```js
require('dotenv').config()
```


now you can again access your env variables like this 
```js
const url = process.env.MONGODB_URI
```


make sure that you import your env file in the beginning of the file, so that other imported modules may require env variables as well.

For example note file inside models folder may require our env variable as well, therefore it should imported after env var : 
```js
	require('dotenv').config()
	const express = require('express')
	const Note = require('./models/note')
```

# When using Render.com
When using Render, the database url is given by defining the proper env in the dashboard.