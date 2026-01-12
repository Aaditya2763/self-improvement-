# Tree Traversal

## Tree Node Definition
```javascript
class TreeNode {
  constructor(val = 0, left = null, right = null) {
    this.val = val;
    this.left = left;
    this.right = right;
  }
}
```

---

## Depth-First Search (DFS)

### 1. Inorder Traversal (Left → Root → Right)

```javascript
// Recursive
const inorderTraversal = (root) => {
  const result = [];
  
  const traverse = (node) => {
    if (!node) return;
    traverse(node.left);
    result.push(node.val);
    traverse(node.right);
  };
  
  traverse(root);
  return result;
};

// Iterative with stack
const inorderTraversal = (root) => {
  const result = [];
  const stack = [];
  let curr = root;
  
  while (curr || stack.length > 0) {
    while (curr) {
      stack.push(curr);
      curr = curr.left;
    }
    
    curr = stack.pop();
    result.push(curr.val);
    curr = curr.right;
  }
  
  return result;
};
```

### 2. Preorder Traversal (Root → Left → Right)

```javascript
// Recursive
const preorderTraversal = (root) => {
  const result = [];
  
  const traverse = (node) => {
    if (!node) return;
    result.push(node.val);
    traverse(node.left);
    traverse(node.right);
  };
  
  traverse(root);
  return result;
};

// Iterative with stack
const preorderTraversal = (root) => {
  if (!root) return [];
  
  const result = [];
  const stack = [root];
  
  while (stack.length > 0) {
    const node = stack.pop();
    result.push(node.val);
    
    // Push right first so left is processed first
    if (node.right) stack.push(node.right);
    if (node.left) stack.push(node.left);
  }
  
  return result;
};
```

### 3. Postorder Traversal (Left → Right → Root)

```javascript
// Recursive
const postorderTraversal = (root) => {
  const result = [];
  
  const traverse = (node) => {
    if (!node) return;
    traverse(node.left);
    traverse(node.right);
    result.push(node.val);
  };
  
  traverse(root);
  return result;
};

// Iterative with two stacks
const postorderTraversal = (root) => {
  if (!root) return [];
  
  const result = [];
  const stack1 = [root];
  const stack2 = [];
  
  while (stack1.length > 0) {
    const node = stack1.pop();
    stack2.push(node);
    
    if (node.left) stack1.push(node.left);
    if (node.right) stack1.push(node.right);
  }
  
  while (stack2.length > 0) {
    result.push(stack2.pop().val);
  }
  
  return result;
};
```

---

## Breadth-First Search (BFS)

### Level Order Traversal (Queue)

```javascript
// Time: O(n), Space: O(w) where w is max width
const levelOrderTraversal = (root) => {
  if (!root) return [];
  
  const result = [];
  const queue = [root];
  
  while (queue.length > 0) {
    const levelSize = queue.length;
    const levelResult = [];
    
    for (let i = 0; i < levelSize; i++) {
      const node = queue.shift();
      levelResult.push(node.val);
      
      if (node.left) queue.push(node.left);
      if (node.right) queue.push(node.right);
    }
    
    result.push(levelResult);
  }
  
  return result;
};

// Example: [1,2,3,4,5,null,6]
// Output: [[1], [2,3], [4,5,6]]
```

---

## Common Tree Problems

### Maximum Depth (Height)
```javascript
const maxDepth = (root) => {
  if (!root) return 0;
  return 1 + Math.max(maxDepth(root.left), maxDepth(root.right));
};
```

### Validate Binary Search Tree
```javascript
const isValidBST = (root) => {
  const validate = (node, min, max) => {
    if (!node) return true;
    
    if (node.val <= min || node.val >= max) return false;
    
    return validate(node.left, min, node.val) && 
           validate(node.right, node.val, max);
  };
  
  return validate(root, -Infinity, Infinity);
};
```

### Path Sum
```javascript
const hasPathSum = (root, targetSum) => {
  if (!root) return false;
  
  if (!root.left && !root.right) {
    return root.val === targetSum;
  }
  
  return hasPathSum(root.left, targetSum - root.val) ||
         hasPathSum(root.right, targetSum - root.val);
};
```

## Traversal Comparison

| Traversal | Order | Use Case |
|-----------|-------|----------|
| Inorder | L-R-Root | BST → sorted |
| Preorder | Root-L-R | Copy tree |
| Postorder | L-R-Root | Delete tree |
| Level Order | By level | Shortest path |

## Practice Tasks
- [ ] All three DFS approaches
- [ ] BFS level order
- [ ] Recursive solutions
- [ ] Iterative solutions
- [ ] Combination problems

## Resources
- LeetCode: Binary Tree tag
- Visualgo: Tree visualization
