# Binary_Tree_Traversal

# 1. Binary Tree Traversal - Preorder, Inorder and Postorder

## 1. Recursively

recursively前中后序遍历代码几乎一样，只是顺序不同。

### [LC 144: Binary Tree Preorder Traversal](https://leetcode.com/problems/binary-tree-preorder-traversal/)

```java
class Solution {
    public List<Integer> preorderTraversal(TreeNode root) {
        List<Integer> res = new ArrayList<>();
        dfs(root, res);
        return res;  
    }
    private void dfs(TreeNode root, List<Integer> list) {
        if (root == null) {
            return;
        }
        list.add(root.val);
        dfs(root.left, list);
        dfs(root.right, list);
    }
}
```

### [LC 94: Binary Tree Inorder Traversal](https://leetcode.com/problems/binary-tree-inorder-traversal/)

```java
class Solution {
    public List<Integer> inorderTraversal(TreeNode root) {
        List<Integer> res = new ArrayList<>();
        dfs(root, res);
        return res;  
    }
    private void dfs(TreeNode root, List<Integer> list) {
        if (root == null) {
            return;
        }
        dfs(root.left, list);
        list.add(root.val);
        dfs(root.right, list);
    }
}
```

### [LC 145: Binary Tree Postorder Traversal](https://leetcode.com/problems/binary-tree-postorder-traversal/)

```java
 class Solution {
    public List<Integer> postorderTraversal(TreeNode root) {
        List<Integer> res = new ArrayList<>();
        dfs(root, res);
        return res;
    }
    private void dfs(TreeNode root, List<Integer> list) {
        if (root == null) {
            return;
        }
        dfs(root.left, list); 
        dfs(root.right, list);
        list.add(root.val);  
    }
}
```

## 2. Iteratively

### 1. preorder traversal

借助栈。思路与后序遍历(借助栈)基本相同，只有左右子树入栈顺序不同。

思路：

* step1：root入栈；
* step2：top元素出栈；
* step3：将top.val放入链表，同时，top的***右**子树先入栈，**左**子树后入栈*；
* step4：重复step2，当栈为空时停止循环。

  ```java
  class Solution {
      public List<Integer> preorderTraversal(TreeNode root) {
          List<Integer> res = new ArrayList<>();
          if (root == null) {
              return res;
          }
          Stack<TreeNode> stack = new Stack<>();
          stack.push(root);
          while (!stack.isEmpty()) {
              TreeNode curNode = stack.pop();
              res.add(curNode.val);
  /**入栈：先右子树，后做子树*/
              if (curNode.right != null) {   
                  stack.push(curNode.right);
              }
              if (curNode.left != null) {
                  stack.push(curNode.left);
              }
          }
          return res;  
      }
  }
  ```

### 2. inorder traversal

思路：pointer指向root，并且入栈，一路向左下，逐个入栈，直至到达null（即到达leaf），然后开始出栈

```java
class Solution {
    public List<Integer> inorderTraversal(TreeNode root) {
        List<Integer> res = new ArrayList<>();
        if (res == null) {
            return res;
        }
        Stack<TreeNode> stack = new Stack<>();
        TreeNode curNode = root;
//当stack和curNode均为空时，既无node需要入栈，也无node需要出栈， 则退出循环。
        while (!stack.isEmpty() || curNode != null) {
            while (curNode != null) {
                stack.push(curNode);
                curNode = curNode.left;
            }
            curNode = stack.pop();
            res.add(curNode.val);
            curNode = curNode.right;   
        }
        return res;  
    }
}
```

### 3. postorder traversal

    思路：

