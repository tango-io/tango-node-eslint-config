# tango node eslint config

Tango ESLint rules

## Installation

You'll first need to install ESLint:

```bash
npm install eslint --save-dev
```

Next, install eslint-plugin-json-files:

```bash
npm i @tango-io/eslint-config-tango-node --save-dev
```

_NOTE:_ If you installed ESLint globally (using the -g flag) then you must also install eslint-plugin-json-files globally.

## Usage

You'll see several dependencies were installed. Now, create (or update) a `.eslintrc` file with the following content:

```json
{
  "extends": [
    "@tango-io/tango-node"
  ]
}
```

## Possible Problems Rules

### for-direction

Examples of incorrect code for this rule:

```javascript
for (var i = 0; i < 10; i--) {}

for (var i = 10; i >= 0; i++) {}

for (var i = 0; i > 10; i++) {}
```

Examples of correct code for this rule:

```javascript
for (var i = 0; i < 10; i++) {}
```

### getter-return

Examples of incorrect code for this rule:

```javascript
p = {
  get name() {
    // no returns.
  },
};

Object.defineProperty(p, 'age', {
  get: function () {
    // no returns.
  },
});

class P {
  get name() {
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
if (foo) {
}

while (foo) {}

switch (foo) {
}

try {
  doSomething();
} catch (ex) {
} finally {
}
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
a = b * c;

a * b + c;

for (a in (b, c));

for (a in b);

for (a of b);

typeof a;

(function () {} ? a() : b());

class A {
  [x] = 1;
}

class B {
  x = y + z;
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
const x = 9007199254740993;
const x = 5123000000000000000000000000001;
const x = 1230000000000000000000000.0;
const x = 0.123;
const x = 0x20000000000001;
const x = 0x2_000000000_0001;
```

Examples of correct code for this rule:

```javascript
const x = 12345;
const x = 123.456;
const x = 123e34;
const x = 12300000000000000000000000;
const x = 0x1fffffffffffff;
const x = 9007199254740991;
const x = 9007_1992547409_91;
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
var foo = bar(1 || 2).baz();

var hello = 'world'[(1, 2, 3)].forEach(addNumber);

let x = (function () {})`hello`;

let x = function () {};
x`hello`;

let x = foo / regex / g.test(bar);
```

Examples of correct code for this rule:

```javascript
var foo = bar;
(1 || 2).baz();

var foo = bar;
(1 || 2).baz();

var hello = 'world';
[1, 2, 3].forEach(addNumber);

var hello = 'world';
void [1, 2, 3].forEach(addNumber);

let x = function () {};
`hello`;

let tag = function () {};
tag`hello`;
```

### no-unreachable

Examples of incorrect code for this rule:

```javascript
function foo() {
  return true;
  console.log('done');
}

function bar() {
  throw new Error('Oops!');
  console.log('done');
}

while (value) {
  break;
  console.log('done');
}

throw new Error('Oops!');
console.log('done');

function baz() {
  if (Math.random() < 0.5) {
    return;
  } else {
    throw new Error();
  }
  console.log('done');
}

for (;;) {}
console.log('done');
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
      throw new Error('Doesn't exist.');
    }
  }
}

for (key in obj) {
  if (key.startsWith('_')) {
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
  throw new Error('Doesn't exist.');
}

for (key in obj) {
  if (key.startsWith('_')) {
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
    console.log('hola!');
  }
};
```

### no-unsafe-negation

Examples of incorrect code for this rule:

```javascript
if (!key in object) {
  // operator precedence makes it equivalent to (!key) in object
  // and type conversion makes it equivalent to (key ? 'false' : 'true') in object
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

examples of incorrect code for this rule:

```javascript
typeof foo === 'strnig';
typeof foo == 'undefimed';
typeof bar != 'nunber';
typeof bar !== 'fucntion';
```

examples of correct code for this rule:

```javascript
typeof foo === 'string';
typeof bar == 'undefined';
typeof foo === baz;
typeof bar === typeof qux;
```

## Best Practices Rules

### block-scoped-var

examples of incorrect code for this rule:

```javascript
/*eslint block-scoped-var: 'error'*/

function doIf() {
  if (true) {
    var build = true;
  }

  console.log(build);
}

function doIfElse() {
  if (true) {
    var build = true;
  } else {
    var build = false;
  }
}

function doTryCatch() {
  try {
    var build = 1;
  } catch (e) {
    var f = build;
  }
}

function doFor() {
  for (var x = 1; x < 10; x++) {
    var y = f(x);
  }
  console.log(y);
}

class C {
  static {
    if (something) {
      var build = true;
    }
    build = false;
  }
}
```

examples of correct code for this rule:

```javascript
/*eslint block-scoped-var: 'error'*/

function doIf() {
  var build;

  if (true) {
    build = true;
  }

  console.log(build);
}

function doIfElse() {
  var build;

  if (true) {
    build = true;
  } else {
    build = false;
  }
}

function doTryCatch() {
  var build;
  var f;

  try {
    build = 1;
  } catch (e) {
    f = build;
  }
}

function doFor() {
  for (var x = 1; x < 10; x++) {
    var y = f(x);
    console.log(y);
  }
}

class C {
  static {
    var build = false;
    if (something) {
      build = true;
    }
  }
}
```

### complexity

examples of incorrect code for this rule:

```javascript
/*eslint complexity: ['error', 2]*/

function a(x) {
  if (true) {
    return x;
  } else if (false) {
    return x + 1;
  } else {
    return 4; // 3rd path
  }
}

function b() {
  foo ||= 1;
  bar &&= 1;
}
```

examples of correct code for this rule:

```javascript
/*eslint complexity: ['error', 2]*/

function a(x) {
  if (true) {
    return x;
  } else {
    return 4;
  }
}

function b() {
  foo ||= 1;
}
```

### consistent-return

examples of incorrect code for this rule:

```javascript
/*eslint consistent-return: 'error'*/

function doSomething(condition) {
  if (condition) {
    return true;
  } else {
    return;
  }
}

function doSomething(condition) {
  if (condition) {
    return true;
  }
}
```

examples of correct code for this rule:

```javascript
/*eslint consistent-return: 'error'*/

function doSomething(condition) {
  if (condition) {
    return true;
  } else {
    return false;
  }
}

function Foo() {
  if (!(this instanceof Foo)) {
    return new Foo();
  }

  this.a = 0;
}
```

### curly

examples of incorrect code for this rule:

```javascript
/*eslint curly: 'error'*/

if (foo) foo++;

while (bar) baz();

if (foo) {
  baz();
} else qux();
```

examples of correct code for this rule:

```javascript
/*eslint curly: 'error'*/

if (foo) {
  foo++;
}

while (bar) {
  baz();
}

if (foo) {
  baz();
} else {
  qux();
}
```

### default-case

examples of incorrect code for this rule:

```javascript
/*eslint default-case: 'error'*/

switch (a) {
  case 1:
    /* code */
    break;
}
```

examples of correct code for this rule:

```javascript
/*eslint default-case: 'error'*/

switch (a) {
  case 1:
    /* code */
    break;

  default:
    /* code */
    break;
}

switch (a) {
  case 1:
    /* code */
    break;

  // no default
}

switch (a) {
  case 1:
    /* code */
    break;

  // No Default
}
```

### default-case-last

examples of incorrect code for this rule:

```javascript
/*eslint default-case-last: 'error'*/

switch (foo) {
  default:
    bar();
    break;
  case 'a':
    baz();
    break;
}

switch (foo) {
  case 1:
    bar();
    break;
  default:
    baz();
    break;
  case 2:
    quux();
    break;
}

switch (foo) {
  case 'x':
    bar();
    break;
  default:
  case 'y':
    baz();
    break;
}

switch (foo) {
  default:
    break;
  case -1:
    bar();
    break;
}

switch (foo) {
  default:
    doSomethingIfNotZero();
  case 0:
    doSomethingAnyway();
}
```

examples of correct code for this rule:

```javascript
/*eslint default-case-last: 'error'*/

switch (foo) {
  case 'a':
    baz();
    break;
  default:
    bar();
    break;
}

switch (foo) {
  case 1:
    bar();
    break;
  case 2:
    quux();
    break;
  default:
    baz();
    break;
}

switch (foo) {
  case 'x':
    bar();
    break;
  case 'y':
  default:
    baz();
    break;
}

switch (foo) {
  case -1:
    bar();
    break;
}

if (foo !== 0) {
  doSomethingIfNotZero();
}
doSomethingAnyway();
```

### default-param-last

examples of incorrect code for this rule:

```javascript
/* eslint default-param-last: ['error'] */

function f(a = 0, b) {}

function f(a, b = 0, c) {}
```

examples of correct code for this rule:

```javascript
/* eslint default-param-last: ['error'] */

