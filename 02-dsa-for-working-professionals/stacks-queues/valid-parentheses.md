# Stack & Queue Fundamentals

## Valid Parentheses

### Problem
Check if parentheses are balanced and properly closed.

### Solution

```javascript
// Time: O(n), Space: O(n)
const isValid = (s) => {
  const stack = [];
  const pairs = {
    ')': '(',
    '}': '{',
    ']': '['
  };
  
  for (let char of s) {
    if (char === '(' || char === '{' || char === '[') {
      stack.push(char);
    } else {
      if (stack.length === 0 || stack.pop() !== pairs[char]) {
        return false;
      }
    }
  }
  
  return stack.length === 0;
};

// Test
console.log(isValid('()')); // true
console.log(isValid('()[]{}')); // true
console.log(isValid('([)]')); // false
```

---

## Stack Implementation

```javascript
class Stack {
  constructor() {
    this.items = [];
  }
  
  push(element) {
    this.items.push(element);
  }
  
  pop() {
    return this.items.pop();
  }
  
  peek() {
    return this.items[this.items.length - 1];
  }
  
  isEmpty() {
    return this.items.length === 0;
  }
  
  size() {
    return this.items.length;
  }
  
  clear() {
    this.items = [];
  }
}
```

---

## Queue Implementation

```javascript
class Queue {
  constructor() {
    this.items = [];
  }
  
  enqueue(element) {
    this.items.push(element);
  }
  
  dequeue() {
    return this.items.shift();
  }
  
  front() {
    return this.items[0];
  }
  
  isEmpty() {
    return this.items.length === 0;
  }
  
  size() {
    return this.items.length;
  }
}
```

---

## Stack Problems

### Reverse String
```javascript
const reverseString = (s) => {
  const stack = [];
  for (let char of s) {
    stack.push(char);
  }
  
  let result = '';
  while (!stack.isEmpty()) {
    result += stack.pop();
  }
  return result;
};
```

### Next Greater Element
```javascript
const nextGreaterElement = (nums) => {
  const result = new Array(nums.length).fill(-1);
  const stack = [];
  
  for (let i = nums.length - 1; i >= 0; i--) {
    while (!stack.isEmpty() && stack[stack.length - 1] <= nums[i]) {
      stack.pop();
    }
    
    if (stack.length > 0) {
      result[i] = stack[stack.length - 1];
    }
    
    stack.push(nums[i]);
  }
  
  return result;
};
```

---

## Queue Problems

### Implement Circular Queue
```javascript
class CircularQueue {
  constructor(k) {
    this.queue = new Array(k);
    this.size = k;
    this.front = -1;
    this.rear = -1;
  }
  
  enqueue(value) {
    if (this.isFull()) return false;
    
    if (this.front === -1) this.front = 0;
    this.rear = (this.rear + 1) % this.size;
    this.queue[this.rear] = value;
    return true;
  }
  
  dequeue() {
    if (this.isEmpty()) return false;
    
    if (this.front === this.rear) {
      this.front = -1;
      this.rear = -1;
      return true;
    }
    
    this.front = (this.front + 1) % this.size;
    return true;
  }
  
  isEmpty() {
    return this.front === -1;
  }
  
  isFull() {
    return (this.rear + 1) % this.size === this.front;
  }
}
```

## Practice Tasks
- [ ] Implement Stack and Queue
- [ ] Valid parentheses
- [ ] Decode string
- [ ] Daily temperatures
- [ ] Implement LRU cache

## Resources
- LeetCode: Stack/Queue tags
- InterviewBit: Stack section
