# Anagram & Frequency Problems

## Group Anagrams

### Problem
Group strings that are anagrams together.

### Solution

```javascript
// Time: O(n*k log k), Space: O(n*k)
// n = number of strings, k = average length
const groupAnagrams = (strs) => {
  const groups = {};
  
  for (let str of strs) {
    // Sort characters to create key
    const sorted = str.split('').sort().join('');
    
    if (!groups[sorted]) {
      groups[sorted] = [];
    }
    groups[sorted].push(str);
  }
  
  return Object.values(groups);
};

// Test
console.log(groupAnagrams(['eat','tea','tan','ate','nat','bat']));
// [['eat','tea','ate'],['tan','nat'],['bat']]
```

### Optimized Solution

```javascript
// Time: O(n*k), Space: O(n*k)
const groupAnagrams = (strs) => {
  const groups = {};
  
  for (let str of strs) {
    // Use frequency as key instead of sorting
    const key = getKey(str);
    
    if (!groups[key]) {
      groups[key] = [];
    }
    groups[key].push(str);
  }
  
  return Object.values(groups);
};

const getKey = (str) => {
  const count = new Array(26).fill(0);
  for (let char of str) {
    count[char.charCodeAt(0) - 'a'.charCodeAt(0)]++;
  }
  return count.join(',');
};
```

---

## Top K Frequent Elements

### Problem
Find K most frequent elements in array.

### Solution 1: Heap

```javascript
// Time: O(n log k), Space: O(n)
const topKFrequent = (nums, k) => {
  // Count frequencies
  const freq = {};
  for (let num of nums) {
    freq[num] = (freq[num] || 0) + 1;
  }
  
  // Create min heap of size k
  const heap = [];
  
  for (let [num, count] of Object.entries(freq)) {
    heap.push([num, count]);
    heap.sort((a, b) => a[1] - b[1]);
    
    if (heap.length > k) {
      heap.shift();
    }
  }
  
  return heap.map(item => parseInt(item[0]));
};
```

### Solution 2: Bucket Sort

```javascript
// Time: O(n), Space: O(n)
const topKFrequent = (nums, k) => {
  // Count frequencies
  const freq = {};
  for (let num of nums) {
    freq[num] = (freq[num] || 0) + 1;
  }
  
  // Bucket: index = frequency, value = numbers
  const buckets = new Array(nums.length + 1);
  
  for (let [num, count] of Object.entries(freq)) {
    if (!buckets[count]) {
      buckets[count] = [];
    }
    buckets[count].push(num);
  }
  
  // Collect results from buckets (reverse order)
  const result = [];
  for (let i = buckets.length - 1; i >= 0 && result.length < k; i--) {
    if (buckets[i]) {
      result.push(...buckets[i]);
    }
  }
  
  return result.slice(0, k);
};

console.log(topKFrequent([1,1,1,2,2,3], 2)); // [1, 2]
```

---

## Frequency Count Template

```javascript
// Basic frequency counter
const countFrequency = (arr) => {
  const freq = {};
  for (let item of arr) {
    freq[item] = (freq[item] || 0) + 1;
  }
  return freq;
};

// Find max frequency
const maxFrequency = (freq) => {
  return Math.max(...Object.values(freq));
};

// Sort by frequency
const sortByFrequency = (arr) => {
  const freq = countFrequency(arr);
  return Object.entries(freq)
    .sort((a, b) => b[1] - a[1])
    .map(entry => entry[0]);
};
```

## Practice Tasks
- [ ] Solve group anagrams
- [ ] Top K problems
- [ ] Frequency counting
- [ ] Bucket sort approach
- [ ] Optimize to O(n)

## Resources
- LeetCode: Frequency Problems
- CodeSignal: Hashmaps