function f(a, b = 0) {}
```

### dot-notation

examples of incorrect code for this rule:

```javascript
/*eslint dot-notation: 'error'*/

var x = foo['bar'];
```

examples of correct code for this rule:

```javascript
/*eslint dot-notation: 'error'*/

var x = foo.bar;

var x = foo[bar]; // Property name is a variable, square-bracket notation required
```

### eqeqeq

examples of incorrect code for this rule:

```javascript
/*eslint eqeqeq: 'error'*/

if (x == 42) {
}

if ('' == text) {
}

if (obj.getStuff() != undefined) {
}
```

examples of correct code for this rule:

```javascript
/*eslint eqeqeq: ['error', 'always']*/

a == b;
foo == true;
bananas != 1;
value == undefined;
typeof foo == 'undefined';
'hello' != 'world';
0 == 0;
true == true;
foo == null;
```

### guard-for-in

examples of incorrect code for this rule:

```javascript
/*eslint guard-for-in: 'error'*/

for (key in foo) {
  doSomething(key);
}
```

examples of correct code for this rule:

```javascript
/*eslint guard-for-in: 'error'*/

for (key in foo) {
  if (Object.prototype.hasOwnProperty.call(foo, key)) {
    doSomething(key);
  }
}

for (key in foo) {
  if ({}.hasOwnProperty.call(foo, key)) {
    doSomething(key);
  }
}
```

### max-classes-per-file

examples of incorrect code for this rule:

```javascript
/*eslint max-classes-per-file: 'error'*/

class Foo {}
class Bar {}
```

examples of correct code for this rule:

```javascript
/*eslint max-classes-per-file: 'error'*/

class Foo {}
```

### no-alert

examples of incorrect code for this rule:

```javascript
/*eslint no-alert: 'error'*/

alert('here!');

confirm('Are you sure?');

