# Introduction

When reading back this assignement done few weeks ago, I spot some small mistakes and I have felt like I need to fix that.

I take this opportunity to create a clean repository which will cover all this test and will be organized into multiples files.

You will find below the list of originals instructions.

Each solution will be placed into a separate file, and the instructions will be echoed at the beginning of each solution file.

## Instructions

Notes :
- All programs should be written in JavaScript,
- If not specified, framework/technology can be chosen freely,
- The usage of node js is mandatory if backend code is necessary,
- Using Reactjs is a big bonus,
- Think professional code. 

## [1. Do the necessary to make the test pass. This is intended to evaluate your algorithmic skills.](./01_Road_to_headache.md)

```js
const resolveObjects = require("../resolveObjects.js");

it("works", () => {
  const tests = [
    {
      input: {
        a: {
          b: {
            c: 'z',
          },
        },
        'a.b.d': 'y',
      },
      output: {
        a: {
          b: {
            c: 'z',
            d: 'y',
          },
        },
      },
    }
  ];
  tests.forEach(test => {
    expect(resolveObjects(test.input)).toEqual(test.output);
  });
});
```

[> Read the solution](./01_Road_to_headache.md#solution)

## [2. Review this code. What remarks would you make? How would you enhance it ?](./02_Who_wrote_this_node.md)

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

[> Read the solution](./02_Who_wrote_this_node.md#solution)

## [3. Bonus exercise (Not mandatory)](./03_Amazing_po_strikes_back.md)

You are expected to build a JavaScript application that lets users write hierarchical lists. 
We need to get an MVP out very soon, which will be incrementally augmented under tight deadlines. 

The main goal is that the application be easy to use and intuitive. 

The initial shipment should allow writing a list element, indenting or de-indenting it using the mouse, and to nest other list items below it. 

Ultimately, the application should:
- be able to export its content in the OPML format (http://dev.opml.org/)
- allow collaborative edition
- be entirely useable with the keyboard
- support Markdown (https://daringfireball.net/projects/markdown/syntax) for formatting -
- handle master-lists (lists of links to lists) 

And do much more; the product owner is bustling with ideas. 

[> Read the solution](./03_Amazing_po_strikes_back.md#solution)