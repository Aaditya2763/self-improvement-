# Linked List Operations

## Reverse Linked List

### Problem
Reverse a singly linked list.

### Node Definition
```javascript
class ListNode {
  constructor(val = 0, next = null) {
    this.val = val;
    this.next = next;
  }
}
```

### Solution 1: Iterative

```javascript
// Time: O(n), Space: O(1)
const reverseList = (head) => {
  let prev = null;
  let curr = head;
  
  while (curr) {
    // Save next node
    const nextTemp = curr.next;
    
    // Reverse the link
    curr.next = prev;
    
    // Move prev and curr one step forward
    prev = curr;
    curr = nextTemp;
  }
  
  return prev;
};
```

### Solution 2: Recursive

```javascript
// Time: O(n), Space: O(n) - call stack
const reverseList = (head) => {
  if (!head || !head.next) return head;
  
  const newHead = reverseList(head.next);
  
  // Reverse the link
  head.next.next = head;
  head.next = null;
  
  return newHead;
};
```

---

## Common Linked List Problems

### Merge Two Sorted Lists

```javascript
const mergeTwoLists = (list1, list2) => {
  const dummy = new ListNode(0);
  let curr = dummy;
  
  while (list1 && list2) {
    if (list1.val <= list2.val) {
      curr.next = list1;
      list1 = list1.next;
    } else {
      curr.next = list2;
      list2 = list2.next;
    }
    curr = curr.next;
  }
  
  // Attach remaining nodes
  curr.next = list1 || list2;
  
  return dummy.next;
};
```

### Detect Cycle (Floyd's Algorithm)

```javascript
const hasCycle = (head) => {
  let slow = head, fast = head;
  
  while (fast && fast.next) {
    slow = slow.next;
    fast = fast.next.next;
    
    if (slow === fast) return true;
  }
  
  return false;
};
```

### Find Middle Node

```javascript
const findMiddle = (head) => {
  let slow = head, fast = head;
  
  while (fast && fast.next) {
    slow = slow.next;
    fast = fast.next.next;
  }
  
  return slow;
};
```

### Remove Nth Node From End

```javascript
const removeNthFromEnd = (head, n) => {
  const dummy = new ListNode(0);
  dummy.next = head;
  
  let first = dummy;
  let second = dummy;
  
  // Move first n+1 steps ahead
  for (let i = 0; i <= n; i++) {
    first = first.next;
  }
  
  // Move both until first reaches end
  while (first) {
    first = first.next;
    second = second.next;
  }
  
  second.next = second.next.next;
  
  return dummy.next;
};
```

## Linked List Template

```javascript
// Traverse
const traverse = (head) => {
  let curr = head;
  while (curr) {
    console.log(curr.val);
    curr = curr.next;
  }
};

// Create list from array
const createList = (arr) => {
  const dummy = new ListNode(0);
  let curr = dummy;
  
  for (let val of arr) {
    curr.next = new ListNode(val);
    curr = curr.next;
  }
  
  return dummy.next;
};

// Convert list to array
const listToArray = (head) => {
  const result = [];
  let curr = head;
  
  while (curr) {
    result.push(curr.val);
    curr = curr.next;
  }
  
  return result;
};
```

## Practice Tasks
- [ ] Reverse linked list
- [ ] Merge sorted lists
- [ ] Detect cycle
- [ ] Find kth node
- [ ] Reorder list

## Resources
- LeetCode: Linked List tag
- Visualgo: Linked List animation
