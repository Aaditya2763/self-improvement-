# Problem-Solving Patterns

## Learning Objectives
- Master common patterns
- Solve problems systematically
- Apply patterns to new problems
- Optimize solutions

## Topics

### 1. Two Pointers
- Sorted array problems
- Container with most water
- Trapping rain water
- Valid palindrome

```javascript
const twoSum = (arr, target) => {
  let left = 0, right = arr.length - 1;
  while (left < right) {
    const sum = arr[left] + arr[right];
    if (sum === target) return [left, right];
    if (sum < target) left++;
    else right--;
  }
  return null;
};
```

### 2. Sliding Window
- Substring problems
- Subarray problems
- Fixed window size
- Variable window size

```javascript
const maxSubarray = (arr, k) => {
  let maxSum = 0, currentSum = 0;
  for (let i = 0; i < arr.length; i++) {
    currentSum += arr[i];
    if (i >= k - 1) {
      maxSum = Math.max(maxSum, currentSum);
      currentSum -= arr[i - k + 1];
    }
  }
  return maxSum;
};
```

### 3. Hash Map / Frequency Count
- Finding pairs
- Counting occurrences
- Checking for duplicates
- Frequency problems

```javascript
const isPalindrome = (s) => {
  const freq = {};
  for (let char of s) {
    freq[char] = (freq[char] || 0) + 1;
  }
  
  let oddCount = 0;
  for (let count of Object.values(freq)) {
    if (count % 2 === 1) oddCount++;
  }
  return oddCount <= 1;
};
```

### 4. Pointer Manipulation
- Array partition
- Sorting related
- In-place modifications
- Quick select

### 5. Divide & Conquer
- Binary search
- Merge sort
- Quick sort
- Tree problems

### 6. Dynamic Programming
- Overlapping subproblems
- Optimal substructure
- Memoization
- Tabulation

## Problem-Solving Steps
1. **Understand**: Clarify requirements
2. **Example**: Work through examples
3. **Approach**: Plan solution
4. **Code**: Write code
5. **Test**: Test edge cases
6. **Optimize**: Improve complexity

## Common Interview Patterns
- [ ] Two Pointers
- [ ] Sliding Window
- [ ] Hash Map
- [ ] Recursion
- [ ] BFS/DFS
- [ ] Binary Search
- [ ] DP

## Practice Tasks
- [ ] Solve pattern-based problems
- [ ] Identify pattern in new problems
- [ ] Optimize brute force solutions
- [ ] Analyze complexity
- [ ] Practice multiple approaches

## Interview Tips
- Ask clarifying questions
- State approach before coding
- Think about edge cases
- Discuss complexity
- Mention alternative approaches

## Resources
- LeetCode Patterns
- Grokking Patterns (Educative)
- Algorithm Design Manual
