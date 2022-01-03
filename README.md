# tango node eslint config

Tango ESLint rules

## Installation

You'll first need to install ESLint:

```bash
npm install eslint --save-dev
```

Next, install eslint-plugin-json-files:

```bash
npm install eslint-config-tango-node --save-dev
```

*NOTE:* If you installed ESLint globally (using the -g flag) then you must also install eslint-plugin-json-files globally.

## Usage

You'll see several dependencies were installed. Now, create (or update) a `.eslintrc` file with the following content:

```json
{
  'extends': [
    'tango-node'
  ]
}
```

## Possible Problems

### for-direction

Examples of incorrect code for this rule:

```javascript
for (var i = 0; i < 10; i--) { }

for (var i = 10; i >= 0; i++) { }

for (var i = 0; i > 10; i++) { }
```

Examples of correct code for this rule:

```javascript
for (var i = 0; i < 10; i++) {}
```
### getter-return

Examples of incorrect code for this rule:

```javascript
p = {
  get name(){
    // no returns.
  }
};

Object.defineProperty(p, 'age', {
  get: function (){
    // no returns.
  }
});

class P{
  get name(){
    // no returns.
  }
}
```

Examples of correct code for this rule:

```javascript
p = {
  get name() {
    return 'nicholas';
  },
};

Object.defineProperty(p, 'age', {
  get: function () {
    return 18;
  },
});

class P {
  get name() {
    return 'nicholas';
  }
}
```

### no-await-in-loop

Examples of incorrect code for this rule:

```javascript
async function foo(things) {
  const results = [];
  for (const thing of things) {
    // Bad: each loop iteration is delayed until the entire asynchronous operation completes
    results.push(await bar(thing));
  }
  return baz(results);
}
```

Examples of correct code for this rule:

```javascript
async function foo(things) {
  const results = [];
  for (const thing of things) {
    // Good: all asynchronous operations are immediately started.
    results.push(bar(thing));
  }
  // Now that all the asynchronous operations are running, here we wait until they all complete.
  return baz(await Promise.all(results));
}
```

### no-compare-neg-zero

Examples of incorrect code for this rule:

```javascript
if (x === -0) {
  // doSomething()...
}
```

Examples of correct code for this rule:

```javascript
if (x === 0) {
  // doSomething()...
}
/* eslint no-compare-neg-zero: 'error' */

if (Object.is(x, -0)) {
  // doSomething()...
}
```

### no-cond-assign

Examples of incorrect code for this rule:

```javascript
// Unintentional assignment
var x;
if ((x = 0)) {
  var b = 1;
}

// Practical example that is similar to an error
function setHeight(someNode) {
  'use strict';
  do {
    someNode.height = '100px';
  } while ((someNode = someNode.parentNode));
}
```

Examples of correct code for this rule:

```javascript
// Assignment replaced by comparison
var x;
if (x === 0) {
  var b = 1;
}

// Practical example that wraps the assignment in parentheses
function setHeight(someNode) {
  'use strict';
  do {
    someNode.height = '100px';
  } while ((someNode = someNode.parentNode));
}

// Practical example that wraps the assignment and tests for 'null'
function setHeight(someNode) {
  'use strict';
  do {
    someNode.height = '100px';
  } while ((someNode = someNode.parentNode) !== null);
}
```

### no-console

Examples of incorrect code for this rule:

```javascript
console.log('Log a debug level message.');
console.log = foo();
```

Examples of correct code for this rule:

```javascript
console.warn('Log a warn level message.');
console.error('Log an error level message.');
```

### no-constant-condition

Examples of incorrect code for this rule:

```javascript
if (false) {
  doSomethingUnfinished();
}

if (void x) {
  doSomethingUnfinished();
}

if ((x &&= false)) {
  doSomethingNever();
}

if (class {}) {
  doSomethingAlways();
}

if (new Boolean(x)) {
  doSomethingAlways();
}

if ((x ||= true)) {
  doSomethingAlways();
}

for (; -2; ) {
  doSomethingForever();
}

while (typeof x) {
  doSomethingForever();
}

do {
  doSomethingForever();
} while ((x = -1));

var result = 0 ? a : b;
```

