# Memory Leaks & Garbage Collection

## Learning Objectives
- Understand garbage collection
- Identify common memory leaks
- Learn memory profiling
- Write memory-efficient code

## Topics

### 1. Memory Management
- Stack vs Heap
- Garbage collection basics
- Mark and sweep algorithm
- Reference counting

### 2. Common Memory Leak Patterns
- Global variables
- Detached DOM nodes
- Event listener callbacks
- Circular references
- Closures holding references

### 3. Memory Profiling
- DevTools memory tab
- Heap snapshots
- Timeline recording
- Leak detection

### 4. Best Practices
- Cleaning up listeners
- Removing references
- Avoiding global scope
- Proper callback management

## Code Examples

### Memory Leak - Event Listener
```javascript
// ❌ Memory leak
const button = document.getElementById('btn');
button.addEventListener('click', () => {
  console.log('clicked');
});
// Listener never removed

// ✅ Solution
function handleClick() {
  console.log('clicked');
}
button.addEventListener('click', handleClick);
button.removeEventListener('click', handleClick);
```

### Memory Leak - Global Variables
```javascript
// ❌ Memory leak
function createUser() {
  userData = { name: 'John' }; // Global!
}

// ✅ Solution
function createUser() {
  let userData = { name: 'John' }; // Local
}
```

### Memory Leak - Circular Reference
```javascript
// ❌ Memory leak
const user = {};
user.self = user; // Circular reference

// ✅ Solution
const user = {};
user.self = null; // Clean up

// or use WeakMap for internal references
const internalMap = new WeakMap();
```

## Closure Memory
```javascript
// ⚠️ Closure holds reference
function setupListener(largeData) {
  const button = document.getElementById('btn');
  button.addEventListener('click', () => {
    console.log(largeData); // Keeps largeData in memory!
  });
}

// ✅ Solution
function setupListener(largeData) {
  const button = document.getElementById('btn');
  const handler = () => {
    console.log('clicked');
  };
  button.addEventListener('click', handler);
  button.addEventListener('cleanup', () => {
    button.removeEventListener('click', handler);
  });
}
```

## Practice Tasks
- [ ] Use DevTools memory profiler
- [ ] Take heap snapshots
- [ ] Identify memory leaks in code
- [ ] Clean up event listeners
- [ ] Use WeakMap/WeakSet appropriately

## Interview Tips
- Explain garbage collection
- Identify potential memory leaks
- Discuss memory profiling tools
- Know how to prevent common patterns
- Understand closure memory impact

## Debugging
1. Open DevTools → Memory tab
2. Take heap snapshot
3. Record allocation timeline
4. Analyze retaining objects
5. Compare snapshots over time

## Resources
- Chrome DevTools Memory
- MDN: Memory Management
- JavaScript.info: Garbage Collection
