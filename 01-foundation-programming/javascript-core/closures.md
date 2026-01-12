# Closures

## Learning Objectives
- Understand closure concept
- Learn lexical scoping
- Master closure patterns
- Identify closure use cases

## Topics

### 1. What are Closures?
- Function inside function
- Access to outer scope
- Memory implications

### 2. Lexical Scoping
- Variable lookup chain
- Outer scope access
- Scope chain mechanism

### 3. Closure Patterns
- Factory functions
- Private variables
- Callbacks and handlers

### 4. Common Closure Problems
- Variable sharing in loops
- Memory leaks
- Unintended references

## Key Concepts
- **Closure**: Function + its lexical environment
- **Lexical Scope**: Inner scopes access outer variables
- **Memory**: Closed-over variables persist

## Code Examples

### Simple Closure
```javascript
function outer() {
  let count = 0;
  return function inner() {
    return ++count;
  };
}

const counter = outer();
console.log(counter()); // 1
console.log(counter()); // 2
```

### Module Pattern
```javascript
const calculator = (() => {
  let value = 0;
  return {
    add: (n) => (value += n),
    get: () => value
  };
})();
```

## Practice Tasks
- [ ] Create a module with private variables
- [ ] Identify closures in your code
- [ ] Fix closure bugs in loops
- [ ] Implement factory functions

## Interview Tips
- Explain closure formation
- Discuss memory implications
- Solve closure-based problems
- Know when to use closures

## Resources
- MDN: Closures
- JavaScript.info: Closures