Examples of correct code for this rule:

```javascript
if (x === 0) {
    doSomething();
}

for (;;) {
    doSomethingForever();
}

while (typeof x === 'undefined') {
    doSomething();
}

do {
    doSomething();
} while (x);

var result = x !== 0 ? a : b;
```

### no-debugger

Examples of incorrect code for this rule:

```javascript
function isTruthy(x) {
  debugger;
  return Boolean(x);
}
```

Examples of correct code for this rule:

```javascript
function isTruthy(x) {
  return Boolean(x); // set a breakpoint at this line
}
```

### no-dupe-args

Examples of incorrect code for this rule:

```javascript
function foo(a, b, a) {
  console.log('value of the second a:', a);
}

var bar = function (a, b, a) {
  console.log('value of the second a:', a);
};
```

Examples of correct code for this rule:

```javascript
function foo(a, b, c) {
  console.log(a, b, c);
}

var bar = function (a, b, c) {
  console.log(a, b, c);
};

```

### no-dupe-else-if

Examples of incorrect code for this rule:

```javascript
if (isSomething(x)) {
  foo();
} else if (isSomething(x)) {
  bar();
}

if (a) {
  foo();
} else if (b) {
  bar();
} else if (c && d) {
  baz();
} else if (c && d) {
  quux();
} else {
  quuux();
}

if (n === 1) {
  foo();
} else if (n === 2) {
  bar();
} else if (n === 3) {
  baz();
} else if (n === 2) {
  quux();
} else if (n === 5) {
  quuux();
}
```

Examples of correct code for this rule:

```javascript
if (isSomething(x)) {
    foo();
} else if (isSomethingElse(x)) {
    bar();
}

if (a) {
    foo();
} else if (b) {
    bar();
} else if (c && d) {
    baz();
} else if (c && e) {
    quux();
} else {
    quuux();
}

if (n === 1) {
    foo();
} else if (n === 2) {
    bar();
} else if (n === 3) {
    baz();
} else if (n === 4) {
    quux();
} else if (n === 5) {
    quuux();
}
```

### no-dupe-keys

Examples of incorrect code for this rule:

```javascript
var foo = {
  bar: 'baz',
  bar: 'qux',
};

var foo = {
  bar: 'baz',
  bar: 'qux',
};

var foo = {
  0x1: 'baz',
  1: 'qux',
};
```

Examples of correct code for this rule:

```javascript
var foo = {
  bar: 'baz',
  quxx: 'qux',
};
```

### no-duplicate-case

Examples of incorrect code for this rule:

```javascript
switch (a) {
  case '1':
    break;
  case '2':
    break;
  case '1': // duplicate test expression
    break;
  default:
    break;
}
```

Examples of correct code for this rule:

```javascript
switch (a) {
  case '1':
    break;
  case '2':
    break;
  case '3':
    break;
  default:
    break;
}
```

### no-empty

Examples of incorrect code for this rule:

```javascript
if (foo) {}

while (foo) {}

switch (foo) {}

try {
  doSomething();
} catch (ex) {
} finally {}
```

Examples of correct code for this rule:

```javascript
if (foo) {
  // empty
}

while (foo) {
  /* empty */
}

try {
  doSomething();
} catch (ex) {
  // continue regardless of error
}

try {
  doSomething();
} finally {
  /* continue regardless of error */
}
```

### no-ex-assign

Examples of incorrect code for this rule:

```javascript
try {
  // code
} catch (e) {
  e = 10;
}
```

Examples of correct code for this rule:

```javascript
try {
  // code
} catch (e) {
  var foo = 10;
}
```

### no-extra-boolean-cast

Examples of incorrect code for this rule:

```javascript
var foo = !!!bar;

var foo = !!bar ? baz : bat;

var foo = Boolean(!!bar);

var foo = new Boolean(!!bar);

if (!!foo) {
  // ...
}

if (Boolean(foo)) {
  // ...
}

while (!!foo) {
  // ...
}

do {
  // ...
} while (Boolean(foo));

for (; !!foo; ) {
  // ...
}
```

Examples of correct code for this rule:

```javascript
var foo = !!bar;
var foo = Boolean(bar);

function foo() {
    return !!bar;
}

var foo = bar ? !!baz : !!bat;
```

### no-extra-parens

Examples of incorrect code for this rule:

```javascript
a = (b * c);

(a * b) + c;

for (a in (b, c));

for (a in (b));

for (a of (b));

typeof (a);

(function(){} ? a() : b());

class A {
  [(x)] = 1;
}

class B {
  x = (y + z);
}
```

Examples of correct code for this rule:

```javascript
(0).toString();

Object.prototype.toString.call();

({}.toString.call());

(function () {} ? a() : b());

/^a$/.test(x);

for (a of (b, c));

for (a of b);

for (a in (b, c));

for (a in b);

class A {
  [x] = 1;
}

class B {
  x = y + z;
}
```

### no-extra-semi

Examples of incorrect code for this rule:

```javascript
var x = 5;;

function foo() {
  // code
};

class C {
  field;;

  method() {
    // code
  };

  static {
      // code
  };
};
```

Examples of correct code for this rule:

```javascript
var x = 5;

function foo() {
  // code
}

class C {
  field;

  method() {
    // code
  }

  static {
    // code
  }
}
```

### no-func-assign

Examples of incorrect code for this rule:

```javascript
function foo() {}
foo = bar;

function foo() {
  foo = bar;
}

var a = function hello() {
  hello = 123;
};
```

Examples of correct code for this rule:

```javascript
var foo = function () {};
foo = bar;

function foo(foo) {
  foo = bar; // `foo` is shadowed.
}

function foo() {
  var foo = bar; // `foo` is shadowed.
}
```

### no-import-assign

Examples of incorrect code for this rule:

```javascript
import mod, { named } from './mod.mjs';
import * as mod_ns from './mod.mjs';

mod = 1; // ERROR: 'mod' is readonly.
named = 2; // ERROR: 'named' is readonly.
mod_ns.named = 3; // ERROR: The members of 'mod_ns' are readonly.
mod_ns = {}; // ERROR: 'mod_ns' is readonly.
// Can't extend 'mod_ns'
Object.assign(mod_ns, { foo: 'foo' }); // ERROR: The members of 'mod_ns' are readonly.

```

Examples of correct code for this rule:

```javascript
import mod, { named } from './mod.mjs';
import * as mod_ns from './mod.mjs';

mod.prop = 1;
named.prop = 2;
mod_ns.named.prop = 3;

// Known Limitation
function test(obj) {
  obj.named = 4; // Not errored because 'obj' is not namespace objects.
}
test(mod_ns); // Not errored because it doesn't know that 'test' updates the member of the argument.

```

### no-inner-declarations

Examples of incorrect code for this rule:

```javascript
if (test) {
  function doSomething() {}
}

function doSomethingElse() {
  if (test) {
    function doAnotherThing() {}
  }
}

if (foo) function f() {}

class C {
  static {
    if (test) {
      function doSomething() {}
    }
  }
}
```

Examples of correct code for this rule:

```javascript
function doSomething() {}

function doSomethingElse() {
  function doAnotherThing() {}
}

class C {
  static {
    function doSomething() {}
  }
}

if (test) {
  asyncCall(id, function (err, data) {});
}

var fn;
if (test) {
  fn = function fnExpression() {};
}

if (foo) var a;
```

### no-irregular-whitespace

Examples of incorrect code for this rule:

