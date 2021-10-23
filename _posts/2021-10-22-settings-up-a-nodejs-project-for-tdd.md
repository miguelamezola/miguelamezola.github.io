---
layout: post
title:  "Setting up a Node.js project for TDD"
summary: "How to use Mocha.js and chai.js in a Node.js project for Test-Driven Development (TDD)"
date:   2021-10-22 23:24:00 -0700
tags: unit testing, test-driven development, JavaScript, Node.js
published: true
---

According to [Wikipedia](https://en.wikipedia.org/wiki/Test-driven_development){:target="_blank"}, test-driven development (TDD) is a software development process that converts software requirements into test cases before the software is actually developed.  The steps of the TDD cycle are as follows:

1. Add a test (the new test should fail)
2. Write the simplest code that makes the new test pass ([YAGNI](https://www.martinfowler.com/bliki/Yagni.html){:target="_blank"})
3. Refactor the code

So, in order to set up a Node.js project for TDD, we need a way to run unit tests on the code that will be developed for it.  There are two popular tools for doing this: [Mocha](https://mochajs.org){:target="_blank"} is a popular JavaScript test framework and [Chai](https://www.chaijs.com){:target="_blank"} is an assertion library that can be nicely paired with it.

We can get started by launching a bash shell and running the following commands to initialize such a project.

```bash
mkdir tdd-project
cd tdd-project
npm init -y
npm install --save-dev mocha
npm install --save-dev chai
```

The `tdd-project` directory should now have a new file called `package.json` which contains the following:

```json
{
  "name": "tdd-project",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "keywords": [],
  "author": "",
  "license": "ISC",
  "devDependencies": {
    "chai": "^4.3.4",
    "mocha": "^9.1.3"
  }
}
```
Now all we have to do is modify the `scripts` property in this file.  We could simply make the `test` script run the `mocha` command.  However, `mocha` by default only finds test files with extensions `.js`, `.mjs`, and `.cjs` in a directory named `test`.  On the other hand, the following script finds all test files ending with `.test.js` in all directories except `node_modules`.

```json
{
  "name": "tdd-project",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "test": "mocha \"./{,!(node_modules)/**/}*.test.js\""
  },
  "keywords": [],
  "author": "",
  "license": "ISC",
  "devDependencies": {
    "chai": "^4.3.4",
    "mocha": "^9.1.3"
  }
}
```

After adding the `test` script, we can then run all tests by calling `npm test` in the project directory.  To get started with writing test files, we create a new file somewhere within the project (not in the `node_modules` directory of course) named `sample.test.js`.  Then we add the following contents:

```javascript
const expect = require('chai').expect;

describe('sample', () => {
	it('fails this test', () => {
		expect(true).to.equal(false);
	});
});
```

Running `npm test` should now return the following:

```bash

> tdd-project@1.0.0 test
> mocha "./{,!(node_modules)/**/}*.test.js"



  sample
    1) fails this test


  0 passing (2ms)
  1 failing

  1) sample
       fails this test:

      AssertionError: expected true to equal false
      + expected - actual

      -true
      +false

      at Context.<anonymous> (sample.test.js:5:19)
      at processImmediate (node:internal/timers:464:21)




```
Congratulations on writing your first failing test!  I have placed all the code in this post in a Github repository for your convenience: [https://github.com/miguelamezola/tdd-project.git](https://github.com/miguelamezola/tdd-project.git){:target="_blank"}
