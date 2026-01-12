# Time & Space Complexity

## Learning Objectives
- Understand Big O notation
- Analyze algorithm efficiency
- Calculate time complexity
- Determine space complexity

## Topics

### 1. Big O Notation
- O(1): Constant time
- O(log n): Logarithmic
- O(n): Linear
- O(n log n): Linearithmic
- O(n²): Quadratic
- O(2^n): Exponential
- O(n!): Factorial

### 2. Complexity Hierarchy
```
O(1) < O(log n) < O(n) < O(n log n) < O(n²) < O(2^n) < O(n!)

Best         →              →              →         Worst
```

### 3. Time Complexity Examples
```javascript
// O(1) - Constant
const getFirst = (arr) => arr[0];

// O(n) - Linear
const sum = (arr) => {
  let total = 0;
  for (let num of arr) {
    total += num;
  }
  return total;
};

// O(n²) - Quadratic
const bubbleSort = (arr) => {
  for (let i = 0; i < arr.length; i++) {
    for (let j = 0; j < arr.length; j++) {
      // compare and swap
    }
  }
};

// O(log n) - Binary search
const binarySearch = (arr, target) => {
  let left = 0, right = arr.length - 1;
  while (left <= right) {
    const mid = Math.floor((left + right) / 2);
    if (arr[mid] === target) return mid;
    if (arr[mid] < target) left = mid + 1;
    else right = mid - 1;
  }
  return -1;
};
```

### 4. Space Complexity Examples
```javascript
// O(1) - Constant space
const max = (arr) => Math.max(...arr);

// O(n) - Linear space
const createArray = (n) => {
  const arr = [];
  for (let i = 0; i < n; i++) {
    arr.push(i);
  }
  return arr;
};

// O(n) - String concatenation
const concat = (arr) => {
  let result = '';
  for (let item of arr) {
    result += item; // Creates new string each time!
  }
  return result;
};
```

### 5. How to Calculate
1. Count operations in loops
2. Identify nested loops (multiply)
3. Ignore constants
4. Keep highest order term
5. Consider best/average/worst case

## Practice Tasks
- [ ] Analyze existing algorithms
- [ ] Calculate complexity
- [ ] Identify optimization opportunities
- [ ] Compare different approaches
- [ ] Write efficient code

## Interview Tips
- Always discuss complexity
- Mention both time and space
- Explain trade-offs
- Suggest optimizations
- Use Big O notation correctly

## Resources
- Big O Cheat Sheet
- Visualgo: Algorithm Visualization
- LeetCode Complexity tags
