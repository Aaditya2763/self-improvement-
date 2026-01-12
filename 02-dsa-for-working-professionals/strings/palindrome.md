# String Problems

## Valid Palindrome

### Problem
Check if string is valid palindrome (alphanumeric only, case-insensitive).

### Solution

```javascript
// Time: O(n), Space: O(1)
const isPalindrome = (s) => {
  let left = 0, right = s.length - 1;
  
  while (left < right) {
    // Skip non-alphanumeric
    while (left < right && !isAlphaNum(s[left])) left++;
    while (left < right && !isAlphaNum(s[right])) right++;
    
    if (s[left].toLowerCase() !== s[right].toLowerCase()) {
      return false;
    }
    
    left++;
    right--;
  }
  
  return true;
};

const isAlphaNum = (char) => {
  return /[a-zA-Z0-9]/.test(char);
};

// Test
console.log(isPalindrome('A man, a plan, a canal: Panama')); // true
```

### Approach
- Two pointer from both ends
- Skip non-alphanumeric
- Compare characters (case-insensitive)

---

## Anagram Detection

### Problem
Check if two strings are anagrams (same characters, different order).

### Solution 1: Sorting

```javascript
// Time: O(n log n), Space: O(1)
const isAnagram = (s, t) => {
  if (s.length !== t.length) return false;
  return s.split('').sort().join('') === t.split('').sort().join('');
};
```

### Solution 2: Hash Map

```javascript
// Time: O(n), Space: O(1) (limited charset)
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

// Test
console.log(isAnagram('anagram', 'nagaram')); // true
```

### Solution 3: Character Frequency Array

```javascript
// Time: O(n), Space: O(1)
const isAnagram = (s, t) => {
  if (s.length !== t.length) return false;
  
  const freq = new Array(26).fill(0);
  
  for (let i = 0; i < s.length; i++) {
    freq[s.charCodeAt(i) - 'a'.charCodeAt(0)]++;
    freq[t.charCodeAt(i) - 'a'.charCodeAt(0)]--;
  }
  
  return freq.every(count => count === 0);
};
```

---

## String Patterns

| Problem | Approach | Complexity |
|---------|----------|-----------|
| Palindrome | Two Pointers | O(n) |
| Anagram | Hash Map | O(n) |
| Longest Substring | Sliding Window | O(n) |
| Edit Distance | DP | O(m*n) |
| Word Pattern | Hash Map | O(n) |

## Practice Tasks
- [ ] Solve palindrome variations
- [ ] Practice anagram detection
- [ ] Try substring problems
- [ ] Learn regex patterns
- [ ] Optimize solutions

## Resources
- LeetCode: String tag
- HackerRank: String problems
- CodeSignal: String algorithms
