# Hoisting and Temporal Dead Zone (TDZ) - Complete Interview Guide

## Table of Contents
1. [What is Hoisting?](#what-is-hoisting)
2. [Temporal Dead Zone (TDZ)](#temporal-dead-zone-tdz)
3. [Basic Interview Questions](#basic-interview-questions)
4. [Intermediate Interview Questions](#intermediate-interview-questions)
5. [Advanced Interview Questions](#advanced-interview-questions)
6. [Edge Cases](#edge-cases)
7. [Common Interview Traps](#common-interview-traps)

---

## What is Hoisting?

Hoisting is JavaScript's default behavior of moving declarations to the top of the current scope (script or function) before code execution. However, only declarations are hoisted, not initializations.

### Key Points:
- `var` declarations are hoisted and initialized with `undefined`
- `let` and `const` declarations are hoisted but NOT initialized (TDZ)
- Function declarations are fully hoisted (both declaration and definition)
- Function expressions are hoisted as variables

---

## Temporal Dead Zone (TDZ)

The TDZ is the period between entering a scope and the actual declaration being reached where variables cannot be accessed. This applies to `let`, `const`, and `class` declarations.

### Key Points:
- TDZ starts from the beginning of the scope
- TDZ ends when the variable declaration is reached
- Accessing variables in TDZ throws `ReferenceError`
- TDZ exists to catch programming errors

---

## Basic Interview Questions

### Q1: What will be the output?

```javascript
console.log(x);
var x = 5;
```

**Answer:** `undefined`

**Explanation:** Due to hoisting, the code is interpreted as:
```javascript
var x;
console.log(x); // undefined
x = 5;
```

---

### Q2: What will be the output?

```javascript
console.log(y);
let y = 10;
```

**Answer:** `ReferenceError: Cannot access 'y' before initialization`

**Explanation:** `let` declarations are hoisted but not initialized. Accessing them before declaration results in TDZ error.

---

### Q3: What will be the output?

```javascript
console.log(greet());

function greet() {
    return "Hello!";
}
```

**Answer:** `"Hello!"`

**Explanation:** Function declarations are fully hoisted, so they can be called before their declaration in the code.

---

### Q4: What will be the output?

```javascript
console.log(sayHi());

var sayHi = function() {
    return "Hi!";
};
```

**Answer:** `TypeError: sayHi is not a function`

**Explanation:** Function expressions are hoisted as variables. The variable `sayHi` is `undefined` at the point of call.

---

### Q5: What will be the output?

```javascript
var a = 1;

function test() {
    console.log(a);
    var a = 2;
}

test();
```

**Answer:** `undefined`

**Explanation:** The local `var a` is hoisted to the top of the function scope, shadowing the global `a`.

---

## Intermediate Interview Questions

### Q6: What will be the output?

```javascript
let x = 1;

if (true) {
    console.log(x);
    let x = 2;
}
```

**Answer:** `ReferenceError: Cannot access 'x' before initialization`

**Explanation:** The block-scoped `let x` creates a TDZ within the if block. The inner `x` shadows the outer one.

---

### Q7: What will be the output?

```javascript
function test() {
    console.log(a);
    console.log(foo());
    
    var a = 1;
    function foo() {
        return 2;
    }
}

test();
```

**Answer:** 
```
undefined
2
```

**Explanation:** Both `var a` and `function foo` are hoisted. `a` is initialized to `undefined`, while `foo` is fully hoisted.

---

### Q8: What will be the output?

```javascript
var x = 10;

function outer() {
    console.log(x);
    
    function inner() {
        console.log(x);
        var x = 20;
    }
    
    inner();
}

outer();
```

**Answer:**
```
10
undefined
```

**Explanation:** In `outer()`, `x` refers to global. In `inner()`, local `var x` is hoisted and shadows the outer scope.

---

### Q9: What will be the output?

```javascript
console.log(typeof undeclaredVariable);
console.log(typeof declaredButNotInitialized);

let declaredButNotInitialized;
```

**Answer:**
```
undefined
ReferenceError: Cannot access 'declaredButNotInitialized' before initialization
```

**Explanation:** `typeof` on undeclared variables returns `"undefined"`. But accessing `let` in TDZ throws an error.

---

### Q10: What will be the output?

```javascript
const PI = 3.14;

function test() {
    console.log(PI);
    const PI = 3.14159;
}

test();
```

**Answer:** `ReferenceError: Cannot access 'PI' before initialization`

**Explanation:** The inner `const PI` creates a TDZ in the function scope, even though there's a global `PI`.

---

### Q11: What will be the output?

```javascript
var a = 1;
var a = 2;

let b = 1;
let b = 2;

console.log(a);
```

**Answer:** `SyntaxError: Identifier 'b' has already been declared`

**Explanation:** `var` allows re-declaration, but `let` and `const` do not. The code won't run due to syntax error.

---

### Q12: What will be the output?

```javascript
for (var i = 0; i < 3; i++) {
    setTimeout(() => console.log(i), 100);
}

for (let j = 0; j < 3; j++) {
    setTimeout(() => console.log(j), 100);
}
```

**Answer:**
```
3
3
3
0
1
2
```

**Explanation:** `var i` has function scope, so all callbacks share the same `i`. `let j` has block scope, creating a new binding for each iteration.

---

## Advanced Interview Questions

### Q13: What will be the output?

```javascript
let x = 1;

{
    console.log(x);
    {
        console.log(x);
        let x = 2;
    }
}
```

**Answer:**
```
1
ReferenceError: Cannot access 'x' before initialization
```

**Explanation:** The inner block has its own TDZ for `let x = 2`, which starts from the beginning of that block.

---

### Q14: What will be the output?

```javascript
function test() {
    return foo;
    foo = 10;
    function foo() {}
    var foo = 11;
}

console.log(typeof test());
```

**Answer:** `"function"`

**Explanation:** After hoisting:
```javascript
function test() {
    function foo() {}  // Function declaration hoisted first
    var foo;           // var declaration (ignored as foo already declared)
    return foo;        // Returns the function
    foo = 10;          // Never executed
    foo = 11;          // Never executed
}
```

---

### Q15: What will be the output?

```javascript
var foo = 1;

function bar() {
    if (!foo) {
        var foo = 10;
    }
    console.log(foo);
}

bar();
```

**Answer:** `10`

**Explanation:** After hoisting:
```javascript
function bar() {
    var foo;  // Hoisted, initially undefined
    if (!foo) {  // !undefined is true
        foo = 10;
    }
    console.log(foo);  // 10
}
```

---

### Q16: What will be the output?

```javascript
let a = 1;

function test() {
    let b = 2;
    
    if (true) {
        let c = 3;
        console.log(a, b, c);
    }
    
    console.log(a, b);
    console.log(c);
}

test();
```

**Answer:**
```
1 2 3
1 2
ReferenceError: c is not defined
```

**Explanation:** `c` is block-scoped to the if statement and cannot be accessed outside it.

---

### Q17: What will be the output?

```javascript
const obj = {
    a: function() {
        console.log(this.b());
    },
    b: () => {
        console.log(this);
        return "Hello";
    }
};

obj.a();
```

**Answer:** 
```
[Window object or undefined in strict mode]
TypeError: this.b is not a function
```

**Explanation:** Arrow functions don't have their own `this`. The arrow function `b` has `this` pointing to the outer scope (likely window/global), not `obj`.

---

### Q18: What will be the output?

```javascript
console.log(x);
console.log(y);

var x = 1;
let y = 2;

function test() {
    var x = 3;
    let y = 4;
    
    console.log(x);
    console.log(y);
}

test();
```

**Answer:**
```
undefined
ReferenceError: Cannot access 'y' before initialization
```

**Explanation:** The `let y` in TDZ causes an error before `test()` is even called.

---

### Q19: What will be the output?

```javascript
var length = 10;

function fn() {
    console.log(this.length);
}

const obj = {
    length: 5,
    method: function(fn) {
        fn();
        arguments[0]();
    }
};

obj.method(fn, 1, 2, 3);
```

**Answer:**
```
10 (or undefined in strict mode)
4
```

**Explanation:** 
- `fn()` calls function in global context (global `length` = 10)
- `arguments[0]()` calls function with `this` as `arguments` object, which has length 4

---

### Q20: What will be the output?

```javascript
function createFunctions() {
    var result = [];
    
    for (var i = 0; i < 3; i++) {
        result[i] = function() {
            return i;
        };
    }
    
    return result;
}

const functions = createFunctions();
console.log(functions[0]());
console.log(functions[1]());
console.log(functions[2]());
```

**Answer:**
```
3
3
3
```

**Explanation:** All functions share the same `i` due to `var` having function scope. When executed, `i` is 3.

**Fix with let:**
```javascript
for (let i = 0; i < 3; i++) {
    result[i] = function() {
        return i;
    };
}
// Would output: 0, 1, 2
```

---

## Edge Cases

### Edge Case 1: Class Declarations in TDZ

```javascript
console.log(MyClass);  // ReferenceError

class MyClass {}
```

**Explanation:** Class declarations are also subject to TDZ like `let` and `const`.

---

### Edge Case 2: Default Parameters and TDZ

```javascript
function test(a = b, b = 1) {
    console.log(a, b);
}

test();  // ReferenceError: Cannot access 'b' before initialization
```

**Explanation:** Default parameters are evaluated left-to-right. `b` is in TDZ when `a` tries to use it.

---

### Edge Case 3: Hoisting in Nested Scopes

```javascript
var x = 'outer';

function test() {
    console.log(x);
    
    if (false) {
        var x = 'inner';
    }
}

test();  // undefined
```

**Explanation:** Even though the `if` block never executes, `var x` is hoisted to function scope.

---

### Edge Case 4: Function Declaration vs Expression Priority

```javascript
console.log(typeof foo);

var foo = 'variable';

function foo() {
    return 'function';
}

console.log(typeof foo);
```

**Answer:**
```
function
string
```

**Explanation:** Function declarations are hoisted before variable declarations. Later, the assignment overwrites it.

---

### Edge Case 5: TDZ in For Loop

```javascript
for (let i = (i = 0); i < 3; i++) {
    console.log(i);
}
```

**Answer:** `ReferenceError: Cannot access 'i' before initialization`

**Explanation:** In the initialization `let i = (i = 0)`, the right side `i` is accessed while still in TDZ.

---

### Edge Case 6: typeof in TDZ

```javascript
console.log(typeof x);  // undefined (no error for undeclared)

let x;

console.log(typeof y);  // ReferenceError
let y;
```

**Explanation:** The second `typeof y` is in the same scope where `y` is declared with `let`, causing TDZ error.

---

### Edge Case 7: Switch Case TDZ

```javascript
switch (x) {
    case 0:
        let foo = 1;
        break;
    case 1:
        let foo = 2;  // SyntaxError: Identifier 'foo' has already been declared
        break;
}
```

**Explanation:** All `case` clauses share the same block scope. Cannot redeclare `let` variables.

**Fix:**
```javascript
switch (x) {
    case 0: {
        let foo = 1;
        break;
    }
    case 1: {
        let foo = 2;  // OK - different block scope
        break;
    }
}
```

---

### Edge Case 8: Destructuring and Hoisting

```javascript
console.log(a);  // undefined
var [a, b] = [1, 2];

console.log(c);  // ReferenceError
let [c, d] = [3, 4];
```

**Explanation:** Destructuring follows the same hoisting rules as regular declarations.

---

### Edge Case 9: Function Name in Expression

```javascript
var foo = function bar() {
    console.log(typeof bar);
};

foo();  // "function"
console.log(typeof bar);  // "undefined" (bar is not defined in outer scope)
```

**Explanation:** Named function expressions create a binding only within their own scope.

---

### Edge Case 10: Hoisting with eval (Anti-pattern)

```javascript
console.log(x);  // undefined with var, ReferenceError with let

eval('var x = 10;');  // or eval('let x = 10;')

console.log(x);
```

**Explanation:** `eval` with `var` adds to the current scope, but `let`/`const` in eval create their own scope.

---

## Common Interview Traps

### Trap 1: Async/Await and Hoisting

```javascript
async function test() {
    console.log(result);
    const result = await Promise.resolve(42);
}

test();  // ReferenceError: Cannot access 'result' before initialization
```

**Explanation:** `const` with `await` still follows TDZ rules.

---

### Trap 2: Multiple var vs let Declarations

```javascript
console.log(a);  // undefined
var a = 1;
var a = 2;
console.log(a);  // 2

// But this throws error before execution:
let b = 1;
let b = 2;  // SyntaxError
```

---

### Trap 3: Hoisting in Arrow Functions

```javascript
const arrowFunc = () => {
    console.log(arguments);  // ReferenceError
};

arrowFunc(1, 2, 3);
```

**Explanation:** Arrow functions don't have their own `arguments` object.

---

### Trap 4: Block Scope Confusion

```javascript
if (true) {
    function foo() { return 1; }
}

console.log(typeof foo);
```

**Answer:** Depends on mode
- **Strict mode:** `undefined` (function is block-scoped)
- **Non-strict mode:** `function` (hoisted to function/global scope)

---

### Trap 5: Import Hoisting

```javascript
console.log(myModule);  // ReferenceError

import myModule from './module.js';
```

**Explanation:** Imports are hoisted but are in TDZ before their declaration. However, they're processed before any code runs, so in practice, you can use imports anywhere in the module.

---

## Quick Reference Table

| Feature | var | let | const | function | class |
|---------|-----|-----|-------|----------|-------|
| Hoisted? | Yes | Yes | Yes | Yes | Yes |
| Initialized? | undefined | No (TDZ) | No (TDZ) | Yes (fully) | No (TDZ) |
| Re-declaration? | Yes | No | No | Yes | No |
| Scope | Function | Block | Block | Function/Block* | Block |
| TDZ? | No | Yes | Yes | No | Yes |

*Function declarations have complex scoping rules in blocks (differs in strict/non-strict mode)

---

## Best Practices

1. **Always declare variables at the top of their scope** to make hoisting behavior explicit
2. **Prefer `const` by default**, use `let` when reassignment is needed, avoid `var`
3. **Declare functions before using them** for better code readability
4. **Use function declarations for top-level functions** and expressions for callbacks
5. **Be aware of block scope** when using `let` and `const` in loops and conditionals
6. **Use strict mode** (`'use strict';`) to catch more potential errors
7. **Avoid accessing variables before declaration** even though possible with `var`

---

## Summary

**Hoisting** is JavaScript's behavior of moving declarations to the top of their scope:
- `var`: Hoisted and initialized with `undefined`
- `let`/`const`: Hoisted but not initialized (TDZ)
- Functions: Declarations fully hoisted, expressions treated as variables

**Temporal Dead Zone (TDZ)** is the period where variables exist but cannot be accessed:
- Applies to `let`, `const`, and `class`
- Prevents usage before declaration
- Helps catch programming errors
- Causes `ReferenceError` when violated

Understanding these concepts is crucial for:
- Avoiding subtle bugs
- Writing predictable code
- Acing JavaScript interviews
- Understanding scope and closures

---

## Additional Resources

- [MDN: Hoisting](https://developer.mozilla.org/en-US/docs/Glossary/Hoisting)
- [MDN: let](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/let)
- [MDN: const](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/const)
- [MDN: var](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/var)
- [ES6 Spec: Let and Const Declarations](https://tc39.es/ecma262/#sec-let-and-const-declarations)

---

**Happy Learning! 🚀**