```javascript
function thing() /*<NBSP>*/{
    return 'test';
}

function thing( /*<NBSP>*/){
    return 'test';
}

function thing /*<NBSP>*/(){
    return 'test';
}

function thing᠎/*<MVS>*/(){
    return 'test';
}

function thing() {
    return 'test'; /*<ENSP>*/
}

function thing() {
    return 'test'; /*<NBSP>*/
}

function thing() {
    // Description <NBSP>: some descriptive text
}

/*
Description <NBSP>: some descriptive text
*/

function thing() {
    return / <NBSP>regexp/;
}

/*eslint-env es6*/
function thing() {
    return `template <NBSP>string`;
}
```

Examples of correct code for this rule:

```javascript
function thing() {
  return ' <NBSP>thing';
}

function thing() {
  return '​<ZWSP>thing';
}

function thing() {
  return 'th <NBSP>ing';
}
```

### no-loss-of-precision

Examples of incorrect code for this rule:

```javascript
const x = 9007199254740993
const x = 5123000000000000000000000000001
const x = 1230000000000000000000000.0
const x = .1230000000000000000000000
const x = 0X20000000000001
const x = 0X2_000000000_0001;
```

Examples of correct code for this rule:

```javascript
const x = 12345
const x = 123.456
const x = 123e34
const x = 12300000000000000000000000
const x = 0x1FFFFFFFFFFFFF
const x = 9007199254740991
const x = 9007_1992547409_91
```

### no-misleading-character-class

Examples of incorrect code for this rule:

```javascript
/^[Á]$/u
/^[❇️]$/u
/^[👶🏻]$/u
/^[🇯🇵]$/u
/^[👨‍👩‍👦]$/u
/^[👍]$/
```

Examples of correct code for this rule:

```javascript
/^[abc]$/
/^[👍]$/u
```

### no-obj-calls

Examples of incorrect code for this rule:

```javascript
var math = Math();

var newMath = new Math();

var json = JSON();

var newJSON = new JSON();

var reflect = Reflect();

var newReflect = new Reflect();

var atomics = Atomics();

var newAtomics = new Atomics();
```

Examples of correct code for this rule:

```javascript
function area(r) {
    return Math.PI * r * r;
}

var object = JSON.parse('{}');

var value = Reflect.get({ x: 1, y: 2 }, 'x');

var first = Atomics.load(foo, 0);
```

### no-promise-executor-return

Examples of incorrect code for this rule:

```javascript
new Promise((resolve, reject) => {
  if (someCondition) {
    return defaultResult;
  }
  getSomething((err, result) => {
    if (err) {
      reject(err);
    } else {
      resolve(result);
    }
  });
});

new Promise((resolve, reject) =>
  getSomething((err, data) => {
    if (err) {
      reject(err);
    } else {
      resolve(data);
    }
  })
);

new Promise(() => {
  return 1;
});
```

Examples of correct code for this rule:

```javascript
new Promise((resolve, reject) => {
  if (someCondition) {
    resolve(defaultResult);
    return;
  }
  getSomething((err, result) => {
    if (err) {
      reject(err);
    } else {
      resolve(result);
    }
  });
});

new Promise((resolve, reject) => {
  getSomething((err, data) => {
    if (err) {
      reject(err);
    } else {
      resolve(data);
    }
  });
});

Promise.resolve(1);
```

### no-prototype-builtins

Examples of incorrect code for this rule:

```javascript
var hasBarProperty = foo.hasOwnProperty('bar');

var isPrototypeOfBar = foo.isPrototypeOf(bar);

var barIsEnumerable = foo.propertyIsEnumerable('bar');
```

Examples of correct code for this rule:

```javascript
var hasBarProperty = Object.prototype.hasOwnProperty.call(foo, 'bar');

var isPrototypeOfBar = Object.prototype.isPrototypeOf.call(foo, bar);

var barIsEnumerable = {}.propertyIsEnumerable.call(foo, 'bar');
```

### no-regex-spaces

Examples of incorrect code for this rule:

```javascript
var re = /foo   bar/;
var re = new RegExp('foo   bar');
```

Examples of correct code for this rule:

```javascript
var re = /foo {3}bar/;
var re = new RegExp('foo {3}bar');
```

### no-setter-return

Examples of incorrect code for this rule:

```javascript
var foo = {
  set a(value) {
    this.val = value;
    return value;
  },
};

class Foo {
  set a(value) {
    this.val = value * 2;
    return this.val;
  }
}

const Bar = class {
  static set a(value) {
    if (value < 0) {
      this.val = 0;
      return 0;
    }
    this.val = value;
  }
};

Object.defineProperty(foo, 'bar', {
  set(value) {
    if (value < 0) {
      return false;
    }
    this.val = value;
  },
});
```

Examples of correct code for this rule:

```javascript
var foo = {
  set a(value) {
    this.val = value;
  },
};

class Foo {
  set a(value) {
    this.val = value * 2;
  }
}

const Bar = class {
  static set a(value) {
    if (value < 0) {
      this.val = 0;
      return;
    }
    this.val = value;
  }
};

Object.defineProperty(foo, 'bar', {
  set(value) {
    if (value < 0) {
      throw new Error('Negative value.');
    }
    this.val = value;
  },
});
```

### no-unexpected-multiline

Examples of incorrect code for this rule:

```javascript
var foo = bar
(1 || 2).baz();

var hello = 'world'
[1, 2, 3].forEach(addNumber);

let x = function() {}
`hello`

let x = function() {}
x
`hello`

let x = foo
/regex/g.test(bar)
```

Examples of correct code for this rule:

```javascript
var foo = bar;
(1 || 2).baz();

var foo = bar
;(1 || 2).baz()

var hello = 'world';
[1, 2, 3].forEach(addNumber);

var hello = 'world'
void [1, 2, 3].forEach(addNumber);

let x = function() {};
`hello`

let tag = function() {}
tag `hello`
```

### no-unreachable

Examples of incorrect code for this rule:

```javascript
function foo() {
  return true;
  console.log("done");
}

function bar() {
  throw new Error("Oops!");
  console.log("done");
}

while (value) {
  break;
  console.log("done");
}

throw new Error("Oops!");
console.log("done");

function baz() {
  if (Math.random() < 0.5) {
    return;
  } else {
    throw new Error();
  }
  console.log("done");
}

for (;;) {}
console.log("done");
```

Examples of correct code for this rule:

```javascript
function foo() {
  return bar();
  function bar() {
    return 1;
  }
}

function bar() {
  return x;
  var x;
}

switch (foo) {
  case 1:
    break;
    var x;
}
```

### no-unreachable-loop

Examples of incorrect code for this rule:

```javascript
while (foo) {
  doSomething(foo);
  foo = foo.parent;
  break;
}

function verifyList(head) {
  let item = head;
  do {
    if (verify(item)) {
      return true;
    } else {
      return false;
    }
  } while (item);
}

function findSomething(arr) {
  for (var i = 0; i < arr.length; i++) {
    if (isSomething(arr[i])) {
      return arr[i];
    } else {
      throw new Error("Doesn't exist.");
    }
  }
}

for (key in obj) {
  if (key.startsWith("_")) {
    break;
  }
  firstKey = key;
  firstValue = obj[key];
  break;
}

for (foo of bar) {
  if (foo.id === id) {
    doSomething(foo);
  }
  break;
}
```

Examples of correct code for this rule:

```javascript
while (foo) {
  doSomething(foo);
  foo = foo.parent;
}

function verifyList(head) {
  let item = head;
  do {
    if (verify(item)) {
      item = item.next;
    } else {
      return false;
    }
  } while (item);

  return true;
}

function findSomething(arr) {
  for (var i = 0; i < arr.length; i++) {
    if (isSomething(arr[i])) {
      return arr[i];
    }
  }
  throw new Error("Doesn't exist.");
}

for (key in obj) {
  if (key.startsWith("_")) {
    continue;
  }
  firstKey = key;
  firstValue = obj[key];
  break;
}

for (foo of bar) {
  if (foo.id === id) {
    doSomething(foo);
    break;
  }
}
```

### no-unsafe-finally

Examples of incorrect code for this rule:

