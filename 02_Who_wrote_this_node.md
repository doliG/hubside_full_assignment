# NodeJS && Express

## Problem

Review this code. What remarks would you make? How would you enhance it ?

```js
let express = require('express');
let bodyParser = require('body-parser');
let app = express(); app.use(bodyParser.json());

let todos = [{ id: 'jkhsdjkf', content: 'review this code' }];

app.post('/todos', (req, res) => {
    todos.push({
        ...req.body,
        id: Math.random().toString(32).slice(2)
    });
    res.sendStatus(201);
});
app.put('/todos/:id', (req, res) => {
    todos[Number(req.params.id)] = req.body;
    res.sendStatus(200);
});
app.get('/todos/:id', (req, res) => {
    res.send(todos[id]);
});
app.get('/todos/all', (req, res) => {
    res.send(todos);
});
app.get('/', (req, res) => {
    res.send('OK');
});
app.listen(8080, () => {
    console.log('Listening on port 8080...');
});
```

## Solution

First thing that comes to my mind: it lacks a way to delete a todo to have a full working CRUD.
We can do it simply by adding this snippet:

```js
app.delete('/todos/:id', (req, res) => {
    const res = todos.find(el => el.id === req.params.id);

    if (res) {
        todos = todos.filter(el => el.id !== req.params.id);
        res.sendStatus(204);
    } else {
        res.sendStatus(404);
    }
});
```


Then, I find that this api is not RESTful compliant (please note that I’ve only done backend stuff at school, so I rely on those notions).

In order to make it restful, I’ll change this (commented lines are old code):

```js
// app.get('/todos/all', (req, res) =>
app.get('/todos', (req, res) => {
    res.send(todos);
});
```

We can simply sendStatus here:

```js
app.get('/', (req, res) => {
    // res.send('OK');
    res.sendStatus(200);
});
```

Also, I think there is a problem in the **get** and **put** `/todos/:id`: the way we select the todo won’t work because `todos` is an array, so we can’t access elements directly by using the `id` of one `todo`. I’d rather do it this way:

```js
app.put('/todos/:id', (req, res) => {
    // todos[Number(req.params.id)] = req.body;
    const taskToUpdate = todos.find(t => t.id === req.params.id);

    if (taskToUpdate) {
        taskToUpdate = { ...taskToUpdate, ...req.body };
        todos = todos.map(t => t.id === req.params.id ? taskToUpdate : t);
        res.send(taskToUpdate);
    } else {
        res.sendStatus(404);
    }
});

app.get('/todos/:id', (req, res) => {
    // res.send(todos[id]);
    const task = todos.find(t => t.id === req.body.id);

    if (task) {
        res.send(task);
    } else {
        res.sendStatus(404);
    }
});
```

Finally, we can use yield to generate id’s , which in my opinion is more elegant:

```js
function* generator(id) {
    while(true) { yield id++; }
}

const idGenerator = generator(0);

app.post('/todos', (req, res) => {
    todos.push({
        ...req.body,
        // id: Math.random().toString(32).slice(2),
        id: idGenerator.next().value,
    });
    res.sendStatus(201);
});
```

Some other comments:
- We should check during creation and update the content of `req.body` and validate it
- We should use DB for persistency
