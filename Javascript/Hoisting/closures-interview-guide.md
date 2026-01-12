# Closures - Complete Interview Guide

## Table of Contents
1. [What are Closures?](#what-are-closures)
2. [How Closures Work](#how-closures-work)
3. [Basic Interview Questions](#basic-interview-questions)
4. [Intermediate Interview Questions](#intermediate-interview-questions)
5. [Advanced Interview Questions](#advanced-interview-questions)
6. [Common Patterns](#common-patterns)
7. [Common Interview Traps](#common-interview-traps)

---

## What are Closures?

A **closure** is a function that has access to another function's scope, even after that function has returned. Closures are created every time a function is created.

### Key Points:
- Function inside another function
- Inner function accesses outer function's variables
- Outer function has returned, but variables are still accessible
- Closure = Function + Lexical Environment
- Every function creates a closure

### Why Closures Matter:
- Data privacy and encapsulation
- Creating reusable functions
- Avoiding global variables
- Managing state
- One of the most important concepts in JavaScript

---

## How Closures Work

### Lexical Scoping
JavaScript uses **lexical scoping**, which means a function can access variables from its outer scope based on where it was defined, not where it was called.

```javascript
const globalVar = 'global';

function outer() {
  const outerVar = 'outer';
  
  function inner() {
    const innerVar = 'inner';
    console.log(innerVar);   // 'inner' - own scope
    console.log(outerVar);   // 'outer' - closure
    console.log(globalVar);  // 'global' - global scope
  }
  
  inner();
}

outer();
```

### Memory Persistence
The key feature: variables remain in memory even after the outer function returns.

```javascript
function makeCounter() {
  let count = 0;  // This variable persists!
  
  return function() {
    return ++count;
  };
}

const counter = makeCounter();
console.log(counter()); // 1
console.log(counter()); // 2
console.log(counter()); // 3
```

---

## Basic Interview Questions

### Q1: What will be the output?

```javascript
function greet() {
  const name = 'Alice';
  
  function sayHi() {
    console.log(name);
  }
  
  return sayHi;
}

const func = greet();
func(); // ?
```

**Answer:** `'Alice'`

**Explanation:** The function `sayHi` is returned and assigned to `func`. Even though `greet()` has finished executing, `sayHi` still has access to the `name` variable through closure. This is a classic closure example.

---

### Q2: What is a closure?

**Answer:** A closure is a function that has access to variables from another function's scope. This is possible because of the way JavaScript handles scoping and the fact that functions can return other functions.

```javascript
function outer() {
  const message = 'Hello';
  
  function inner() {
    console.log(message); // Closure: accesses outer's variable
  }
  
  return inner;
}

const closure = outer();
closure(); // 'Hello'
```

---

### Q3: Will this cause a memory leak?

```javascript
function setupListener() {
  const largeData = new Array(1000000);
  
  document.getElementById('btn').addEventListener('click', () => {
    console.log(largeData.length);
  });
}
```

**Answer:** Yes, this can cause a memory leak. The closure holds a reference to `largeData` even though the event listener only needs the length. Solution:

```javascript
function setupListener() {
  const largeData = new Array(1000000);
  const length = largeData.length; // Extract what you need
  
  document.getElementById('btn').addEventListener('click', () => {
    console.log(length);
  });
}
```

---

### Q4: What will be the output?

```javascript
function outer() {
  let x = 10;
  
  function inner() {
    return x;
  }
  
  x = 20;
  return inner;
}

const func = outer();
console.log(func()); // ?
```

**Answer:** `20`

**Explanation:** The closure captures the variable `x` by reference, not by value. When `x` is changed to 20 before returning, the closure accesses the updated value.

---

### Q5: Create a simple counter with closures

```javascript
// Solution
function createCounter() {
  let count = 0;
  
  return {
    increment: () => ++count,
    decrement: () => --count,
    getCount: () => count
  };
}

const counter = createCounter();
console.log(counter.increment()); // 1
console.log(counter.increment()); // 2
console.log(counter.decrement()); // 1
console.log(counter.getCount());  // 1
```

---

## Intermediate Interview Questions

### Q6: What will be the output?

```javascript
const functions = [];

for (var i = 0; i < 3; i++) {
  functions.push(function() {
    return i;
  });
}

console.log(functions[0]()); // ?
console.log(functions[1]()); // ?
console.log(functions[2]()); // ?
```

**Answer:** 
```
3
3
3
```

**Explanation:** All functions share the same variable `i` due to `var` having function scope. When the loop finishes, `i` is 3, and all functions return 3.

**Fix with let:**
```javascript
const functions = [];

for (let i = 0; i < 3; i++) {
  functions.push(function() {
    return i;
  });
}

console.log(functions[0]()); // 0
console.log(functions[1]()); // 1
console.log(functions[2]()); // 2
```

With `let`, each iteration creates a new binding, so each function has its own `i`.

---

### Q7: Implement a function factory

```javascript
function multiplyBy(factor) {
  return function(number) {
    return number * factor;
  };
}

const double = multiplyBy(2);
const triple = multiplyBy(3);

console.log(double(5));  // ?
console.log(triple(5));  // ?
```

**Answer:**
```
10
15
```

**Explanation:** Each call to `multiplyBy()` creates a new closure with a different `factor`. This is a common pattern for creating specialized functions.

---

### Q8: What will be the output?

```javascript
function createMultiplier(x) {
  return function(y) {
    return function(z) {
      return x * y * z;
    };
  };
}

const multiply = createMultiplier(2)(3);
console.log(multiply(4)); // ?
```

**Answer:** `24`

**Explanation:** This demonstrates currying with closures. Each function has access to all outer variables through closures, enabling nested function composition.

---

### Q9: Module Pattern (Private Variables)

```javascript
const calculator = (function() {
  let result = 0; // Private variable
  
  return {
    add: function(x) {
      result += x;
      return result;
    },
    subtract: function(x) {
      result -= x;
      return result;
    },
    getResult: function() {
      return result;
    }
  };
})();

console.log(calculator.add(5));      // ?
console.log(calculator.subtract(2));  // ?
console.log(calculator.result);       // ?
```

**Answer:**
```
5
3
undefined
```

**Explanation:** The `result` variable is private and can only be accessed through the public methods. Direct access returns `undefined`. This is the Module Pattern - a way to create private scope.

---

### Q10: What happens with forEach closure?

```javascript
const arr = [1, 2, 3];
const functions = [];

arr.forEach(function(num) {
  functions.push(function() {
    return num;
  });
});

console.log(functions[0]()); // ?
console.log(functions[1]()); // ?
console.log(functions[2]()); // ?
```

**Answer:**
```
1
2
3
```

**Explanation:** Unlike the `var` loop problem, `forEach` creates a new scope for each iteration. Each callback function has its own `num` variable through closure.

---

### Q11: Event Listener with Closure

```javascript
function setupButtons() {
  for (let i = 1; i <= 3; i++) {
    const button = document.createElement('button');
    button.textContent = `Button ${i}`;
    
    button.addEventListener('click', function() {
      console.log(`Button ${i} clicked`);
    });
    
    document.body.appendChild(button);
  }
}

setupButtons();
// Click Button 1 → logs "Button 1 clicked"
// Click Button 2 → logs "Button 2 clicked"
// Click Button 3 → logs "Button 3 clicked"
```

**Explanation:** The closure captures the loop variable `i` for each button. Since we use `let`, each iteration has its own `i` value.

---

## Advanced Interview Questions

### Q12: Closure with setTimeout

```javascript
for (var i = 0; i < 3; i++) {
  setTimeout(function() {
    console.log(i);
  }, 100);
}
```

**Answer:** Logs `3`, `3`, `3`

**Explanation:** The closure captures the variable `i`, not its value. By the time `setTimeout` executes, the loop is done and `i` is 3.

**Solutions:**

```javascript
// Solution 1: IIFE
for (var i = 0; i < 3; i++) {
  (function(j) {
    setTimeout(function() {
      console.log(j);
    }, 100);
  })(i);
}

// Solution 2: let
for (let i = 0; i < 3; i++) {
  setTimeout(function() {
    console.log(i);
  }, 100);
}

// Solution 3: Extra variable
for (var i = 0; i < 3; i++) {
  const j = i;
  setTimeout(function() {
    console.log(j);
  }, 100);
}
```

---

### Q13: Memoization with Closures

```javascript
function memoize(fn) {
  const cache = {};
  
  return function(...args) {
    const key = JSON.stringify(args);
    
    if (key in cache) {
      console.log('From cache');
      return cache[key];
    }
    
    console.log('Computing');
    const result = fn(...args);
    cache[key] = result;
    return result;
  };
}

const fibonacci = memoize(function(n) {
  if (n <= 1) return n;
  return fibonacci(n - 1) + fibonacci(n - 2);
});

console.log(fibonacci(5)); // Computing
console.log(fibonacci(5)); // From cache
```

**Explanation:** The `cache` object is enclosed in the closure, persisting across calls. This demonstrates performance optimization with closures.

---

### Q14: Closure Scope Chain

```javascript
const a = 'global';

function outer() {
  const a = 'outer';
  
  function middle() {
    const a = 'middle';
    
    function inner() {
      console.log(a);
    }
    
    return inner;
  }
  
  return middle;
}

const func = outer()();
func(); // ?
```

**Answer:** `'middle'`

**Explanation:** JavaScript looks for the variable in the scope chain starting from the innermost scope. It finds `a` in `middle()` before looking further out.

---

### Q15: Partial Application with Closures

```javascript
function curry(fn) {
  const arity = fn.length;
  
  return function curried(...args) {
    if (args.length >= arity) {
      return fn(...args);
    }
    
    return (...nextArgs) => curried(...args, ...nextArgs);
  };
}

const add = (a, b, c) => a + b + c;
const curriedAdd = curry(add);

console.log(curriedAdd(1)(2)(3)); // ?
console.log(curriedAdd(1, 2)(3)); // ?
console.log(curriedAdd(1)(2, 3)); // ?
```

**Answer:** All output `6`

**Explanation:** Currying breaks down a function with multiple parameters into a series of functions that each take a single parameter. Closures enable this by remembering previous arguments.

---

### Q16: WeakMap and Closures

```javascript
const privateData = new WeakMap();

class User {
  constructor(name, age) {
    privateData.set(this, { name, age });
  }
  
  getName() {
    return privateData.get(this).name;
  }
}

const user = new User('Alice', 25);
console.log(user.getName());         // ?
console.log(privateData.get(user));  // ?
```

**Answer:**
```
'Alice'
{ name: 'Alice', age: 25 }
```

**Explanation:** WeakMap allows true private data through closures. Unlike regular closures, when the object is garbage collected, the WeakMap entry is also removed.

---

### Q17: Higher-Order Function Composition

```javascript
const compose = (...fns) => x => fns.reduceRight((v, f) => f(v), x);

const addTwo = x => x + 2;
const multiplyByTwo = x => x * 2;
const square = x => x * x;

const fn = compose(square, multiplyByTwo, addTwo);

console.log(fn(3)); // ?
```

**Answer:** `400`

**Explanation:** Composition applies functions right to left: ((3 + 2) * 2)² = (5 * 2)² = 10² = 100. Wait, let me recalculate: (3 + 2) = 5, (5 * 2) = 10, (10 * 10) = 100. Actually: compose applies right to left, so square(multiplyByTwo(addTwo(3))) = square(multiplyByTwo(5)) = square(10) = 100.

---

### Q18: Closure with Object Methods

```javascript
const obj = {
  value: 42,
  
  getValue: function() {
    return this.value;
  },
  
  getValueArrow: () => {
    return this.value;
  }
};

console.log(obj.getValue());     // ?
console.log(obj.getValueArrow()); // ?
```

**Answer:**
```
42
undefined (or window.value)
```

**Explanation:** Regular functions have their own `this`, while arrow functions use the `this` from their enclosing scope (closure). The arrow function closes over the global `this`.

---

### Q19: Closure Memory Management

```javascript
function createProcessor() {
  const largeArray = new Array(1000000).fill(0);
  
  return {
    processSmall: function(x) {
      return x * 2; // Uses largeArray? No!
    },
    
    processLarge: function() {
      return largeArray.reduce((a, b) => a + b);
    }
  };
}

const processor = createProcessor();
processor.processSmall(5); // ?
```

**Answer:** `10`

**Explanation:** Even though both methods are in the same object, `processSmall` closes over the entire `largeArray`. This keeps it in memory. To optimize, extract only needed data:

```javascript
function createProcessor() {
  const result = process();
  
  return {
    getResult: () => result
  };
}
```

---

### Q20: Closure with Setters/Getters

```javascript
function createUser() {
  let _email = ''; // Private
  
  return {
    setEmail: function(email) {
      if (email.includes('@')) {
        _email = email;
        return true;
      }
      return false;
    },
    
    getEmail: function() {
      return _email;
    }
  };
}

const user = createUser();
console.log(user.setEmail('invalid'));      // ?
console.log(user.setEmail('test@test.com')); // ?
console.log(user.getEmail());                // ?
console.log(user._email);                    // ?
```

**Answer:**
```
false
true
'test@test.com'
undefined
```

**Explanation:** Closures enable data validation and encapsulation. The `_email` variable is truly private and can only be modified through the public API.

---

## Common Patterns

### 1. Module Pattern
```javascript
const Module = (function() {
  const privateVar = 'private';
  
  return {
    publicMethod: () => privateVar
  };
})();
```

### 2. Revealing Module Pattern
```javascript
const Module = (function() {
  const privateVar = 'private';
  
  const privateMethod = () => privateVar;
  
  return {
    public: privateMethod
  };
})();
```

### 3. Factory Function
```javascript
function createItem(name) {
  return {
    getName: () => name,
    setName: (n) => name = n
  };
}
```

### 4. Immediately Invoked Function Expression (IIFE)
```javascript
(function() {
  const temp = 'local';
  console.log(temp);
})();
```

### 5. Currying
```javascript
const add = (a) => (b) => (c) => a + b + c;
add(1)(2)(3); // 6
```

---

## Common Interview Traps

### Trap 1: The Loop Problem
```javascript
// ❌ Problem
for (var i = 0; i < 3; i++) {
  setTimeout(() => console.log(i), 0);
}
// Logs: 3, 3, 3

// ✅ Solution
for (let i = 0; i < 3; i++) {
  setTimeout(() => console.log(i), 0);
}
// Logs: 0, 1, 2
```

### Trap 2: This in Closures
```javascript
// ❌ Problem
const obj = {
  name: 'Object',
  getName: () => this.name
};

// ✅ Solution
const obj = {
  name: 'Object',
  getName: function() { return this.name; }
};
```

### Trap 3: Forgetting Variable Scope
```javascript
// ❌ Problem
function outer() {
  x = 5; // Creates global!
}

// ✅ Solution
function outer() {
  const x = 5; // Proper scope
}
```

### Trap 4: Closure Size
```javascript
// ❌ Memory issue
function setup() {
  const huge = new Array(1000000);
  document.addEventListener('click', () => {
    console.log('clicked');
  });
}

// ✅ Extract what you need
function setup() {
  const huge = new Array(1000000);
  const needed = huge[0];
  document.addEventListener('click', () => {
    console.log(needed);
  });
}
```

### Trap 5: Array Functions in Closures
```javascript
// ❌ Problem
const arr = [1, 2, 3];
arr.forEach(function(val) {
  setTimeout(() => console.log(val), 0); // Works correctly!
});

// Arrow function captures differently
const obj = {
  value: 42,
  test: () => this.value // ❌ Wrong this
};
```

---

## Quick Reference

| Concept | Example | Use Case |
|---------|---------|----------|
| Basic Closure | Inner function accessing outer variables | Data encapsulation |
| Module Pattern | IIFE returning object | Private state |
| Currying | `f(a)(b)(c)` | Partial application |
| Memoization | Caching with closure | Performance |
| Factory | Function returning objects | Creating instances |

---

## Key Takeaways

1. **Every function is a closure** - it has access to its outer scope
2. **Closures persist variables** - variables stay in memory as long as the closure exists
3. **Lexical scoping** - inner functions access outer variables where they're defined
4. **Private data** - use closures to create truly private variables
5. **Memory management** - be careful not to capture unneeded large objects
6. **Common pattern** - for loops with var create shared closure problems
7. **This context** - arrow functions in closures use outer this

---

## Best Practices

✅ **Do:**
- Use closures for data encapsulation
- Understand closure scope chains
- Use let/const to avoid closure pitfalls
- Clean up references in closures
- Use closures for callbacks and event handlers

❌ **Don't:**
- Unintentionally capture large objects
- Expect var to create separate closures in loops
- Confuse `this` in arrow functions
- Ignore memory implications
- Over-complicate with nested closures

---

## Interview Strategy

1. **Explain the concept first** - "A closure is..."
2. **Give a simple example** - Show a basic counter
3. **Discuss use cases** - Module pattern, data privacy
4. **Mention pitfalls** - Loop problems, memory issues
5. **Show solutions** - How to fix common problems
6. **Ask clarifying questions** - Understand expectations

---

## Resources

- [MDN: Closures](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Closures)
- [JavaScript.info: Variable Scope, Closure](https://javascript.info/closure)
- [You Don't Know JS: Scope & Closures](https://github.com/getify/You-Dont-Know-JS)
- [Eloquent JavaScript: Higher-Order Functions](https://eloquentjavascript.net/)

---

**Happy Learning! Master closures and you'll master JavaScript! 🚀**
