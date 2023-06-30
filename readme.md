# Node.js

## Node.js Modules

### Node.js process module

The process module is a global object that provides information about, and control over, the current Node.js process. As a global, it is always available to Node.js applications without using require().

- Example: process.argv

```js
console.log(process.argv);
```

result:

```bash
$ node process.js one two=three four
[
  '/usr/local/bin/node',
  '/Users/mjr/work/node/process-args.js',
  'one',
  'two=three',
  'four'
]
```

### Node.js os module