prompt('What's your name?', 'John Doe');
```

examples of correct code for this rule:

```javascript
/*eslint no-alert: 'error'*/

customAlert('Something happened!');

customConfirm('Are you sure?');

customPrompt('Who are you?');

function foo() {
  var alert = myCustomLib.customAlert;
  alert();
}
```

### no-caller

examples of incorrect code for this rule:

```javascript
/*eslint no-caller: 'error'*/

function foo(n) {
  if (n <= 0) {
    return;
  }

  arguments.callee(n - 1);
}

[1, 2, 3, 4, 5].map(function (n) {
  return !(n > 1) ? 1 : arguments.callee(n - 1) * n;
});
```

examples of correct code for this rule:

```javascript
/*eslint no-caller: 'error'*/

function foo(n) {
  if (n <= 0) {
    return;
  }

  foo(n - 1);
}

[1, 2, 3, 4, 5].map(function factorial(n) {
  return !(n > 1) ? 1 : factorial(n - 1) * n;
});
```

### no-constructor-return

examples of incorrect code for this rule:

```javascript
/*eslint no-constructor-return: 'error'*/

class A {
  constructor(a) {
    this.a = a;
    return a;
  }
}

class B {
  constructor(f) {
    if (!f) {
      return 'falsy';
    }
  }
}
```

examples of correct code for this rule:

```javascript
/*eslint no-constructor-return: 'error'*/

class C {
  constructor(c) {
    this.c = c;
  }
}

class D {
  constructor(f) {
    if (!f) {
      return; // Flow control.
    }

    f();
  }
}
```

### no-else-return

examples of incorrect code for this rule:

```javascript
/*eslint no-else-return: 'error'*/

function foo() {
  if (x) {
    return y;
  } else {
    return z;
  }
}

function foo() {
  if (x) {
    return y;
  } else if (z) {
    return w;
  } else {
    return t;
  }
}

function foo() {
  if (x) {
    return y;
  } else {
    var t = 'foo';
  }

  return t;
}

function foo() {
  if (error) {
    return 'It failed';
  } else {
    if (loading) {
      return 'It's still loading';
    }
  }
}

// Two warnings for nested occurrences
function foo() {
  if (x) {
    if (y) {
      return y;
    } else {
      return x;
    }
  } else {
    return z;
  }
}
```

examples of correct code for this rule:

```javascript
/*eslint no-else-return: 'error'*/

function foo() {
  if (x) {
    return y;
  }

  return z;
}

function foo() {
  if (x) {
    return y;
  } else if (z) {
    var t = 'foo';
  } else {
    return w;
  }
}

function foo() {
  if (x) {
    if (z) {
      return y;
    }
  } else {
    return z;
  }
}

function foo() {
  if (error) {
    return 'It failed';
  } else if (loading) {
    return 'It's still loading';
  }
}
```

### no-empty-pattern

examples of incorrect code for this rule:

```javascript
/*eslint no-empty-pattern: 'error'*/

var {} = foo;
var [] = foo;
var {
  a: {},
} = foo;
var {
  a: [],
} = foo;
function foo({}) {}
function foo([]) {}
function foo({ a: {} }) {}
function foo({ a: [] }) {}
```

examples of correct code for this rule:

```javascript
/*eslint no-empty-pattern: 'error'*/

var { a = {} } = foo;
var { a = [] } = foo;
function foo({ a = {} }) {}
function foo({ a = [] }) {}
```

### no-eq-null

examples of incorrect code for this rule:

```javascript
/*eslint no-eq-null: 'error'*/

if (foo == null) {
  bar();
}

while (qux != null) {
  baz();
}
```

examples of correct code for this rule:

```javascript
/*eslint no-eq-null: 'error'*/

if (foo === null) {
  bar();
}

while (qux !== null) {
  baz();
}
```

### no-eval

examples of incorrect code for this rule:

```javascript
/*eslint no-eval: 'error'*/

var obj = { x: 'foo' },
  key = 'x',
  value = eval('obj.' + key);

(0, eval)('var a = 0');

var foo = eval;
foo('var a = 0');

// This `this` is the global object.
this.eval('var a = 0');
```

examples of correct code for this rule:

```javascript
/*eslint no-eval: 'error'*/
/*eslint-env browser*/

window.eval('var a = 0');
```

### no-extend-native

examples of incorrect code for this rule:

```javascript
/*eslint no-extend-native: 'error'*/

Object.prototype.a = 'a';
Object.defineProperty(Array.prototype, 'times', { value: 999 });
```

examples of correct code for this rule:

```javascript
/*eslint no-extend-native: ['error', { 'exceptions': ['Object'] }]*/

Object.prototype.a = 'a';
```

### no-extra-bind

examples of incorrect code for this rule:

```javascript
/*eslint no-extra-bind: 'error'*/
/*eslint-env es6*/

var x = function () {
  foo();
}.bind(bar);

var x = (() => {
  foo();
}).bind(bar);

var x = (() => {
  this.foo();
}).bind(bar);

var x = function () {
  (function () {
    this.foo();
  })();
}.bind(bar);

var x = function () {
  function foo() {
    this.bar();
  }
}.bind(baz);
```

examples of correct code for this rule:

```javascript
/*eslint no-extra-bind: 'error'*/

var x = function () {
  this.foo();
}.bind(bar);

var x = function (a) {
  return a + 1;
}.bind(foo, bar);
```

### no-extra-label

examples of incorrect code for this rule:

```javascript
/*eslint no-extra-label: 'error'*/

A: while (a) {
  break A;
}

B: for (let i = 0; i < 10; ++i) {
  break B;
}

C: switch (a) {
  case 0:
    break C;
}
```

examples of correct code for this rule:

```javascript
/*eslint no-extra-label: 'error'*/

while (a) {
  break;
}

for (let i = 0; i < 10; ++i) {
  break;
}

switch (a) {
  case 0:
    break;
}

A: {
  break A;
}

B: while (a) {
  while (b) {
    break B;
  }
}

C: switch (a) {
  case 0:
    while (b) {
      break C;
    }
    break;
}
```

### no-fallthrough

examples of incorrect code for this rule:

```javascript
/*eslint no-fallthrough: 'error'*/

switch (foo) {
  case 1:
    doSomething();

  case 2:
    doSomething();
}
```

examples of correct code for this rule:

```javascript
/*eslint no-fallthrough: 'error'*/

switch (foo) {
  case 1:
    doSomething();
    break;

  case 2:
    doSomething();
}

function bar(foo) {
  switch (foo) {
    case 1:
      doSomething();
      return;

    case 2:
      doSomething();
  }
}

switch (foo) {
  case 1:
    doSomething();
    throw new Error('Boo!');

  case 2:
    doSomething();
}

switch (foo) {
  case 1:
  case 2:
    doSomething();
}

switch (foo) {
  case 1:
    doSomething();
  // falls through

  case 2:
    doSomething();
}

switch (foo) {
  case 1: {
    doSomething();
    // falls through
  }

  case 2: {
    doSomethingElse();
  }
}
```

### no-floating-decimal

examples of incorrect code for this rule:

```javascript
/*eslint no-floating-decimal: 'error'*/

var num = 0.5;
var num = 2;
var num = -0.7;
```

examples of correct code for this rule:

```javascript
/*eslint no-floating-decimal: 'error'*/

var num = 0.5;
var num = 2.0;
var num = -0.7;
```

### no-global-assign

examples of incorrect code for this rule:

```javascript
/*eslint no-global-assign: 'error'*/

Object = null;
undefined = 1;
```

examples of correct code for this rule:

```javascript
/*eslint no-global-assign: 'error'*/
/*eslint-env browser*/

window = {};
length = 1;
top = 1;
```

### no-implicit-coercion

examples of incorrect code for this rule:

```javascript
/*eslint no-implicit-coercion: 'error'*/

var b = !!foo;
var b = ~foo.indexOf('.');
// bitwise not is incorrect only with `indexOf`/`lastIndexOf` method calling.
```

examples of correct code for this rule:

```javascript
/*eslint no-implicit-coercion: 'error'*/

var b = Boolean(foo);
var b = foo.indexOf('.') !== -1;

var n = ~foo; // This is a just bitwise not.
```

### no-implicit-globals:

### no-implied-eval

examples of incorrect code for this rule:

```javascript
/*eslint no-implied-eval: 'error'*/

setTimeout('alert('Hi!');', 100);

setInterval('alert('Hi!');', 100);

execScript('alert('Hi!')');

window.setTimeout('count = 5', 10);

window.setInterval('foo = bar', 10);
```

examples of correct code for this rule:

```javascript
/*eslint no-implied-eval: 'error'*/

setTimeout(function () {
  alert('Hi!');
}, 100);

setInterval(function () {
  alert('Hi!');
}, 100);
```

### no-invalid-this

examples of incorrect code for this rule:

```javascript
/*eslint no-invalid-this: 'error'*/
/*eslint-env es6*/

'use strict';

this.a = 0;
baz(() => this);

(function () {
  this.a = 0;
  baz(() => this);
})();

function foo() {
  this.a = 0;
  baz(() => this);
}

var foo = function () {
  this.a = 0;
  baz(() => this);
};

foo(function () {
  this.a = 0;
  baz(() => this);
});

obj.foo = () => {
  // `this` of arrow functions is the outer scope's.
  this.a = 0;
};

var obj = {
  aaa: function () {
    return function foo() {
      // There is in a method `aaa`, but `foo` is not a method.
      this.a = 0;
      baz(() => this);
    };
  },
};

foo.forEach(function () {
  this.a = 0;
  baz(() => this);
});
```

examples of correct code for this rule:

```javascript
/*eslint no-invalid-this: 'error'*/
/*eslint-env es6*/

'use strict';

function Foo() {
  // OK, this is in a legacy style constructor.
  this.a = 0;
  baz(() => this);
}

class Foo {
  constructor() {
    // OK, this is in a constructor.
    this.a = 0;
    baz(() => this);
  }
}

var obj = {
  foo: function foo() {
    // OK, this is in a method (this function is on object literal).
    this.a = 0;
  },
};

var obj = {
  foo() {
    // OK, this is in a method (this function is on object literal).
    this.a = 0;
  },
};

var obj = {
  get foo() {
    // OK, this is in a method (this function is on object literal).
    return this.a;
  },
};

var obj = Object.create(null, {
  foo: {
    value: function foo() {
      // OK, this is in a method (this function is on object literal).
      this.a = 0;
    },
  },
});

Object.defineProperty(obj, 'foo', {
  value: function foo() {
    // OK, this is in a method (this function is on object literal).
    this.a = 0;
  },
});

Object.defineProperties(obj, {
  foo: {
    value: function foo() {
      // OK, this is in a method (this function is on object literal).
      this.a = 0;
    },
  },
});

function Foo() {
  this.foo = function foo() {
    // OK, this is in a method (this function assigns to a property).
    this.a = 0;
    baz(() => this);
  };
}

obj.foo = function foo() {
  // OK, this is in a method (this function assigns to a property).
  this.a = 0;
};

Foo.prototype.foo = function foo() {
  // OK, this is in a method (this function assigns to a property).
  this.a = 0;
};

class Foo {
  // OK, this is in a class field initializer.
  a = this.b;

  // OK, static initializers also have valid this.
  static a = this.b;

  foo() {
    // OK, this is in a method.
    this.a = 0;
    baz(() => this);
  }

  static foo() {
    // OK, this is in a method (static methods also have valid this).
    this.a = 0;
    baz(() => this);
  }

  static {
    // OK, static blocks also have valid this.
    this.a = 0;
    baz(() => this);
  }
}

var foo = function foo() {
  // OK, the `bind` method of this function is called directly.
  this.a = 0;
}.bind(obj);

foo.forEach(function () {
  // OK, `thisArg` of `.forEach()` is given.
  this.a = 0;
  baz(() => this);
}, thisArg);

/** @this Foo */
function foo() {
  // OK, this function has a `@this` tag in its JSDoc comment.
  this.a = 0;
}
```

### no-empty-function

examples of incorrect code for this rule:

```javascript
/*eslint no-empty-function: 'error'*/
/*eslint-env es6*/

function foo() {}

var foo = function () {};

var foo = () => {};

function* foo() {}

var foo = function* () {};

var obj = {
  foo: function () {},

  foo: function* () {},

  foo() {},

  *foo() {},

  get foo() {},

  set foo(value) {},
};

class A {
  constructor() {}

  foo() {}

  *foo() {}

  get foo() {}

  set foo(value) {}

  static foo() {}

  static *foo() {}

  static get foo() {}

  static set foo(value) {}
}
```

examples of correct code for this rule:

```javascript
/*eslint no-empty-function: 'error'*/
/*eslint-env es6*/

function foo() {
  // do nothing.
}

var foo = function () {
  // any clear comments.
};

var foo = () => {
  bar();
};

function* foo() {
  // do nothing.
}

var foo = function* () {
  // do nothing.
};

var obj = {
  foo: function () {
    // do nothing.
  },

  foo: function* () {
    // do nothing.
  },

  foo() {
    // do nothing.
  },

  *foo() {
    // do nothing.
  },

  get foo() {
    // do nothing.
  },

  set foo(value) {
    // do nothing.
  },
};

class A {
  constructor() {
    // do nothing.
  }

  foo() {
    // do nothing.
  }

  *foo() {
    // do nothing.
  }

  get foo() {
    // do nothing.
  }

  set foo(value) {
    // do nothing.
  }

  static foo() {
    // do nothing.
  }

  static *foo() {
    // do nothing.
  }

  static get foo() {
    // do nothing.
  }

  static set foo(value) {
    // do nothing.
  }
}
```

### no-iterator

examples of incorrect code for this rule:

```javascript
/*eslint no-iterator: 'error'*/

Foo.prototype.__iterator__ = function () {
  return new FooIterator(this);
};

foo.__iterator__ = function () {};

foo['__iterator__'] = function () {};
```

examples of correct code for this rule:

```javascript
/*eslint no-iterator: 'error'*/

var __iterator__ = foo; // Not using the `__iterator__` property.
```

### no-lone-blocks

examples of incorrect code for this rule:

```javascript
/*eslint no-lone-blocks: 'error'*/

{
}

if (foo) {
  bar();
  {
    baz();
  }
}

function bar() {
  {
    baz();
  }
}

{
  function foo() {}
}

{
  aLabel: {
  }
}

class C {
  static {
    {
      foo();
    }
  }
}
```

examples of correct code for this rule:

```javascript
/*eslint no-lone-blocks: 'error'*/
/*eslint-env es6*/

while (foo) {
  bar();
}

if (foo) {
  if (bar) {
    baz();
  }
}

function bar() {
  baz();
}

{
  let x = 1;
}

{
  const y = 1;
}

{
  class Foo {}
}

aLabel: {
}

class C {
  static {
    lbl: {
      if (something) {
        break lbl;
      }

      foo();
    }
  }
}
```

### no-loop-func

examples of incorrect code for this rule:

```javascript
/*eslint no-loop-func: 'error'*/
/*eslint-env es6*/

for (var i = 10; i; i--) {
  (function () {
    return i;
  })();
}

while (i) {
  var a = function () {
    return i;
  };
  a();
}

do {
  function a() {
    return i;
  }
  a();
} while (i);

let foo = 0;
for (let i = 0; i < 10; ++i) {
  //Bad, `foo` is not in the loop-block's scope and `foo` is modified in/after the loop
  setTimeout(() => console.log(foo));
  foo += 1;
}

for (let i = 0; i < 10; ++i) {
  //Bad, `foo` is not in the loop-block's scope and `foo` is modified in/after the loop
  setTimeout(() => console.log(foo));
}
foo = 100;
```

examples of correct code for this rule:

```javascript
/*eslint no-loop-func: 'error'*/
/*eslint-env es6*/

var a = function () {};

for (var i = 10; i; i--) {
  a();
}

for (var i = 10; i; i--) {
  var a = function () {}; // OK, no references to variables in the outer scopes.
  a();
}

for (let i = 10; i; i--) {
  var a = function () {
    return i;
  }; // OK, all references are referring to block scoped variables in the loop.
  a();
}

var foo = 100;
for (let i = 10; i; i--) {
  var a = function () {
    return foo;
  }; // OK, all references are referring to never modified variables.
  a();
}
//... no modifications of foo after this loop ...
```

### no-magic-numbers

examples of incorrect code for this rule:

```javascript
/*eslint no-magic-numbers: 'error'*/

var dutyFreePrice = 100,
  finalPrice = dutyFreePrice + dutyFreePrice * 0.25;
```

examples of correct code for this rule:

```javascript
/*eslint no-magic-numbers: 'error'*/

var data = ['foo', 'bar', 'baz'];

var dataLast = data[2];
```

### no-multi-spaces

examples of incorrect code for this rule:

```javascript
/*eslint no-multi-spaces: 'error'*/

var a = 1;

if (foo === 'bar') {
}

a << b;

var arr = [1, 2];

a ? b : c;
```

examples of correct code for this rule:

```javascript
/*eslint no-multi-spaces: 'error'*/

var a = 1;

if (foo === 'bar') {
}

a << b;

var arr = [1, 2];

a ? b : c;
```

### no-new

examples of incorrect code for this rule:

```javascript
/*eslint no-new: 'error'*/

new Thing();
```

examples of correct code for this rule:

```javascript
/*eslint no-new: 'error'*/

var thing = new Thing();

Thing();
```

### no-new-func

examples of incorrect code for this rule:

```javascript
/*eslint no-new-func: 'error'*/

var x = new Function('a', 'b', 'return a + b');
var x = Function('a', 'b', 'return a + b');
var x = Function.call(null, 'a', 'b', 'return a + b');
var x = Function.apply(null, ['a', 'b', 'return a + b']);
var x = Function.bind(null, 'a', 'b', 'return a + b')();
var f = Function.bind(null, 'a', 'b', 'return a + b'); // assuming that the result of Function.bind(...) will be eventually called.
```

examples of correct code for this rule:

```javascript
/*eslint no-new-func: 'error'*/

var x = function (a, b) {
  return a + b;
};
```

### no-new-wrappers

examples of incorrect code for this rule:

```javascript
/*eslint no-new-wrappers: 'error'*/

var stringObject = new String('Hello world');
var numberObject = new Number(33);
var booleanObject = new Boolean(false);

var stringObject = new String();
var numberObject = new Number();
var booleanObject = new Boolean();
```

examples of correct code for this rule:

```javascript
/*eslint no-new-wrappers: 'error'*/

var text = String(someValue);
var num = Number(someValue);

var object = new MyString();
```

### no-param-reassign

examples of incorrect code for this rule:

```javascript
/*eslint no-param-reassign: 'error'*/

function foo(bar) {
  bar = 13;
}

function foo(bar) {
  bar++;
}

function foo(bar) {
  for (bar in baz) {
  }
}

function foo(bar) {
  for (bar of baz) {
  }
}
```

examples of correct code for this rule:

```javascript
/*eslint no-param-reassign: 'error'*/

function foo(bar) {
  var baz = bar;
}
```

### no-proto

examples of incorrect code for this rule:

```javascript
/*eslint no-proto: 'error'*/

var a = obj.__proto__;

var a = obj['__proto__'];

obj.__proto__ = b;

obj['__proto__'] = b;
```

examples of correct code for this rule:

```javascript
/*eslint no-proto: 'error'*/

var a = Object.getPrototypeOf(obj);

Object.setPrototypeOf(obj, b);

var c = { __proto__: a };
```

### no-redeclare

examples of incorrect code for this rule:

```javascript
/*eslint no-redeclare: 'error'*/

var a = 3;
var a = 10;

class C {
  foo() {
    var b = 3;
    var b = 10;
  }

  static {
    var c = 3;
    var c = 10;
  }
}
```

examples of correct code for this rule:

```javascript
/*eslint no-redeclare: 'error'*/

var a = 3;
a = 10;

class C {
  foo() {
    var b = 3;
    b = 10;
  }

  static {
    var c = 3;
    c = 10;
  }
}
```

### no-return-assign

examples of incorrect code for this rule:

```javascript
/*eslint no-return-assign: 'error'*/

function doSomething() {
  return (foo = bar + 2);
}

function doSomething() {
  return (foo += 2);
}

const foo = (a, b) => (a = b);

const bar = (a, b, c) => ((a = b), c == b);

function doSomething() {
  return (foo = bar && foo > 0);
}
```

examples of correct code for this rule:

```javascript
/*eslint no-return-assign: 'error'*/

function doSomething() {
  return foo == bar + 2;
}

function doSomething() {
  return foo === bar + 2;
}

function doSomething() {
  return (foo = bar + 2);
}

const foo = (a, b) => (a = b);

const bar = (a, b, c) => ((a = b), c == b);

function doSomething() {
  return (foo = bar) && foo > 0;
}
```

### no-return-await

examples of incorrect code for this rule:

```javascript
/*eslint no-return-await: 'error'*/

async function foo() {
  return await bar();
}
```

examples of correct code for this rule:

```javascript
/*eslint no-return-await: 'error'*/

async function foo() {
  return bar();
}

async function foo() {
  await bar();
  return;
}

// This is essentially the same as `return await bar();`, but the rule checks only `await` in `return` statements
async function foo() {
  const x = await bar();
  return x;
}

// In this example the `await` is necessary to be able to catch errors thrown from `bar()`
async function foo() {
  try {
    return await bar();
  } catch (error) {}
}
```

### no-self-assign

examples of incorrect code for this rule:

```javascript
/*eslint no-self-assign: 'error'*/

foo = foo;

[a, b] = [a, b];

[a, ...b] = [x, ...b];

({ a, b } = { a, x });

foo &&= foo;
foo ||= foo;
foo ??= foo;
```

examples of correct code for this rule:

```javascript
/*eslint no-self-assign: 'error'*/

foo = bar;
[a, b] = [b, a];

// This pattern is warned by the `no-use-before-define` rule.
let foo = foo;

// The default values have an effect.
[foo = 1] = [foo];

// non-self-assignments with properties.
obj.a = obj.b;
obj.a.b = obj.c.b;
obj.a.b = obj.a.c;
obj[a] = obj['a'];

// This ignores if there is a function call.
obj.a().b = obj.a().b;
a().b = a().b;

// `&=` and `|=` have an effect on non-integers.
foo &= foo;
foo |= foo;

// Known limitation: this does not support computed properties except single literal or single identifier.
obj[a + b] = obj[a + b];
obj['a' + 'b'] = obj['a' + 'b'];
```

### no-self-compare

examples of incorrect code for this rule:

```javascript
/*eslint no-self-compare: 'error'*/

var x = 10;
if (x === x) {
  x = 20;
}
```

### no-throw-literal

examples of incorrect code for this rule:

```javascript
/*eslint no-throw-literal: 'error'*/
/*eslint-env es6*/

throw 'error';

throw 0;

throw undefined;

throw null;

var err = new Error();
throw 'an ' + err;
// err is recast to a string literal

var err = new Error();
throw `${err}`;
```

examples of correct code for this rule:

```javascript
/*eslint no-throw-literal: 'error'*/

throw new Error();

throw new Error('error');

var e = new Error('error');
throw e;

try {
  throw new Error('error');
} catch (e) {
  throw e;
}
```

### no-unused-expressions:

### no-useless-call

examples of incorrect code for this rule:

```javascript
/*eslint no-useless-call: 'error'*/

// These are same as `foo(1, 2, 3);`
foo.call(undefined, 1, 2, 3);
foo.apply(undefined, [1, 2, 3]);
foo.call(null, 1, 2, 3);
foo.apply(null, [1, 2, 3]);

// These are same as `obj.foo(1, 2, 3);`
obj.foo.call(obj, 1, 2, 3);
obj.foo.apply(obj, [1, 2, 3]);
```

examples of correct code for this rule:

```javascript
/*eslint no-useless-call: 'error'*/

// The `this` binding is different.
foo.call(obj, 1, 2, 3);
foo.apply(obj, [1, 2, 3]);
obj.foo.call(null, 1, 2, 3);
obj.foo.apply(null, [1, 2, 3]);
obj.foo.call(otherObj, 1, 2, 3);
obj.foo.apply(otherObj, [1, 2, 3]);

// The argument list is variadic.
// Those are warned by the `prefer-spread` rule.
foo.apply(undefined, args);
foo.apply(null, args);
obj.foo.apply(obj, args);
```

### no-useless-concat

examples of incorrect code for this rule:

```javascript
/*eslint no-useless-concat: 'error'*/
/*eslint-env es6*/

var a = `some` + `string`;

// these are the same as '10'
var a = '1' + '0';
var a = '1' + `0`;
var a = `1` + '0';
var a = `1` + `0`;
```

examples of correct code for this rule:

```javascript
/*eslint no-useless-concat: 'error'*/

// when a non string is included
var c = a + b;
var c = '1' + a;
var a = 1 + '1';
var c = 1 - 2;
// when the string concatenation is multiline
var c = 'foo' + 'bar';
```

### no-useless-return

examples of incorrect code for this rule:

```javascript
/* eslint no-useless-return: 'error' */

function foo() {
  return;
}

function foo() {
  doSomething();
  return;
}

function foo() {
  if (condition) {
    bar();
    return;
  } else {
    baz();
  }
}

function foo() {
  switch (bar) {
    case 1:
      doSomething();
    default:
      doSomethingElse();
      return;
  }
}
```

examples of correct code for this rule:

```javascript
/* eslint no-useless-return: 'error' */

function foo() {
  return 5;
}

function foo() {
  return doSomething();
}

function foo() {
  if (condition) {
    bar();
    return;
  } else {
    baz();
  }
  qux();
}

function foo() {
  switch (bar) {
    case 1:
      doSomething();
      return;
    default:
      doSomethingElse();
  }
}

function foo() {
  for (const foo of bar) {
    return;
  }
}
```

### no-void

examples of incorrect code for this rule:

```javascript
/*eslint no-void: 'error'*/

void foo;
void someFunction();

var foo = void bar();
function baz() {
  return void 0;
}
```

examples of correct code for this rule:

```javascript
/*eslint no-void: ['error', { 'allowAsStatement': true }]*/

var foo = void bar();
function baz() {
  return void 0;
}
```

### no-with

examples of incorrect code for this rule:

```javascript
/*eslint no-with: 'error'*/

with (point) {
  r = Math.sqrt(x * x + y * y); // is r a member of point?
}
```

examples of correct code for this rule:

```javascript
/*eslint no-with: 'error'*/
/*eslint-env es6*/

const r = ({ x, y }) => Math.sqrt(x * x + y * y);
```

### wrap-iife

examples of incorrect code for this rule:

```javascript
/*eslint wrap-iife: ['error', 'outside']*/

var x = (function () {
  return { y: 1 };
})(); // unwrapped
var x = (function () {
  return { y: 1 };
})(); // wrapped function expression
```

examples of correct code for this rule:

```javascript
/*eslint wrap-iife: ['error', 'outside']*/

var x = (function () {
  return { y: 1 };
})(); // wrapped call expression
```

### require-await

examples of incorrect code for this rule:

```javascript
/*eslint require-await: 'error'*/

async function foo() {
  doSomething();
}

bar(async () => {
  doSomething();
});
```

examples of correct code for this rule:

```javascript
/*eslint require-await: 'error'*/

async function foo() {
  await doSomething();
}

bar(async () => {
  await doSomething();
});

function foo() {
  doSomething();
}

bar(() => {
  doSomething();
});

// Allow empty functions.
async function noop() {}
```

### yoda

examples of incorrect code for this rule:

```javascript
/*eslint yoda: 'error'*/

if ('red' === color) {
  // ...
}

if (`red` === color) {
  // ...
}

if (`red` === `${color}`) {
  // ...
}

if (true == flag) {
  // ...
}

if (5 > count) {
  // ...
}

if (-1 < str.indexOf(substr)) {
  // ...
}

if (0 <= x && x < 1) {
  // ...
}
```

examples of correct code for this rule:

```javascript
/*eslint yoda: 'error'*/

if (5 & value) {
  // ...
}

if (value === 'red') {
  // ...
}

if (value === `red`) {
  // ...
}

if (`${value}` === `red`) {
}
```

## Variables Rules

### no-delete-var

examples of incorrect code for this rule:

```javascript
/*eslint no-delete-var: 'error'*/

var x;
delete x;
```

### no-shadow

examples of incorrect code for this rule:

```javascript
/*eslint no-shadow: 'error'*/
/*eslint-env es6*/

var a = 3;
function b() {
  var a = 10;
}

var b = function () {
  var a = 10;
};

function b(a) {
  a = 10;
}
b(a);

if (true) {
  let a = 5;
}
```

examples of correct code for this rule:

```javascript
{
    'no-shadow': ['error', { 'builtinGlobals': false, 'hoist': 'functions', 'allow': [] }]
}
```

### no-unused-vars

examples of incorrect code for this rule:

```javascript
/*eslint no-unused-vars: 'error'*/
/*global some_unused_var*/

// It checks variables you have defined as global
some_unused_var = 42;

var x;

// Write-only variables are not considered as used.
var y = 10;
y = 5;

// A read for a modification of itself is not considered as used.
var z = 0;
z = z + 1;

// By default, unused arguments cause warnings.
(function (foo) {
  return 5;
})();

// Unused recursive functions also cause warnings.
function fact(n) {
  if (n < 2) return 1;
  return n * fact(n - 1);
}

// When a function definition destructures an array, unused entries from the array also cause warnings.
function getY([x, y]) {
  return y;
}
```

examples of correct code for this rule:

```javascript
/*eslint no-unused-vars: 'error'*/

var x = 10;
alert(x);

// foo is considered used here
myFunc(
  function foo() {
    // ...
  }.bind(this)
);

(function (foo) {
  return foo;
})();

var myFunc;
myFunc = setTimeout(function () {
  // myFunc is considered used
  myFunc();
}, 50);

// Only the second argument from the destructured array is used.
function getY([, y]) {
  return y;
}
```

### no-use-before-define

examples of incorrect code for this rule:

```javascript
/*eslint no-use-before-define: 'error'*/

alert(a);
var a = 10;

f();
function f() {}

function g() {
  return b;
}
var b = 1;

{
  alert(c);
  let c = 1;
}

{
  class C extends C {}
}

{
  class C {
    static x = 'foo';
    [C.x]() {}
  }
}

{
  const C = class {
    static x = C;
  };
}

{
  const C = class {
    static {
      C.x = 'foo';
    }
  };
}
```

examples of correct code for this rule:

```javascript
/*eslint no-use-before-define: 'error'*/

var a;
a = 10;
alert(a);

function f() {}
f(1);

var b = 1;
function g() {
  return b;
}

{
  let c;
  c++;
}

{
  class C {
    static x = C;
  }
}

{
  const C = class C {
    static x = C;
  };
}

{
  const C = class {
    x = C;
  };
}

{
  const C = class C {
    static {
      C.x = 'foo';
    }
  };
}
```

## Stylistic Issues Rules

### array-bracket-newline

examples of incorrect code for this rule:

```javascript
/*eslint array-bracket-newline: ['error', 'always']*/

var a = [];
var b = [1];
var c = [1, 2];
var d = [1, 2];
var e = [
  function foo() {
    dosomething();
  },
];
```

examples of correct code for this rule:

```javascript
/*eslint array-bracket-newline: ['error', 'always']*/

var a = [];
var b = [1];
var c = [1, 2];
var d = [1, 2];
var e = [
  function foo() {
    dosomething();
  },
];
```

### array-bracket-spacing

examples of incorrect code for this rule:

```javascript
/*eslint array-bracket-spacing: ['error', 'never']*/
/*eslint-env es6*/

var arr = ['foo', 'bar'];
var arr = ['foo', 'bar'];
var arr = [['foo'], 'bar'];
var arr = [['foo'], 'bar'];
var arr = ['foo', 'bar'];
var [x, y] = z;
var [x, y] = z;
var [x, ...y] = z;
var [, , x] = z;
```

examples of correct code for this rule:

```javascript
/*eslint array-bracket-spacing: ['error', 'never']*/
/*eslint-env es6*/

var arr = [];
var arr = ['foo', 'bar', 'baz'];
var arr = [['foo'], 'bar', 'baz'];
var arr = ['foo', 'bar', 'baz'];
var arr = ['foo', 'bar'];
var arr = ['foo', 'bar'];

var [x, y] = z;
var [x, y] = z;
var [x, ...y] = z;
var [, , x] = z;
```

### array-element-newline

examples of incorrect code for this rule:

```javascript
{
    'array-element-newline': ['error', {
        'ArrayExpression': 'consistent',
        'ArrayPattern': { 'minItems': 3 },
    }]
}
```

examples of correct code for this rule:

```javascript
/*eslint array-element-newline: ['error', 'always']*/

var c = [1, 2];
var d = [1, 2, 3];
var e = [1, 2, 3];
var f = [1, 2, 3];
var g = [
  function foo() {
    dosomething();
  },
  function bar() {
    dosomething();
  },
];
```

### block-spacing

examples of incorrect code for this rule:

```javascript
/*eslint block-spacing: 'error'*/

function foo() {
  return true;
}
if (foo) {
  bar = 0;
}
function baz() {
  let i = 0;
  return i;
}

class C {
  static {
    this.bar = 0;
  }
}
```

examples of correct code for this rule:

```javascript
/*eslint block-spacing: 'error'*/

function foo() {
  return true;
}
if (foo) {
  bar = 0;
}

class C {
  static {
    this.bar = 0;
  }
}
```

### brace-style

examples of incorrect code for this rule:

```javascript
/*eslint brace-style: 'error'*/

function foo() {
  return true;
}

if (foo) {
  bar();
}

try {
  somethingRisky();
} catch (e) {
  handleError();
}

if (foo) {
  bar();
} else {
  baz();
}

class C {
  static {
    foo();
  }
}
```

examples of correct code for this rule:

```javascript
/*eslint brace-style: 'error'*/

function foo() {
  return true;
}

if (foo) {
  bar();
}

if (foo) {
  bar();
} else {
  baz();
}

try {
  somethingRisky();
} catch (e) {
  handleError();
}

class C {
  static {
    foo();
  }
}

// when there are no braces, there are no problems
if (foo) bar();
else if (baz) boom();
```

### camelcase

examples of incorrect code for this rule:

```javascript
/*eslint camelcase: 'error'*/

import { no_camelcased } from 'external-module';

var my_favorite_color = '#112C85';

function do_something() {
  // ...
}

obj.do_something = function () {
  // ...
};

function foo({ no_camelcased }) {
  // ...
}

function foo({ isCamelcased: no_camelcased }) {
  // ...
}

function foo({ no_camelcased = 'default value' }) {
  // ...
}

var obj = {
  my_pref: 1,
};

var { category_id = 1 } = query;

var { foo: no_camelcased } = bar;

var { foo: bar_baz = 1 } = quz;
```

examples of correct code for this rule:

```javascript
/*eslint camelcase: 'error'*/

import { no_camelcased as camelCased } from 'external-module';

var myFavoriteColor = '#112C85';
var _myFavoriteColor = '#112C85';
var myFavoriteColor_ = '#112C85';
var MY_FAVORITE_COLOR = '#112C85';
var foo = bar.baz_boom;
var foo = { qux: bar.baz_boom };

obj.do_something();
do_something();
new do_something();

var { category_id: category } = query;

function foo({ isCamelCased }) {
  // ...
}

function foo({ isCamelCased: isAlsoCamelCased }) {
  // ...
}

function foo({ isCamelCased = 'default value' }) {
  // ...
}

var { categoryId = 1 } = query;

var { foo: isCamelCased } = bar;

var { foo: isCamelCased = 1 } = quz;
```

### comma-dangle

examples of incorrect code for this rule:

```javascript
{
    'comma-dangle': ['error', 'never'],
    // or
    'comma-dangle': ['error', {
        'arrays': 'never',
        'objects': 'never',
        'imports': 'never',
        'exports': 'never',
        'functions': 'never'
    }]
}
```

examples of correct code for this rule:

```javascript
/*eslint comma-dangle: ['error', 'never']*/

var foo = {
  bar: 'baz',
  qux: 'quux',
};

var arr = [1, 2];

foo({
  bar: 'baz',
  qux: 'quux',
});
```

### comma-style

examples of incorrect code for this rule:

```javascript
/*eslint comma-style: ['error', 'last']*/

var foo = 1,
  bar = 2;

var foo = 1,
  bar = 2;

var foo = ['apples', 'oranges'];

function bar() {
  return {
    a: 1,
    'b:': 2,
  };
}
```

examples of correct code for this rule:

```javascript
/*eslint comma-style: ['error', 'last']*/

var foo = 1,
  bar = 2;

var foo = 1,
  bar = 2;

var foo = ['apples', 'oranges'];

function bar() {
  return {
    a: 1,
    'b:': 2,
  };
}
```

### computed-property-spacing

examples of incorrect code for this rule:

```javascript
/*eslint computed-property-spacing: ['error', 'never']*/
/*eslint-env es6*/

obj[foo];
obj['foo'];
var x = { [b]: a };
obj[foo[bar]];

const { [a]: someProp } = obj;
({ [b]: anotherProp } = anotherObj);
```

examples of correct code for this rule:

```javascript
/*eslint computed-property-spacing: ['error', 'never']*/
/*eslint-env es6*/

obj[foo];
obj['foo'];
var x = { [b]: a };
obj[foo[bar]];

const { [a]: someProp } = obj;
({ [b]: anotherProp } = anotherObj);
```

### func-call-spacing

examples of incorrect code for this rule:

```javascript
/*eslint func-call-spacing: ['error', 'never']*/

fn();

fn();
```

examples of correct code for this rule:

```javascript
/*eslint func-call-spacing: ['error', 'never']*/

fn();
```

### func-names

examples of incorrect code for this rule:

```javascript
/*eslint func-names: ['error', 'always']*/

Foo.prototype.bar = function () {};

const cat = {
  meow: function () {},
}(
  (function () {
    // ...
  })()
);

export default function () {}
```

examples of correct code for this rule:

```javascript
/*eslint func-names: ['error', 'always']*/

Foo.prototype.bar = function bar() {};

const cat = {
  meow() {},
}(
  (function bar() {
    // ...
  })()
);

export default function foo() {}
```

### func-style

examples of incorrect code for this rule:

```javascript
/*eslint func-style: ['error', 'expression']*/

function foo() {
  // ...
}
```

examples of correct code for this rule:

```javascript
/*eslint func-style: ['error', 'expression']*/

var foo = function () {
  // ...
};

var foo = () => {};

// allowed as allowArrowFunctions : false is applied only for declaration
```

### implicit-arrow-linebreak

examples of incorrect code for this rule:

```javascript
/* eslint implicit-arrow-linebreak: ['error', 'beside'] */

(foo) => bar;

(foo) => bar;

(foo) => (bar) => baz;

(foo) => bar();
```

examples of correct code for this rule:

```javascript
/* eslint implicit-arrow-linebreak: ['error', 'beside'] */

(foo) => bar;

(foo) => bar;

(foo) => (bar) => baz;

(foo) => bar();

// functions with block bodies allowed with this rule using any style
// to enforce a consistent location for this case, see the rule: `brace-style`
(foo) => {
  return bar();
};

(foo) => {
  return bar();
};
```

### jsx-quotes

examples of incorrect code for this rule:

```javascript
/*eslint jsx-quotes: ['error', 'prefer-double']*/

<a b='c' />
```

examples of correct code for this rule:

```javascript
/*eslint jsx-quotes: ['error', 'prefer-double']*/

<a b='c' />
<a b=''' />
```

### key-spacing

examples of incorrect code for this rule:

```javascript
/*eslint key-spacing: ['error', { 'beforeColon': false }]*/

var obj = { foo: 42 };
```

examples of correct code for this rule:

```javascript
/*eslint key-spacing: ['error', { 'beforeColon': false }]*/

var obj = { foo: 42 };
```

### lines-between-class-members

examples of incorrect code for this rule:

```javascript
/* eslint lines-between-class-members: ['error', 'always']*/
class MyClass {
  x;
  foo() {
    //...
  }
  bar() {
    //...
  }
}
```

examples of correct code for this rule:

```javascript
/* eslint lines-between-class-members: ['error', 'always']*/
class MyClass {
  x;

  foo() {
    //...
  }

  bar() {
    //...
  }
}
```

### linebreak-style

examples of incorrect code for this rule:

```javascript
/*eslint linebreak-style: ['error', 'unix']*/

var a = 'a'; // \r\n
```

examples of correct code for this rule:

```javascript
/*eslint linebreak-style: ['error', 'unix']*/

var a = 'a', // \n
  b = 'b'; // \n
// \n
function foo(params) {
  // \n
  // do stuff \n
} // \n
```

### indent

examples of incorrect code for this rule:

```javascript
function hello(indentSize, type) {
  if (indentSize === 4 && type !== 'tab') {
    console.log('Each next indentation will increase on 4 spaces');
  }
}
```

examples of correct code for this rule:

```javascript
{
    'indent': ['error', 2]
}
```

### max-depth

examples of incorrect code for this rule:

```javascript
/*eslint max-depth: ['error', 4]*/

function foo() {
  for (;;) {
    // Nested 1 deep
    while (true) {
      // Nested 2 deep
      if (true) {
        // Nested 3 deep
        if (true) {
          // Nested 4 deep
          if (true) {
            // Nested 5 deep
          }
        }
      }
    }
  }
}
```

examples of correct code for this rule:

```javascript
/*eslint max-depth: ['error', 4]*/

function foo() {
  for (;;) {
    // Nested 1 deep
    while (true) {
      // Nested 2 deep
      if (true) {
        // Nested 3 deep
        if (true) {
          // Nested 4 deep
        }
      }
    }
  }
}
```

### max-len

examples of incorrect code for this rule:

```javascript
/*eslint max-len: ['error', { 'code': 80 }]*/

var foo = {
  bar: 'This is a bar.',
  baz: { qux: 'This is a qux' },
  difficult: 'to read',
};
```

examples of correct code for this rule:

```javascript
/*eslint max-len: ['error', { 'code': 80 }]*/

var foo = {
  bar: 'This is a bar.',
  baz: { qux: 'This is a qux' },
  easier: 'to read',
};
```

### newline-per-chained-call

examples of incorrect code for this rule:

```javascript
/*eslint newline-per-chained-call: ['error', { 'ignoreChainWithDepth': 2 }]*/

_.chain({}).map(foo).filter(bar).value();

// Or
_.chain({}).map(foo).filter(bar);

// Or
_.chain({}).map(foo).filter(bar);

// Or
obj.method().method2().method3();
```

examples of correct code for this rule:

```javascript
/*eslint newline-per-chained-call: ['error', { 'ignoreChainWithDepth': 2 }]*/

_.chain({}).map(foo).filter(bar).value();

// Or
_.chain({}).map(foo).filter(bar);

// Or
_.chain({}).map(foo).filter(bar);

// Or
obj.prop.method().prop;

// Or
obj.prop.method().method2().method3().prop;
```

### no-bitwise

examples of incorrect code for this rule:

```javascript
/*eslint no-bitwise: 'error'*/

var x = y | z;

var x = y & z;

var x = y ^ z;

var x = ~z;

var x = y << z;

var x = y >> z;

var x = y >>> z;

x |= y;

x &= y;

x ^= y;

x <<= y;

x >>= y;

x >>>= y;
```

examples of correct code for this rule:

```javascript
/*eslint no-bitwise: 'error'*/

var x = y || z;

var x = y && z;

var x = y > z;

var x = y < z;

x += y;
```

### no-lonely-if

examples of incorrect code for this rule:

```javascript
/*eslint no-lonely-if: 'error'*/

if (condition) {
  // ...
} else {
  if (anotherCondition) {
    // ...
  }
}

if (condition) {
  // ...
} else {
  if (anotherCondition) {
    // ...
  } else {
    // ...
  }
}
```

examples of correct code for this rule:

```javascript
/*eslint no-lonely-if: 'error'*/

if (condition) {
  // ...
} else if (anotherCondition) {
  // ...
}

if (condition) {
  // ...
} else if (anotherCondition) {
  // ...
} else {
  // ...
}

if (condition) {
  // ...
} else {
  if (anotherCondition) {
    // ...
  }
  doSomething();
}
```

### no-mixed-spaces-and-tabs

examples of incorrect code for this rule:

```javascript
/*eslint no-mixed-spaces-and-tabs: 'error'*/

function add(x, y) {
  // --->..return x + y;

  return x + y;
}

function main() {
  // --->var x = 5,
  // --->....y = 7;

  var x = 5,
    y = 7;
}
```

examples of correct code for this rule:

```javascript
/*eslint no-mixed-spaces-and-tabs: 'error'*/

function add(x, y) {
  // --->return x + y;
  return x + y;
}
```

### no-multi-assign

examples of incorrect code for this rule:

```javascript
/*eslint no-multi-assign: 'error'*/

var a = (b = c = 5);

const foo = (bar = 'baz');

let a = (b = c);

class Foo {
  a = (b = 10);
}

a = b = 'quux';
```

examples of correct code for this rule:

```javascript
/*eslint no-multi-assign: 'error'*/

var a = 5;
var b = 5;
var c = 5;

const foo = 'baz';
const bar = 'baz';

let a = c;
let b = c;

class Foo {
  a = 10;
  b = 10;
}

a = 'quux';
b = 'quux';
```

### no-multiple-empty-lines

examples of incorrect code for this rule:

```javascript
/*eslint no-multiple-empty-lines: 'error'*/

var foo = 5;

var bar = 3;
```

examples of correct code for this rule:

```javascript
/*eslint no-multiple-empty-lines: 'error'*/

var foo = 5;

var bar = 3;
```

### no-negated-condition

examples of incorrect code for this rule:

```javascript
/*eslint no-negated-condition: 'error'*/

if (!a) {
  doSomething();
} else {
  doSomethingElse();
}

if (a != b) {
  doSomething();
} else {
  doSomethingElse();
}

if (a !== b) {
  doSomething();
} else {
  doSomethingElse();
}

!a ? c : b;
```

examples of correct code for this rule:

```javascript
/*eslint no-negated-condition: 'error'*/

if (!a) {
  doSomething();
}

if (!a) {
  doSomething();
} else if (b) {
  doSomething();
}

if (a != b) {
  doSomething();
}

a ? b : c;
```

### no-nested-ternary

examples of incorrect code for this rule:

```javascript
/*eslint no-nested-ternary: 'error'*/

var thing = foo ? bar : baz === qux ? quxx : foobar;

foo ? (baz === qux ? quxx() : foobar()) : bar();
```

examples of correct code for this rule:

```javascript
/*eslint no-nested-ternary: 'error'*/

var thing = foo ? bar : foobar;

var thing;

if (foo) {
  thing = bar;
} else if (baz === qux) {
  thing = quxx;
} else {
  thing = foobar;
}
```

### no-new-object

examples of incorrect code for this rule:

```javascript
/*eslint no-new-object: 'error'*/

var myObject = new Object();

new Object();
```

examples of correct code for this rule:

```javascript
/*eslint no-new-object: 'error'*/

var myObject = new CustomObject();

var myObject = {};

var Object = function Object() {};
new Object();
```

### no-trailing-spaces

examples of incorrect code for this rule:

```javascript
/*eslint no-trailing-spaces: 'error'*/

var foo = 0; //•••••
var baz = 5; //••
//•••••
```

examples of correct code for this rule:

```javascript
/*eslint no-trailing-spaces: 'error'*/

var foo = 0;
var baz = 5;
```

### no-unneeded-ternary

examples of incorrect code for this rule:

```javascript
/*eslint no-unneeded-ternary: 'error'*/

var a = x === 2 ? true : false;

var a = x ? true : false;
```

examples of correct code for this rule:

```javascript
/*eslint no-unneeded-ternary: 'error'*/

var a = x === 2 ? 'Yes' : 'No';

var a = x !== false;

var a = x ? 'Yes' : 'No';

var a = x ? y : x;

f(x ? x : 1); // default assignment - would be disallowed if defaultAssignment option set to false. See option details below.
```

### no-whitespace-before-property

examples of incorrect code for this rule:

```javascript
/*eslint no-whitespace-before-property: 'error'*/

foo[bar];

foo.bar;

foo.bar;

foo.bar.baz;

foo.bar().baz();

foo.bar().baz();
```

examples of correct code for this rule:

```javascript
/*eslint no-whitespace-before-property: 'error'*/

foo.bar;

foo[bar];

foo[bar];

foo.bar.baz;

foo.bar().baz();

foo.bar().baz();

foo.bar().baz();
```

### prefer-object-spread

examples of incorrect code for this rule:

```javascript
/*eslint prefer-object-spread: 'error'*/

Object.assign({}, foo);

Object.assign({}, { foo: 'bar' });

Object.assign({ foo: 'bar' }, baz);

Object.assign({ foo: 'bar' }, Object.assign({ bar: 'foo' }));

Object.assign({}, { foo, bar, baz });

Object.assign({}, { ...baz });

// Object.assign with a single argument that is an object literal
Object.assign({});

Object.assign({ foo: bar });
```

examples of correct code for this rule:

```javascript
/*eslint prefer-object-spread: 'error'*/

Object.assign(...foo);

// Any Object.assign call without an object literal as the first argument
Object.assign(foo, { bar: baz });

Object.assign(foo, Object.assign(bar));

Object.assign(foo, { bar, baz });

Object.assign(foo, { ...baz });
```

### object-curly-spacing

examples of incorrect code for this rule:

```javascript
/*eslint object-curly-spacing: ['error', 'never']*/

var obj = { foo: 'bar' };
var obj = { foo: 'bar' };
var obj = { baz: { foo: 'qux' }, bar };
var obj = { baz: { foo: 'qux' }, bar };
var { x } = y;
import { foo } from 'bar';
```

examples of correct code for this rule:

```javascript
/*eslint object-curly-spacing: ['error', 'never']*/

var obj = { foo: 'bar' };
var obj = { foo: { bar: 'baz' }, qux: 'quxx' };
var obj = {
  foo: 'bar',
};
var obj = { foo: 'bar' };
var obj = {
  foo: 'bar',
};
var obj = {};
var { x } = y;
import { foo } from 'bar';
```

### object-property-newline

examples of incorrect code for this rule:

```javascript
/*eslint object-property-newline: 'error'*/

const obj0 = { foo: 'foo', bar: 'bar', baz: 'baz' };

const obj1 = {
  foo: 'foo',
  bar: 'bar',
  baz: 'baz',
};

const obj2 = {
  foo: 'foo',
  bar: 'bar',
  baz: 'baz',
};

const obj3 = {
  [process.argv[3] ? 'foo' : 'bar']: 0,
  baz: [1, 2, 4, 8],
};

const a = 'antidisestablishmentarianistically';
const b = 'yugoslavyalılaştırabildiklerimizdenmişsiniz';
const obj4 = { a, b };

const domain = process.argv[4];
const obj5 = {
  foo: 'foo',
  [domain.includes(':') ? 'complexdomain' : 'simpledomain']: true,
};
```

examples of correct code for this rule:

```javascript
/*eslint object-property-newline: 'error'*/

const obj1 = {
  foo: 'foo',
  bar: 'bar',
  baz: 'baz',
};

const obj2 = {
  foo: 'foo',
  bar: 'bar',
  baz: 'baz',
};

const user = process.argv[2];
const obj3 = {
  user,
  [process.argv[3] ? 'foo' : 'bar']: 0,
  baz: [1, 2, 4, 8],
};
```

### one-var

examples of incorrect code for this rule:

```javascript
/*eslint one-var: ['error', 'always']*/

function foo() {
  var bar;
  var baz;
  let qux;
  let norf;
}

function foo() {
  const bar = false;
  const baz = true;
  let qux;
  let norf;
}

function foo() {
  var bar;

  if (baz) {
    var qux = true;
  }
}

class C {
  static {
    var foo;
    var bar;
  }

  static {
    var foo;
    if (bar) {
      var baz = true;
    }
  }

  static {
    let foo;
    let bar;
  }
}
```

examples of correct code for this rule:

```javascript
/*eslint one-var: ['error', 'always']*/

function foo() {
  var bar, baz;
  let qux, norf;
}

function foo() {
  const bar = true,
    baz = false;
  let qux, norf;
}

function foo() {
  var bar, qux;

  if (baz) {
    qux = true;
  }
}

function foo() {
  let bar;

  if (baz) {
    let qux;
  }
}

class C {
  static {
    var foo, bar;
  }

  static {
    var foo, baz;
    if (bar) {
      baz = true;
    }
  }

  static {
    let foo, bar;
  }

  static {
    let foo;
    if (bar) {
      let baz;
    }
  }
}
```

### operator-linebreak

examples of incorrect code for this rule:

```javascript
/*eslint operator-linebreak: ['error', 'after']*/

foo = 1 + 2;

foo = 1 + 2;

foo = 5;

if (someCondition || otherCondition) {
}

answer = everything ? 42 : foo;

class Foo {
  a = 1;
  [b] = 2;
  [c] = 3;
}
```

examples of correct code for this rule:

```javascript
/*eslint operator-linebreak: ['error', 'after']*/

foo = 1 + 2;

foo = 1 + 2;

foo = 5;

if (someCondition || otherCondition) {
}

answer = everything ? 42 : foo;

class Foo {
  a = 1;
  [b] = 2;
  [c] = 3;
  d = 4;
}
```

### padded-blocks

examples of incorrect code for this rule:

```javascript
/*eslint padded-blocks: ['error', 'always']*/

if (a) {
  b();
}

if (a) {
  b();
}

if (a) {
  b();
}

if (a) {
  b();
}

if (a) {
  // comment
  b();
}

class C {
  static {
    a();
  }
}
```

examples of correct code for this rule:

```javascript
/*eslint padded-blocks: ['error', 'always']*/

if (a) {
  b();
}

if (a) {
  b();
}

if (a) {
  // comment
  b();
}

class C {
  static {
    a();
  }
}
```

### padding-line-between-statements

examples of incorrect code for this rule:

```javascript
/*eslint padding-line-between-statements: [
    'error',
    { blankLine: 'always', prev: 'var', next: 'return' }
]*/

function foo() {
  var a = 1;

  return a;
}
```

examples of correct code for this rule:

```javascript
{
    'padding-line-between-statements': [
        'error',
        { 'blankLine': LINEBREAK_TYPE, 'prev': STATEMENT_TYPE, 'next': STATEMENT_TYPE },
        { 'blankLine': LINEBREAK_TYPE, 'prev': STATEMENT_TYPE, 'next': STATEMENT_TYPE },
        { 'blankLine': LINEBREAK_TYPE, 'prev': STATEMENT_TYPE, 'next': STATEMENT_TYPE },
        { 'blankLine': LINEBREAK_TYPE, 'prev': STATEMENT_TYPE, 'next': STATEMENT_TYPE },
        ...
    ]
}
```

### quotes

examples of incorrect code for this rule:

```javascript
/*eslint quotes: ['error', 'single']*/

var double = "single";
var unescaped = "a string containing 'double' quotes";
var backtick = `back\ntick`;
```

examples of correct code for this rule:

```javascript
/*eslint quotes: ['error', 'single']*/
/*eslint-env es6*/

var double = 'double';
var backtick = `back
tick`; // backticks are allowed due to newline
var backtick = tag`backtick`; // backticks are allowed due to tag
```

### semi

examples of incorrect code for this rule:

```javascript
/*eslint semi: ['error', 'always']*/

var name = 'ESLint';

object.method = function () {
  // ...
};

class Foo {
  bar = 1;
}
```

examples of correct code for this rule:

```javascript
/*eslint semi: 'error'*/

var name = 'ESLint';

object.method = function () {
  // ...
};

class Foo {
  bar = 1;
}
```

### space-before-blocks

examples of incorrect code for this rule:

```javascript
/*eslint space-before-blocks: 'error'*/

if (a) {
  b();
}

function a() {}

for (;;) {
  b();
}

try {
} catch (a) {}

class Foo {
  constructor() {}
}
```

examples of correct code for this rule:

```javascript
/*eslint space-before-blocks: 'error'*/

if (a) {
  b();
}

if (a) {
  b();
} else {
  /*no error. this is checked by `keyword-spacing` rule.*/
  c();
}

class C {
  static {} /*no error. this is checked by `keyword-spacing` rule.*/
}

function a() {}

for (;;) {
  b();
}

try {
} catch (a) {}
```

### space-in-parens

examples of incorrect code for this rule:

```javascript
'space-in-parens': ['error', 'always']
```

examples of correct code for this rule:

```javascript
/*eslint space-in-parens: ['error', 'never']*/

foo();

foo('bar');
foo('bar');
foo('bar');

foo(/* bar */);

var foo = (1 + 2) * 3;
(function () {
  return 'bar';
})();
```

### space-infix-ops

examples of incorrect code for this rule:

```javascript
'space-infix-ops': ['error', { 'int32Hint': false }]
```

examples of correct code for this rule:

```javascript
/*eslint space-infix-ops: 'error'*/
/*eslint-env es6*/

a + b;

a + b;

a + b;

a ? b : c;

const a = { b: 1 };

var { a = 0 } = bar;

function foo(a = 0) {}
```

### space-unary-ops

examples of incorrect code for this rule:

```javascript
    'space-unary-ops': [
        2, {
          'words': true,
          'nonwords': false,
          'overrides': {
            'new': false,
            '++': true
          }
    }]
```

examples of correct code for this rule:

```javascript
/*eslint space-unary-ops: 'error'*/

typeof !foo;

void { foo: 0 };

new [foo][0]();

delete foo.bar;

++foo;

foo--;

-foo;

+'3';
```

### spaced-comment

examples of incorrect code for this rule:

```javascript
'spaced-comment': ['error', 'always', { 'exceptions': ['-', '+'] }]
```

examples of correct code for this rule:

```javascript
'spaced-comment': ['error', 'always', { 'markers': ['/'] }]
```

### wrap-regex

examples of incorrect code for this rule:

```javascript
/*eslint wrap-regex: 'error'*/

function a() {
  return /foo/.test('bar');
}
```

examples of correct code for this rule:

```javascript
/*eslint wrap-regex: 'error'*/

function a() {
  return /foo/.test('bar');
}
```
