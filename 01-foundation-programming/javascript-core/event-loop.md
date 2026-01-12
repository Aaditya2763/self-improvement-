# Event Loop

## Learning Objectives
- Understand JavaScript execution model
- Master event loop mechanics
- Learn about macrotasks and microtasks
- Predict code execution order

## Topics

### 1. JavaScript Engine
- Single-threaded nature
- Call stack
- Memory heap
- Execution context

### 2. Event Loop
- Event loop phases
- Call stack mechanics
- Task queues
- Execution order

### 3. Macrotasks vs Microtasks
- Macrotasks: setTimeout, setInterval, I/O
- Microtasks: Promises, MutationObserver
- Priority and execution order

### 4. Asynchronous Operations
- Web APIs
- Callbacks
- Promise handling
- Timing guarantees

## Key Concepts
- **Call Stack**: Function execution
- **Event Loop**: Coordinating execution
- **Microtask Queue**: Promise handlers
- **Macrotask Queue**: Timers, events

## Code Examples

### Event Loop Demonstration
```javascript
console.log('1'); // Synchronous

setTimeout(() => console.log('2'), 0); // Macrotask

Promise.resolve()
  .then(() => console.log('3')); // Microtask

console.log('4'); // Synchronous

// Output: 1, 4, 3, 2
```

### Microtask vs Macrotask
```javascript
console.log('Start');

setTimeout(() => console.log('Macrotask'), 0);

Promise.resolve()
  .then(() => console.log('Microtask 1'))
  .then(() => console.log('Microtask 2'));

console.log('End');

// Order: Start, End, Microtask 1, Microtask 2, Macrotask
```

## Practice Tasks
- [ ] Predict code execution order
- [ ] Understand setTimeout behavior
- [ ] Work with Promise resolution
- [ ] Debug async timing issues
- [ ] Understand queueMicrotask()

## Interview Tips
- Draw event loop diagram
- Explain call stack behavior
- Predict execution with multiple async operations
- Understand why setTimeout(..., 0) is not instant
- Explain difference between microtasks and macrotasks

## Visualization
```
Call Stack
    ↑
    |
Event Loop ← Microtask Queue
    |           ↑
    ↓       (Promises)
Macrotask Queue
```

## Resources
- Jake Archibald: In The Loop
- MDN: Concurrency Model
- JavaScript.info: Event Loop
