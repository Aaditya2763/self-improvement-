# Event Loop - Complete Interview Guide

## Table of Contents
1. [What is Event Loop?](#what-is-event-loop)
2. [JavaScript Execution Model](#javascript-execution-model)
3. [Basic Interview Questions](#basic-interview-questions)
4. [Intermediate Interview Questions](#intermediate-interview-questions)
5. [Advanced Interview Questions](#advanced-interview-questions)
6. [Microtasks vs Macrotasks](#microtasks-vs-macrotasks)
7. [Common Patterns](#common-patterns)
8. [Common Interview Traps](#common-interview-traps)

---

## What is Event Loop?

The **Event Loop** is the mechanism that allows JavaScript to perform asynchronous operations despite being single-threaded. It continuously checks if there are any tasks to execute and manages the execution order.

### Key Points:
- JavaScript is **single-threaded**
- The **event loop** handles asynchronous operations
- Browser APIs (setTimeout, fetch, etc.) are separate from JS engine
- Tasks are queued and executed in a specific order
- **Microtasks** execute before **Macrotasks**

### Why Event Loop Matters:
- Understanding asynchronous JavaScript
- Debugging timing issues
- Performance optimization
- Predicting code execution order
- Working with Promises and async/await

---

## JavaScript Execution Model

### Three Main Components

1. **Call Stack**: Executes synchronous code
2. **Event Loop**: Coordinates task execution
3. **Task Queues**: Hold pending tasks
   - **Microtask Queue**: Promises, MutationObserver, queueMicrotask
   - **Macrotask Queue**: setTimeout, setInterval, setImmediate, I/O

### Visual Flow

```
┌─────────────────────────────────────────┐
│         JAVASCRIPT ENGINE               │
│  ┌───────────────────────────────────┐  │
│  │      CALL STACK (Sync Code)       │  │
│  └───────────────────────────────────┘  │
│                    ↑                      │
│                    │                      │
│  ┌─────────────────┴────────────────┐   │
│  │      EVENT LOOP                  │   │
│  │  (Monitors & Coordinates)        │   │
│  └─────────────────┬────────────────┘   │
└─────────────────────┼────────────────────┘
        ↑              ↓
        │     ┌────────────────────┐
        │     │ MICROTASK QUEUE    │
        │     │ (High Priority)    │
        │     └────────────────────┘
        │              ↓
        │     ┌────────────────────┐
        │     │ MACROTASK QUEUE    │
        │     │ (Low Priority)     │
        │     └────────────────────┘
        │
        └─── Browser APIs (setTimeout, fetch, etc.)
```

### Execution Order

```
1. Execute all SYNCHRONOUS code (Call Stack)
2. Call Stack is empty? Check Microtask Queue
3. Execute ALL microtasks
4. Check Macrotask Queue
5. Execute ONE macrotask
6. Check Microtask Queue again (repeat from step 3)
```

---

## Basic Interview Questions

### Q1: What will be the output?

```javascript
console.log('1');

setTimeout(() => {
  console.log('2');
}, 0);

console.log('3');
```

**Answer:**
```
1
3
2
```

**Explanation:** 
- `console.log('1')` is synchronous, executes immediately
- `setTimeout` is asynchronous (macrotask), goes to macrotask queue
- `console.log('3')` is synchronous, executes immediately
- Call stack is empty, event loop checks microtask queue (empty)
- Event loop checks macrotask queue, finds `setTimeout`, executes it

---

### Q2: What will be the output?

```javascript
console.log('Start');

Promise.resolve()
  .then(() => console.log('Promise'));

console.log('End');
```

**Answer:**
```
Start
End
Promise
```

**Explanation:**
- `console.log('Start')` - synchronous, executes first
- `Promise.then()` - asynchronous (microtask), queued
- `console.log('End')` - synchronous, executes second
- Call stack empty, event loop runs microtask queue
- Promise microtask executes

---

### Q3: Microtasks vs Macrotasks - which executes first?

**Answer:** Microtasks execute BEFORE macrotasks.

```javascript
console.log('Sync');

setTimeout(() => console.log('Macrotask'), 0);

Promise.resolve().then(() => console.log('Microtask'));

console.log('Sync 2');

// Output:
// Sync
// Sync 2
// Microtask
// Macrotask
```

---

### Q4: What will be the output?

```javascript
console.log('1');

setTimeout(() => console.log('2'), 0);
setTimeout(() => console.log('3'), 0);

console.log('4');
```

**Answer:**
```
1
4
2
3
```

**Explanation:** Both `setTimeout` calls go to the macrotask queue. They execute one at a time, after all synchronous code and microtasks complete.

---

### Q5: What is the event loop?

**Answer:** The event loop is a loop that continuously checks if the call stack is empty and if there are any tasks in the task queues. If the call stack is empty, it moves tasks from the queues to the call stack for execution.

```javascript
// Simplified event loop pseudocode
while (eventLoop.waitForTask()) {
  const macrotask = macrotaskQueue.pop();
  executeMacrotask(macrotask);
  
  while (microtaskQueue.hasTasks()) {
    const microtask = microtaskQueue.pop();
    executeMicrotask(microtask);
  }
}
```

---

### Q6: Why setTimeout with 0 doesn't execute immediately?

**Answer:** Because `setTimeout` is a macrotask. Even with 0ms delay, it's queued and can only execute after:
1. The call stack is empty (all synchronous code finishes)
2. All microtasks are processed
3. The event loop picks the macrotask

```javascript
console.log('Before');
setTimeout(() => console.log('Timeout'), 0);
console.log('After');

// Output: Before, After, Timeout
```

---

## Intermediate Interview Questions

### Q7: What will be the output?

```javascript
console.log('Script start');

setTimeout(() => console.log('setTimeout'), 0);

Promise.resolve()
  .then(() => console.log('Promise 1'))
  .then(() => console.log('Promise 2'));

console.log('Script end');
```

**Answer:**
```
Script start
Script end
Promise 1
Promise 2
setTimeout
```

**Explanation:**
1. Synchronous: 'Script start', 'Script end'
2. Microtasks: Promise chain executes in order
3. Macrotask: setTimeout executes last

---

### Q8: What will be the output?

```javascript
console.log('1');

setTimeout(() => {
  console.log('2');
  Promise.resolve().then(() => console.log('3'));
}, 0);

Promise.resolve()
  .then(() => {
    console.log('4');
    setTimeout(() => console.log('5'), 0);
  });

console.log('6');
```

**Answer:**
```
1
6
4
2
3
5
```

**Explanation:**
1. Sync: '1', '6'
2. Microtask: '4' (with setTimeout queued)
3. Macrotask: '2'
4. Microtask: '3'
5. Macrotask: '5'

---

### Q9: What will be the output?

```javascript
const promise = new Promise((resolve) => {
  console.log('1');
  resolve('2');
  console.log('3');
});

promise.then((val) => console.log(val));

console.log('4');
```

**Answer:**
```
1
3
4
2
```

**Explanation:**
- Constructor code is synchronous
- `.then()` is a microtask
- Execution: sync code ('1', '3', '4'), then microtask ('2')

---

### Q10: What will be the output?

```javascript
setTimeout(() => {
  console.log('setTimeout 1');
  Promise.resolve().then(() => console.log('Promise 1'));
}, 0);

setTimeout(() => {
  console.log('setTimeout 2');
}, 0);

Promise.resolve()
  .then(() => {
    console.log('Promise 2');
    setTimeout(() => console.log('setTimeout 3'), 0);
  })
  .then(() => console.log('Promise 3'));
```

**Answer:**
```
Promise 2
Promise 3
setTimeout 1
Promise 1
setTimeout 2
setTimeout 3
```

**Explanation:**
1. Microtasks: Promise 2, Promise 3
2. Macrotask: setTimeout 1
3. Microtask: Promise 1
4. Macrotask: setTimeout 2
5. Macrotask: setTimeout 3

---

### Q11: What's the difference between queueMicrotask and Promise.then()?

```javascript
queueMicrotask(() => console.log('queueMicrotask'));

Promise.resolve().then(() => console.log('Promise'));

console.log('Sync');

// Output: Sync, queueMicrotask, Promise
```

**Answer:** Both are microtasks and execute in the same queue, in the order they're added. The difference is:
- `queueMicrotask()` - explicit microtask API
- `Promise.then()` - microtask via Promise
- Both execute before macrotasks

---

### Q12: What will be the output?

```javascript
async function asyncFunc() {
  console.log('1');
  await Promise.resolve();
  console.log('2');
}

asyncFunc();

console.log('3');
```

**Answer:**
```
1
3
2
```

**Explanation:**
- Function call is synchronous: '1'
- Code after `await` is a microtask: '2'
- Synchronous: '3'
- Microtask: '2'

---

### Q13: Fetch vs setTimeout - execution order?

```javascript
console.log('Start');

fetch('https://api.example.com')
  .then(() => console.log('Fetch'));

setTimeout(() => console.log('Timeout'), 0);

console.log('End');
```

**Answer:**
```
Start
End
Fetch
Timeout
```

**Explanation:** Fetch returns a Promise, which is a microtask. setTimeout is a macrotask. Microtasks always execute before macrotasks.

---

## Advanced Interview Questions

### Q14: What will be the output?

```javascript
console.log('1');

Promise.resolve()
  .then(() => {
    console.log('2');
    return Promise.resolve();
  })
  .then(() => console.log('3'));

console.log('4');
```

**Answer:**
```
1
4
2
3
```

**Explanation:**
- Sync: '1', '4'
- Microtask: '2'
- Microtask: '3' (queued after the returned promise resolves)

---

### Q15: Advanced Microtask Ordering

```javascript
console.log('Start');

const p1 = Promise.resolve().then(() => console.log('p1'));
const p2 = Promise.resolve().then(() => console.log('p2'));

p1.then(() => console.log('p1-then'));
p2.then(() => console.log('p2-then'));

console.log('End');
```

**Answer:**
```
Start
End
p1
p2
p1-then
p2-then
```

**Explanation:**
- Microtask queue order: p1, p2, p1-then, p2-then
- First level promises execute, THEN their subsequent .then() callbacks queue

---

### Q16: MutationObserver as Microtask

```javascript
console.log('1');

const observer = new MutationObserver(() => {
  console.log('Mutation Observer');
});

observer.observe(document.body, { attributes: true });

document.body.setAttribute('test', 'value');

setTimeout(() => console.log('Timeout'), 0);

console.log('2');

// Output: 1, 2, Mutation Observer, Timeout
```

**Explanation:** MutationObserver callbacks are microtasks, executing before setTimeout (macrotask).

---

### Q17: What will be the output?

```javascript
const promise = Promise.resolve();

promise.then(() => {
  console.log('1');
  return 'value';
});

promise.then((val) => {
  console.log('2');
});

promise.then(() => {
  console.log('3');
});
```

**Answer:**
```
1
2
3
```

**Explanation:** All `.then()` handlers are added to the same promise and execute in order as separate microtasks.

---

### Q18: Nested Event Loop Behavior

```javascript
console.log('1');

setTimeout(() => {
  console.log('2');
  Promise.resolve().then(() => {
    console.log('3');
    setTimeout(() => console.log('4'), 0);
  });
}, 0);

setTimeout(() => {
  console.log('5');
}, 0);
```

**Answer:**
```
1
2
5
3
4
```

**Explanation:**
1. Sync: '1'
2. Macrotask: '2'
3. Macrotask: '5'
4. Microtask: '3'
5. Macrotask: '4'

---

### Q19: RequestAnimationFrame Timing

```javascript
console.log('1');

setTimeout(() => console.log('Timeout'), 0);

requestAnimationFrame(() => console.log('RAF'));

Promise.resolve().then(() => console.log('Promise'));

console.log('2');

// Output: 1, 2, Promise, RAF, Timeout
```

**Explanation:** 
- Synchronous: '1', '2'
- Microtask: 'Promise'
- Before repaint: 'RAF'
- Macrotask: 'Timeout'

Note: RAF timing can vary based on browser and rendering cycle.

---

### Q20: Complex Mixed Scenario

```javascript
console.log('Script start');

setTimeout(() => {
  console.log('setTimeout 1');
  
  Promise.resolve()
    .then(() => console.log('Promise in setTimeout 1'));
  
  setTimeout(() => console.log('setTimeout 2'), 0);
}, 0);

Promise.resolve()
  .then(() => {
    console.log('Promise 1');
    
    setTimeout(() => console.log('setTimeout 3'), 0);
  })
  .then(() => {
    console.log('Promise 1.1');
  });

console.log('Script end');
```

**Answer:**
```
Script start
Script end
Promise 1
Promise 1.1
setTimeout 1
Promise in setTimeout 1
setTimeout 2
setTimeout 3
```

**Explanation:**
1. Sync code
2. All microtasks (Promises)
3. First macrotask (setTimeout 1)
4. Its microtask
5. Remaining macrotasks in order

---

### Q21: Async/Await Under the Hood

```javascript
async function test() {
  console.log('1');
  await new Promise(resolve => setTimeout(resolve, 0));
  console.log('2');
}

test();
console.log('3');
```

**Answer:**
```
1
3
2
```

**Explanation:** `await` creates a microtask boundary. Code after `await` executes as a microtask, so it runs after all synchronous code but before macrotasks.

---

## Microtasks vs Macrotasks

### Complete List

**Microtasks (High Priority)**
- Promises (`.then()`, `.catch()`, `.finally()`)
- `queueMicrotask()`
- `MutationObserver`
- `process.nextTick()` (Node.js)

**Macrotasks (Low Priority)**
- `setTimeout()`
- `setInterval()`
- `setImmediate()` (Node.js)
- `requestAnimationFrame()`
- I/O operations
- Event handlers
- UI rendering

### Execution Table

| Phase | What Executes | Quantity |
|-------|---------------|----------|
| Call Stack | Synchronous code | Until empty |
| Microtask Queue | ALL microtasks | Everything |
| Rendering | Paint/Composite | As needed |
| Macrotask Queue | ONE task | One per cycle |

---

## Common Patterns

### Pattern 1: Promise Chain Execution

```javascript
Promise.resolve()
  .then(() => 'Task 1')
  .then(result => console.log(result) || 'Task 2')
  .then(() => console.log('Task 3'));

// All execute in microtask phase, in order
```

### Pattern 2: Debouncing with setTimeout

```javascript
function debounce(fn, delay) {
  let timeoutId;
  return function(...args) {
    clearTimeout(timeoutId);
    timeoutId = setTimeout(() => fn(...args), delay);
  };
}
```

### Pattern 3: Batch Operations

```javascript
const batch = [];

function addToBatch(item) {
  batch.push(item);
  
  if (batch.length === 1) {
    // Schedule batch processing as macrotask
    setTimeout(() => {
      processBatch(batch);
      batch.length = 0;
    }, 0);
  }
}
```

### Pattern 4: Ensure Async Behavior

```javascript
function ensureAsync(callback) {
  Promise.resolve().then(callback); // Ensure microtask
  // or
  setTimeout(callback, 0); // Ensure macrotask
}
```

---

## Common Interview Traps

### Trap 1: setTimeout(fn, 0) Doesn't Mean Instant

```javascript
// ❌ Wrong assumption
setTimeout(() => console.log('instant'), 0);
// Takes time to execute!

// ✅ For actual async boundary
Promise.resolve().then(() => console.log('faster')); 
// Still async but faster (microtask)
```

### Trap 2: Forgetting Microtask Precedence

```javascript
// ❌ Wrong prediction
setTimeout(() => console.log('1'), 0);
Promise.resolve().then(() => console.log('2'));
// Expected: 1, 2
// Actual: 2, 1

// ✅ Correct understanding
// Promises (microtask) execute before setTimeout (macrotask)
```

### Trap 3: Returning Promise in setTimeout

```javascript
// ⚠️ Tricky
setTimeout(() => {
  console.log('1');
  
  return Promise.resolve().then(() => console.log('2'));
  // '2' still executes (microtask after macrotask)
}, 0);

// Output: 1, 2
```

### Trap 4: Nested Promise Timing

```javascript
// ⚠️ Easy to get wrong
Promise.resolve()
  .then(() => {
    console.log('1');
    return Promise.resolve();
  })
  .then(() => console.log('2'));

console.log('3');

// Output: 3, 1, 2 (not 3, 1, 2)
```

### Trap 5: Async Function Misunderstanding

```javascript
// ❌ Wrong assumption
const fn = async () => console.log('async');
fn(); // Doesn't execute immediately!

// ✅ Correct
const fn = async () => console.log('async');
fn(); // Queues as microtask
// Still logs after synchronous code
```

### Trap 6: Event Handler Timing

```javascript
// ⚠️ Tricky
button.addEventListener('click', () => {
  console.log('1');
  
  Promise.resolve().then(() => console.log('2'));
  
  setTimeout(() => console.log('3'), 0);
});

// Click → Output: 1, 2, 3
// Event handler is macrotask, so '2' (microtask) runs before '3' (macrotask)
```

---

## Quick Reference

### Execution Order Checklist

```
✓ 1. Execute all SYNCHRONOUS code first
✓ 2. Is call stack empty? Yes → continue
✓ 3. Execute ALL MICROTASKS
    - Promises
    - queueMicrotask
    - MutationObserver
✓ 4. Check if rendering needed
✓ 5. Execute ONE MACROTASK
    - setTimeout
    - setInterval
    - I/O
✓ 6. Back to step 3 (check microtasks again)
```

### Priority Reference

```
Priority 1 (Highest): Synchronous Code
Priority 2: Microtasks (Promises, etc.)
Priority 3: UI Rendering
Priority 4: Macrotasks (setTimeout, etc.)
```

---

## Key Takeaways

1. **JavaScript is single-threaded** but handles async through the event loop
2. **Microtasks always execute before macrotasks** - this is crucial
3. **The event loop continuously monitors** the call stack and task queues
4. **setTimeout(fn, 0) is not instant** - goes through the macrotask queue
5. **Promises are faster** than setTimeout because they're microtasks
6. **All microtasks execute** before any macrotask
7. **Event loop checks microtasks after each macrotask** - important for nested operations
8. **Rendering happens between macrotasks**, not during microtasks

---

## Best Practices

✅ **Do:**
- Understand microtask vs macrotask
- Use Promises for async operations
- Test code to understand execution order
- Visualize the event loop mentally
- Use async/await for clarity
- Batch operations with setTimeout for macrotask

❌ **Don't:**
- Assume setTimeout is instant (it's not!)
- Forget about microtasks
- Use setTimeout for synchronization
- Nest too many async operations
- Block the event loop with long-running sync code
- Forget that Promises are microtasks

---

## Interview Strategy

1. **Draw the event loop diagram** - Shows understanding
2. **Explain synchronous vs asynchronous** - Foundation
3. **Mention microtask vs macrotask** - Key concept
4. **Walk through an example** - Demonstrate knowledge
5. **Discuss common mistakes** - Shows experience
6. **Ask clarifying questions** - "What about...?"

### Common Interview Questions

- "Explain the event loop"
- "Why does setTimeout execute after Promise?"
- "What will this code output?" (prediction)
- "Explain async/await in terms of event loop"
- "What's the difference between microtask and macrotask?"

---

## Browser DevTools Tips

### Monitoring Event Loop

```javascript
// Track execution with timestamps
const log = (msg) => {
  console.log(`${msg} - ${performance.now()}`);
};

// Use Timeline/Performance tab to visualize
```

### In Chrome DevTools
1. Open DevTools → Performance tab
2. Record → Execute code → Stop
3. View the timeline with tasks and microtasks

---

## Resources

- [Jake Archibald: In The Loop (Video)](https://www.youtube.com/watch?v=cCOL7MC4Pl0) ⭐ BEST
- [MDN: Concurrency Model](https://developer.mozilla.org/en-US/docs/Web/JavaScript/EventLoop)
- [JavaScript.info: Event Loop](https://javascript.info/event-loop)
- [Web.dev: Tasks, microtasks, queues and schedules](https://jakearchibald.com/2015/tasks-microtasks-queues-and-schedules/)
- [Loupe: Interactive Event Loop Visualizer](http://latentflip.com/loupe/)

---

## Practice Exercises

1. Predict the output of 10 event loop examples
2. Trace execution step-by-step
3. Identify whether something is microtask or macrotask
4. Explain why certain code executes in a specific order
5. Optimize code using event loop knowledge
6. Debug timing issues using event loop understanding

---

**Master the Event Loop and Understand JavaScript Deeply! 🚀**
