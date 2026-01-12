# Hash Map Patterns

## Frequency Count Problems

### Two Sum (Revisited)

```javascript
const twoSum = (nums, target) => {
  const seen = {};
  
  for (let i = 0; i < nums.length; i++) {
    const complement = target - nums[i];
    
    if (complement in seen) {
      return [seen[complement], i];
    }
    
    seen[nums[i]] = i;
  }
  
  return [];
};
```

### Check if Array Contains Duplicates

```javascript
const containsDuplicate = (nums) => {
  const seen = new Set();
  
  for (let num of nums) {
    if (seen.has(num)) return true;
    seen.add(num);
  }
  
  return false;
};

// Or with object
const containsDuplicate = (nums) => {
  const seen = {};
  
  for (let num of nums) {
    if (num in seen) return true;
    seen[num] = true;
  }
  
  return false;
};
```

### Valid Anagram

```javascript
const isAnagram = (s, t) => {
  if (s.length !== t.length) return false;
  
  const count = {};
  
  for (let char of s) {
    count[char] = (count[char] || 0) + 1;
  }
  
  for (let char of t) {
    if (!count[char]) return false;
    count[char]--;
  }
  
  return true;
};
```

---

## Hash Map Template

```javascript
// 1. Count occurrences
const countOccurrences = (arr) => {
  const map = {};
  for (let item of arr) {
    map[item] = (map[item] || 0) + 1;
  }
  return map;
};

// 2. Check existence
const exists = (arr, target) => {
  const set = new Set(arr);
  return set.has(target);
};

// 3. Find pairs
const findPairs = (arr, target) => {
  const seen = new Set();
  const pairs = new Set();
  
  for (let num of arr) {
    const complement = target - num;
    if (seen.has(complement)) {
      pairs.add(`${Math.min(num, complement)},${Math.max(num, complement)}`);
    }
    seen.add(num);
  }
  
  return Array.from(pairs);
};

// 4. Character mapping
const isIsomorphic = (s, t) => {
  if (s.length !== t.length) return false;
  
  const sToT = {}, tToS = {};
  
  for (let i = 0; i < s.length; i++) {
    const sc = s[i], tc = t[i];
    
    if ((sc in sToT && sToT[sc] !== tc) || 
        (tc in tToS && tToS[tc] !== sc)) {
      return false;
    }
    
    sToT[sc] = tc;
    tToS[tc] = sc;
  }
  
  return true;
};
```

## Set vs Object vs Map

| Operation | Object | Set | Map |
|-----------|--------|-----|-----|
| Add | `obj[key] = val` | `set.add(val)` | `map.set(key, val)` |
| Check | `key in obj` | `set.has(val)` | `map.has(key)` |
| Delete | `delete obj[key]` | `set.delete(val)` | `map.delete(key)` |
| Size | `Object.keys().length` | `set.size` | `map.size` |
| Keys | `Object.keys()` | N/A | `map.keys()` |

## Practice Tasks
- [ ] Solve frequency problems
- [ ] Two sum variations
- [ ] Check for duplicates
- [ ] Valid mappings
- [ ] Optimize to O(n)

## Resources
- LeetCode: Hash Table tag
- GeeksforGeeks: Hashing
