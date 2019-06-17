# Algorithm

## Problem

Do the necessary to make the test pass. This is intended to evaluate your algorithmic skills.

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

## Solution

After some headache, I came to this solution :

```js
const resolveObject = (obj) => {
    Object.keys(obj).forEach(key => {
        const splitted = key.split('.');

        if (splitted.length > 1) {
            let cursor = { ...obj };

            splitted.forEach((part, i) => {
                if (cursor.hasOwnProperty(part)) {
                    cursor = cursor[part];
                } else if (i === splitted.length - 1) {
                    cursor[part] = obj[key];
                    delete obj[key];
                }
            });
        }
    });
    return obj;
}

module.exports = resolveObject;
```

Pros:
- Simply modify our object, so it is a lot faster than to clone it
- Code is pretty simple to understand
- Pass the test (yes !)
- Will be a good start in a TDD way

Cons:
- Only works if keys already exists
- Only works for not nested keys `foo.bar` 
- In a TDD way, there is already some overengeneering
