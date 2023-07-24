# Templating libraries

## Embedded Javascript templates (ejs)

Link: [https://ejs.co/](https://ejs.co/)

Очень популярный шаблонизатор в NodeJS мире

### Как он работает с Express

```
$ tree
-> app.js
-> views
----> index.ejs

$ npm i express ejs
$ node app.js
App listening on port 3000
```

В views директорию закидываем наши шаблоны. Доступ к ним через вызов функции `res.render()`.

app.js:

```javascript
// Setup app
const express = require("express");
const app  = express();
const port = 3000;

// Select ejs templating library
app.set('view engine', 'ejs');

// Routes
app.get("/", (req, res) => {
    res.render("index");
})

// Start app
app.listen(port, () => {
    console.log(`App listening on port ${port}`)
})
```