```javascript
let foo = function () {
  try {
    return 1;
  } catch (err) {
    return 2;
  } finally {
    return 3;
  }
};
```

Examples of correct code for this rule:

```javascript
let foo = function () {
  try {
    return 1;
  } catch (err) {
    return 2;
  } finally {
    console.log("hola!");
  }
};
```

### no-unsafe-negation

Examples of incorrect code for this rule:

```javascript
if (!key in object) {
  // operator precedence makes it equivalent to (!key) in object
  // and type conversion makes it equivalent to (key ? "false" : "true") in object
}

if (!obj instanceof Ctor) {
  // operator precedence makes it equivalent to (!obj) instanceof Ctor
  // and it equivalent to always false since boolean values are not objects.
}
```

Examples of correct code for this rule:

```javascript
if (!(key in object)) {
  // key is not in object
}

if (!(obj instanceof Ctor)) {
  // obj is not an instance of Ctor
}
```

### no-unsafe-optional-chaining

Examples of incorrect code for this rule:

```javascript
(obj?.foo)();

(obj?.foo).bar;

(foo?.()).bar;

(foo?.()).bar();

(obj?.foo ?? obj?.bar)();

(foo || obj?.foo)();

(obj?.foo && foo)();

(foo ? obj?.foo : bar)();

(foo, obj?.bar).baz;

obj?.foo`template`;

new (obj?.foo)();

[...obj?.foo];

bar(...obj?.foo);

1 in obj?.foo;

bar instanceof obj?.foo;

for (bar of obj?.foo);

const { bar } = obj?.foo;

[{ bar } = obj?.foo] = [];

with (obj?.foo);

class A extends obj?.foo {}

var a = class A extends obj?.foo {};

async function foo() {
  const { bar } = await obj?.foo;
  (await obj?.foo)();
  (await obj?.foo).bar;
}
```

Examples of correct code for this rule:

```javascript
obj?.foo?.();

obj?.foo();

(obj?.foo ?? bar)();

obj?.foo.bar;

obj.foo?.bar;

foo?.()?.bar;

(obj?.foo ?? bar)`template`;

new (obj?.foo ?? bar)();

var baz = { ...obj?.foo };

const { bar } = obj?.foo || baz;

async function foo() {
  const { bar } = (await obj?.foo) || baz;
  (await obj?.foo)?.();
  (await obj?.foo)?.bar;
}
```

### require-atomic-updates

Examples of incorrect code for this rule:

```javascript
let result;

async function foo() {
  result += await something;
}

async function bar() {
  result = result + (await something);
}

async function baz() {
  result = result + doSomething(await somethingElse);
}

async function qux() {
  if (!result) {
    result = await initialize();
  }
}

function* generator() {
  result += yield;
}
```

Examples of correct code for this rule:

```javascript
let result;

async function foobar() {
  result = (await something) + result;
}

async function baz() {
  const tmp = doSomething(await somethingElse);
  result += tmp;
}

async function qux() {
  if (!result) {
    const tmp = await initialize();
    if (!result) {
      result = tmp;
    }
  }
}

async function quux() {
  let localVariable = 0;
  localVariable += await something;
}

function* generator() {
  result = (yield) + result;
}
```

### use-isnan

Examples of incorrect code for this rule:

```javascript
if (foo == NaN) {
  // ...
}

if (foo != NaN) {
  // ...
}

if (foo == Number.NaN) {
  // ...
}

if (foo != Number.NaN) {
  // ...
}
```

Examples of correct code for this rule:

```javascript
if (isNaN(foo)) {
  // ...
}

if (!isNaN(foo)) {
  // ...
}
```

### valid-typeof

Examples of incorrect code for this rule:

```javascript
typeof foo === "strnig"
typeof foo == "undefimed"
typeof bar != "nunber"
typeof bar !== "fucntion"
```

Examples of correct code for this rule:

```javascript
typeof foo === "string"
typeof bar == "undefined"
typeof foo === baz
typeof bar === typeof qux
```
