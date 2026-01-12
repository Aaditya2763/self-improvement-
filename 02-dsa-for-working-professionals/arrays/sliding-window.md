# Sliding Window Pattern

## Concept
- Maintain a window over data
- Expand and contract window
- Solve subarray/substring problems
- Often O(n) solution

## Template

```javascript
const slidingWindow = (arr, k) => {
  if (arr.length < k) return null;
  
  // Build initial window
  let window = 0;
  for (let i = 0; i < k; i++) {
    window += arr[i];
  }
  
  let maxWindow = window;
  
  // Slide window
  for (let i = k; i < arr.length; i++) {
    // Remove leftmost element
    window -= arr[i - k];
    // Add new rightmost element
    window += arr[i];
    // Update result
    maxWindow = Math.max(maxWindow, window);
  }
  
  return maxWindow;
};
```

## Problems

### Longest Substring with K Distinct Characters

```javascript
const lengthOfLongestSubstringKDistinct = (s, k) => {
  const charCount = {};
  let left = 0, maxLen = 0;
  
  for (let right = 0; right < s.length; right++) {
    // Expand window
    charCount[s[right]] = (charCount[s[right]] || 0) + 1;
    
    // Shrink window if too many distinct chars
    while (Object.keys(charCount).length > k) {
      charCount[s[left]]--;
      if (charCount[s[left]] === 0) {
        delete charCount[s[left]];
      }
      left++;
    }
    
    maxLen = Math.max(maxLen, right - left + 1);
  }
  
  return maxLen;
};
```

### Minimum Window Substring

```javascript
const minWindow = (s, t) => {
  if (t.length > s.length) return '';
  
  const targetCount = {};
  for (let char of t) {
    targetCount[char] = (targetCount[char] || 0) + 1;
  }
  
  let required = Object.keys(targetCount).length;
  let formed = 0;
  const windowCount = {};
  let left = 0, result = '', resultLen = Infinity;
  
  for (let right = 0; right < s.length; right++) {
    const char = s[right];
    windowCount[char] = (windowCount[char] || 0) + 1;
    
    if (targetCount[char] && windowCount[char] === targetCount[char]) {
      formed++;
    }
    
    while (formed === required && left <= right) {
      const windowLen = right - left + 1;
      if (windowLen < resultLen) {
        resultLen = windowLen;
        result = s.substring(left, right + 1);
      }
      
      const leftChar = s[left];
      windowCount[leftChar]--;
      if (targetCount[leftChar] && windowCount[leftChar] < targetCount[leftChar]) {
        formed--;
      }
      left++;
    }
  }
  
  return result;
};

console.log(minWindow('ADOBECODEBANC', 'ABC')); // 'BANC'
```

## Interview Tips
- Draw the window
- Explain expanding/contracting
- Discuss when to move left/right
- Optimize to O(n) with window