* step1：root入栈；
* step2：top元素出栈；
* step3：将top.val放入链表，同时，top的***左**子树先入栈，**右**子树后入栈*；
* step4：重复step2，当栈为空时停止循环。

  ```java


  class Solution {
      public List<Integer> postorderTraversal(TreeNode root) {
          List<Integer> res = new ArrayList<>();
          if (root == null) {
              return res;
          }

          Stack<TreeNode> stack = new Stack<>();
          stack.push(root);
          while (!stack.isEmpty()) {
              TreeNode curNode = stack.pop();
              res.add(0, curNode.val);
   /**入栈：先左子树，后右子树。*/
              if (curNode.left != null) {
                  stack.push(curNode.left);
              }
              if (curNode.right != null) {
                  stack.push(curNode.right);
              }
          }
          return res;  
      }
  }
  ```

# 2. Binary Tree Traversal - Level Order

## [LC 102: Binary Tree Level Order Traversal](https://leetcode.com/problems/binary-tree-level-order-traversal/)

* 思路：借助queue
* 重点：队列中的元素出队列时，其左右子树（若非空）需要入队列
  关于数据结构：链表可以实现队列

```java

class Solution {
    public List<List<Integer>> levelOrder(TreeNode root) {
        List<List<Integer>> res = new ArrayList<>();
        if (root == null) {
            return res;
        }
        Queue<TreeNode> queue = new LinkedList<>();
        queue.add(root);

        while (!queue.isEmpty()) {
            int qLen = queue.size();
            List<Integer> level = new ArrayList<>();
            for (int i = 0; i < qLen; i +=1) {
                TreeNode curNode = queue.poll();
                level.add(curNode.val);
                if (curNode.left != null) {
                    queue.add(curNode.left);
                }
                if (curNode.right != null) {
                    queue.add(curNode.right);
                }
            }
            res.add(level);
        }
        return res;

    }
}
```

## [LC 103：Binary Tree Zigzag Level Order Traversal](https://leetcode.com/problems/binary-tree-zigzag-level-order-traversal/)

思路：跟LC102非常相似，就是在102的基础上加了一个参数count，当count为偶数时，从左往右；当count为奇数时，从右往左。

```java
class Solution {
    public List<List<Integer>> zigzagLevelOrder(TreeNode root) {
        List<List<Integer>> res = new ArrayList<>();
        if (root == null) {
            return res;
        }
        Queue<TreeNode> queue = new LinkedList<>();
        queue.add(root);
        int count = 0;

        while (!queue.isEmpty()) {
            int qLen = queue.size();
            List<Integer> level = new ArrayList<>();
            for (int i = 0; i < qLen; i += 1) {
                TreeNode curNode = queue.poll();
/**当层数为偶数时，一直addlast；当层数为奇数时，一直addfirst。*/
                if (count%2 == 0) {
                    level.add(curNode.val);
                } else {
                    level.add(0, curNode.val);
                }
                if (curNode.left != null) {
                    queue.add(curNode.left);
                }
                if (curNode.right != null) {
                    queue.add(curNode.right);
                }   
            }
            res.add(level);
            count += 1;
        }
        return res;  
    }
}
```

## [LC 107. Binary Tree Level Order Traversal II](https://leetcode.com/problems/binary-tree-level-order-traversal-ii/)

思路：在LC 102的基础上，只有一行代码有所改动。

```java
class Solution {
    public List<List<Integer>> levelOrderBottom(TreeNode root) {
        List<List<Integer>> res = new ArrayList<>();
        if (root == null) {
            return res;
        }
        Queue<TreeNode> queue = new LinkedList<>();
        queue.add(root);
        while (!queue.isEmpty()) {
            int qLen = queue.size();
            List<Integer> level = new ArrayList<>();
            for (int i = 0; i < qLen; i +=1) {
                TreeNode curNode = queue.poll();
                level.add(curNode.val);
                if (curNode.left != null) {
                    queue.add(curNode.left);
                }
                if (curNode.right != null) {
                    queue.add(curNode.right);
                }
            }
/**lc102是addlast，这道题这里改成addfirst即可。*/
            res.add(0, level);
        }
        return res;  
    }
}
```

