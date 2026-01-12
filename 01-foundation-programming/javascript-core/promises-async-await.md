# Promises & Async/Await

## Learning Objectives
- Master Promise API
- Learn async/await syntax
- Handle errors properly
- Avoid common pitfalls

## Topics

### 1. Promises Basics
- Promise states: pending, fulfilled, rejected
- Creating promises
- .then() and .catch()
- Promise chaining

### 2. Promise API Methods
- Promise.all()
- Promise.race()
- Promise.allSettled()
- Promise.any()

### 3. Async/Await
- Async function syntax
- Await operator
- Error handling with try/catch
- Async best practices

### 4. Error Handling
- Promise rejection
- Unhandled rejections
- Error propagation
- try/catch patterns

## Code Examples

### Promise Creation
```javascript
const promise = new Promise((resolve, reject) => {
  if (success) {
    resolve(value);
  } else {
    reject(error);
  }
});
```

### Async/Await Pattern
```javascript
async function fetchData() {
  try {
    const response = await fetch(url);
    const data = await response.json();
    return data;
  } catch (error) {
    console.error('Error:', error);
  }
}
```

### Promise.all()
```javascript
Promise.all([promise1, promise2, promise3])
  .then(values => console.log(values))
  .catch(error => console.error(error));
```

## Common Pitfalls
- Not returning promises in .then()
- Forgetting to await async operations
- Improper error handling
- Mixing callbacks with promises

## Practice Tasks
- [ ] Convert callback code to promises
- [ ] Use async/await in real scenarios
- [ ] Handle multiple async operations
- [ ] Implement proper error handling
- [ ] Use Promise.all() for parallel operations

## Interview Tips
- Explain Promise states and transitions
- Compare callbacks vs promises vs async/await
- Handle race conditions
- Solve async sequencing problems

## Resources
- MDN: Promise
- MDN: async function
- JavaScript.info: Promises
