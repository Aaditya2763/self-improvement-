# Array Problems

## Two Sum (LeetCode 1)

### Problem
Given array of integers and target, return indices of the two numbers that add up to target.

### Solution - Hash Map
```javascript
// Time: O(n), Space: O(n)
const twoSum = (nums, target) => {
  const seen = {};
  for (let i = 0; i < nums.length; i++) {
    const complement = target - nums[i];
    if (seen[complement] !== undefined) {
      return [seen[complement], i];
    }
    seen[nums[i]] = i;
  }
  return [];
};

// Example
console.log(twoSum([2,7,11,15], 9)); // [0, 1]
```

### Approach
1. Use hash map to store value → index
2. For each number, check if complement exists
3. Return indices when found

### Variations
- Unsorted array (hash map)
- Sorted array (two pointers)
- Multiple solutions (return all pairs)

---

## Sliding Window Problems

### Max Subarray Sum (Kadane's Algorithm)

```javascript
// Time: O(n), Space: O(1)
const maxSubarraySum = (arr) => {
  let maxCurrent = arr[0];
  let maxGlobal = arr[0];
  
  for (let i = 1; i < arr.length; i++) {
    maxCurrent = Math.max(arr[i], maxCurrent + arr[i]);
    maxGlobal = Math.max(maxGlobal, maxCurrent);
  }
  
  return maxGlobal;
};

console.log(maxSubarraySum([-2,1,-3,4,-1,2,1,-5,4])); // 6
```

### Longest Substring Without Repeating

```javascript
// Time: O(n), Space: O(n)
const lengthOfLongestSubstring = (s) => {
  const charIndex = {};
  let maxLen = 0, start = 0;
  
  for (let i = 0; i < s.length; i++) {
    if (charIndex[s[i]] !== undefined) {
      start = Math.max(start, charIndex[s[i]] + 1);
    }
    charIndex[s[i]] = i;
    maxLen = Math.max(maxLen, i - start + 1);
  }
  
  return maxLen;
};

console.log(lengthOfLongestSubstring('abcabcbb')); // 3
```

---

## Common Array Problems

| Problem | Pattern | Complexity |
|---------|---------|-----------|
| Two Sum | Hash Map | O(n) |
| Merge Sorted Arrays | Two Pointers | O(n+m) |
| Remove Duplicates | Two Pointers | O(n) |
| Rotate Array | Math | O(n) |
| Best Time to Buy Stock | Tracking | O(n) |

## Practice Tasks
- [ ] Solve 5 two-pointer problems
- [ ] Solve 5 sliding window problems
- [ ] Optimize brute force solutions
- [ ] Understand trade-offs
- [ ] Code without looking up

## Resources
- LeetCode: Array tag
- GeeksforGeeks: Array problems
- InterviewBit: Array section
