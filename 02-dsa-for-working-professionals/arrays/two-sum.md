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

1️⃣ UNSORTED ARRAY → HASH MAP
🟥 Brute Force (O(n²))
function twoSumBrute(nums, target) {
    const res = [];
    for (let i = 0; i < nums.length; i++) {
        for (let j = i + 1; j < nums.length; j++) {
            if (nums[i] + nums[j] === target) {
                res.push([i, j]);
            }
        }
    }
    return res;
}


⏱️ Time: O(n²)
📦 Space: O(1)

🟩 Optimized – Hash Map (O(n))
👉 Return first pair
function twoSum(nums, target) {
    const map = new Map();

    for (let i = 0; i < nums.length; i++) {
        const diff = target - nums[i];

        if (map.has(diff)) {
            return [map.get(diff), i];
        }
        map.set(nums[i], i);
    }
}


⏱️ Time: O(n)
📦 Space: O(n)

🟩 Optimized – Hash Map
👉 Return ALL pairs (indices)
function twoSumAll(nums, target) {
    const map = new Map();
    const res = [];

    for (let i = 0; i < nums.length; i++) {
        const diff = target - nums[i];

        if (map.has(diff)) {
            res.push([map.get(diff), i]);
        }
        map.set(nums[i], i);
    }
    return res;
}

🔹 2️⃣ SORTED ARRAY → TWO POINTERS

⚠️ Works only if array is sorted

🟥 Brute Force (O(n²))
function twoSumSortedBrute(nums, target) {
    const res = [];
    for (let i = 0; i < nums.length; i++) {
        for (let j = i + 1; j < nums.length; j++) {
            if (nums[i] + nums[j] === target) {
                res.push([i, j]);
            }
        }
    }
    return res;
}

🟩 Optimized – Two Pointers (O(n))
👉 Return one pair
function twoSumSorted(nums, target) {
    let left = 0, right = nums.length - 1;

    while (left < right) {
        const sum = nums[left] + nums[right];

        if (sum === target) return [left, right];
        if (sum < target) left++;
        else right--;
    }
}

🟩 Optimized – Two Pointers
👉 Return ALL unique pairs
function twoSumSortedAll(nums, target) {
    let left = 0, right = nums.length - 1;
    const res = [];

    while (left < right) {
        const sum = nums[left] + nums[right];

        if (sum === target) {
            res.push([nums[left], nums[right]]);
            left++;
            right--;

            while (left < right && nums[left] === nums[left - 1]) left++;
            while (left < right && nums[right] === nums[right + 1]) right--;
        }
        else if (sum < target) left++;
        else right--;
    }
    return res;
}

🔹 3️⃣ MULTIPLE SOLUTIONS (Return ALL Pairs)
👉 Unsorted array (values, not indices)
function twoSumAllValues(nums, target) {
    const seen = new Set();
    const res = new Set();

    for (let num of nums) {
        const diff = target - num;

        if (seen.has(diff)) {
            res.add([Math.min(num, diff), Math.max(num, diff)].toString());
        }
        seen.add(num);
    }
    return Array.from(res).map(pair => pair.split(',').map(Number));
}

🔥 Interview Comparison Table
Case	Best Approach	Time	Space
Unsorted (one pair)	Hash Map	O(n)	O(n)
Unsorted (all pairs)	Hash Map	O(n)	O(n)
Sorted (one pair)	Two Pointers	O(n)	O(1)
Sorted (all pairs)	Two Pointers	O(n)	O(1)
