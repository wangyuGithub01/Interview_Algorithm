<!-- GFM-TOC -->
:warning: 表示容易忘记思路的 \
:deciduous_tree: 表示当前进度 \
:hibiscus: 表示已完成

* [递归](#递归) :hibiscus:
    * [树的高度](#树的高度)
    * [平衡树](#平衡树) 
    * [树的直径/最长路径](#树的直径)
    * [翻转树](#翻转树) 
    * [归并两棵树](#归并两棵树)
    * [判断路径和是否等于一个数](#判断路径和是否等于一个数)
    * [统计路径和等于一个数的路径数量](#统计路径和等于一个数的路径数量) :warning:
    * [子树](#子树) 
    * [树的对称](#树的对称) 
    * [最小路径](#最小路径)
    * [统计左叶子节点的和](#统计左叶子节点的和) 
    * [相同节点值的最大路径长度](#相同节点值的最大路径长度) 
    * [间隔遍历](#间隔遍历) :warning:
    * [找出二叉树中第二小的节点](#找出二叉树中第二小的节点)
* [层次遍历](#层次遍历) :hibiscus:
    * [一棵树每层节点的平均数](#一棵树每层节点的平均数)
    * [得到左下角的节点](#得到左下角的节点)
* [前中后序遍历](#前中后序遍历) :hibiscus:
    * [非递归实现二叉树的前序遍历](#非递归实现二叉树的前序遍历)
    * [非递归实现二叉树的后序遍历](#非递归实现二叉树的后序遍历)
    * [非递归实现二叉树的中序遍历](#非递归实现二叉树的中序遍历)
* [BST](#bst) :hibiscus:
    * [修剪二叉查找树](#修剪二叉查找树) :warning:
    * [寻找二叉查找树的第 k 个元素](#寻找二叉查找树的第-k-个元素)
    * [把二叉查找树每个节点的值都加上比它大的节点的值](#把二叉查找树每个节点的值都加上比它大的节点的值) 
    * [二叉查找树的最近公共祖先](#二叉查找树的最近公共祖先)
    * [二叉树的最近公共祖先](#二叉树的最近公共祖先) :warning: 
    * [从有序数组中构造二叉查找树](#从有序数组中构造二叉查找树)
    * [根据有序链表构造平衡的二叉查找树](#根据有序链表构造平衡的二叉查找树)
    * [在二叉查找树中寻找两个节点，使它们的和为一个给定值](#在二叉查找树中寻找两个节点，使它们的和为一个给定值)
    * [在二叉查找树中查找两个节点之差的最小绝对值](#在二叉查找树中查找两个节点之差的最小绝对值) 
    * [寻找二叉查找树中出现次数最多的值](#寻找二叉查找树中出现次数最多的值)
* [Trie](#trie) :hibiscus:
    * [实现一个 Trie](#实现一个-trie)
    * [实现一个 Trie，用来求前缀和](#实现一个-trie，用来求前缀和)
<!-- GFM-TOC -->


# 递归

一棵树要么是空树，要么有两个指针，每个指针指向一棵树。树是一种递归结构，很多树的问题可以使用递归来处理。

## 树的高度

[104. Maximum Depth of Binary Tree (Easy)](https://leetcode.com/problems/maximum-depth-of-binary-tree/description/)

```python
class Solution(object):
    def maxDepth(self, root):
        """
        :type root: TreeNode
        :rtype: int
        """
        if root == None:
            return 0
        else:
            return max(1 + self.maxDepth(root.left), 1 + self.maxDepth(root.right))
```

## 平衡树

[110. Balanced Binary Tree (Easy)](https://leetcode.com/problems/balanced-binary-tree/description/)

```html
    3
   / \
  9  20
    /  \
   15   7
```

平衡树左右子树高度差都小于等于 1

```python
class Solution(object):
    def isBalanced(self, root):
        """
        :type root: TreeNode
        :rtype: bool
        """
        if root==None:
            return True
        self.balance = True
        self.subBalance(root)
        return self.balance
    
    def subBalance(self,root):
        if root==None:
            return 0
        left = self.subBalance(root.left)
        right = self.subBalance(root.right)
        if abs(left-right)>1:
            self.balance = False
        return 1 + max(left,right)
```

## 树的直径

[543. Diameter of Binary Tree (Easy)](https://leetcode.com/problems/diameter-of-binary-tree/description/)

```html
给定一棵二叉树，你需要计算它的直径长度。一棵二叉树的直径长度是任意两个结点路径长度中的最大值。这条路径可能穿过根结点。

示例 :
给定二叉树

          1
         / \
        2   3
       / \     
      4   5    
返回 3, 它的长度是路径 [4,2,1,3] 或者 [5,2,1,3]。

注意：两结点之间的路径长度是以它们之间边的数目表示。
```
```python
class Solution(object):
    def diameterOfBinaryTree(self, root):
        if root==None:
            return 0
        self.diameter = 0
        self.helper(root)
        return self.diameter - 1 # 减1是因为最长路径的”顶点“被两边的”半径“包含，计算了两次
    
    def helper(self, root):
        if root == None:
            return 0
        left = self.helper(root.left)
        right = self.helper(root.right)
        d = left + 1 + right # 以当前root为顶点的最长路径
        self.diameter =  max(d, self.diameter)
        return max(1+left, 1+right) # 当前root对应的最长“半径”
```

## 翻转树

[226. Invert Binary Tree (Easy)](https://leetcode.com/problems/invert-binary-tree/description/)

```python
class Solution(object):
    def invertTree(self, root):
        if root==None:
            return
        root.left, root.right = root.right, root.left
        self.invertTree(root.left)
        self.invertTree(root.right)
        return root
```

## 归并两棵树

[617. Merge Two Binary Trees (Easy)](https://leetcode.com/problems/merge-two-binary-trees/description/)

```html
Input:
       Tree 1                     Tree 2
          1                         2
         / \                       / \
        3   2                     1   3
       /                           \   \
      5                             4   7

Output:
         3
        / \
       4   5
      / \   \
     5   4   7
```

```python
class Solution:
    def mergeTrees(self, t1, t2):
        if t1==None and t2==None:
            return None
        if t1==None or t2==None:
            return t1 or t2
        newt = TreeNode(t1.val + t2.val)
        newt.left = self.mergeTrees(t1.left, t2.left)
        newt.right = self.mergeTrees(t1.right, t2.right)
        return newt
```

## 判断路径和是否等于一个数

[Leetcdoe : 112. Path Sum (Easy)](https://leetcode.com/problems/path-sum/description/)

```html
Given the below binary tree and sum = 22,

              5
             / \
            4   8
           /   / \
          11  13  4
         /  \      \
        7    2      1

return true, as there exist a root-to-leaf path 5->4->11->2 which sum is 22.
```

路径和定义为从 root 到 leaf 的所有节点的和。

```python
class Solution:
    def hasPathSum(self, root, target) :
        if root==None:
            return False
        if root.left==None and root.right==None and root.val==target:
            return True
        return self.hasPathSum(root.left,target-root.val) or self.hasPathSum(root.right,target-root.val)
```

## 统计路径和等于一个数的路径数量

[437. Path Sum III (Easy)](https://leetcode.com/problems/path-sum-iii/description/)

```html
root = [10,5,-3,3,2,null,11,3,-2,null,1], sum = 8

      10
     /  \
    5   -3
   / \    \
  3   2   11
 / \   \
3  -2   1

Return 3. The paths that sum to 8 are:

1.  5 -> 3
2.  5 -> 2 -> 1
3. -3 -> 11
```

路径不一定以 root 开头，也不一定以 leaf 结尾，但是必须连续。

```python
class Solution(object):
    def pathSum(self, root, target):
        if root==None:
            return 0
        return self.helper(root,target) + self.pathSum(root.left,target) + self.pathSum(root.right, target)
        
    def helper(self, root, target): 
        if root==None:
            return 0
        cur = 0
        if root.val==target:
            cur = 1
        return cur + self.helper(root.left,target-root.val)+self.helper(root.right, target-root.val)
```

## 子树

[572. Subtree of Another Tree (Easy)](https://leetcode.com/problems/subtree-of-another-tree/description/)

```html
Given tree s:
     3
    / \
   4   5
  / \
 1   2

Given tree t:
   4
  / \
 1   2

Return true, because t has the same structure and node values with a subtree of s.

Given tree s:

     3
    / \
   4   5
  / \
 1   2
    /
   0

Given tree t:
   4
  / \
 1   2

Return false.
```

```python
class Solution:
    def isSubtree(self, t1: TreeNode, t2: TreeNode) -> bool:
        if t1==None or t2==None:
            return False
        return self.subtree(t1, t2) or self.isSubtree(t1.left, t2) or self.isSubtree(t1.right, t2)
    
    def subtree(self, root1, root2):
        if root1==None and root2==None:
            return True
        if root1==None or root2==None:
            return False
        if root1.val==root2.val:
            return self.subtree(root1.left, root2.left) and self.subtree(root1.right, root2.right) 
        else:
            return False
```

## 树的对称

[101. Symmetric Tree (Easy)](https://leetcode.com/problems/symmetric-tree/description/)

```html
    1
   / \
  2   2
 / \ / \
3  4 4  3
```

```python
class Solution:
    def isSymmetric(self, root: TreeNode) -> bool:
        if root==None:
            return True
        return self.check(root, root)
    
    def check(self, root1, root2):
        if root1==None and root2==None:
            return True
        if root1==None or root2==None:
            return False
        
        if root1.val==root2.val:
            return self.check(root1.left, root2.right) and self.check(root1.right, root2.left)
        else:
            return False
```

## 最小路径

[111. Minimum Depth of Binary Tree (Easy)](https://leetcode.com/problems/minimum-depth-of-binary-tree/description/)

树的根节点到叶子节点的最小路径长度

```python
class Solution:
    def minDepth(self, root: TreeNode) -> int:
        if root==None:
            return 0
        left = self.minDepth(root.left)
        right = self.minDepth(root.right)
        if left==0 or right==0:
            return left+right+1
        return min(left, right) + 1
```

## 统计左叶子节点的和

[404. Sum of Left Leaves (Easy)](https://leetcode.com/problems/sum-of-left-leaves/description/)

```html
    3
   / \
  9  20
    /  \
   15   7

There are two left leaves in the binary tree, with values 9 and 15 respectively. Return 24.
```

```python
class Solution:
    def sumOfLeftLeaves(self, root: TreeNode) -> int:
        if root==None:
            return 0
        val = 0
        if root.left!=None:
            if root.left.right==None and root.left.left==None:
                val += root.left.val
            val += self.sumOfLeftLeaves(root.left)
        if root.right!=None:
            val += self.sumOfLeftLeaves(root.right)
        return val
```

## 相同节点值的最大路径长度

[687. Longest Univalue Path (Easy)](https://leetcode.com/problems/longest-univalue-path/)

```html
             1
            / \
           4   5
          / \   \
         4   4   5

Output : 2
```

```python
class Solution:
    def longestUnivaluePath(self, root: TreeNode) -> int:
        if root==None:
            return 0
        self.l = 0
        self.helper(root)
        return self.l
    def helper(self, root):
        if root==None:
            return 0
        left = self.helper(root.left)
        right = self.helper(root.right)
        
        if root.left and root.left.val == root.val:
            left = 1 + left
        else:
            left = 0
        if root.right and root.right.val == root.val:
            right = 1 + right
        else:
            right = 0
        self.l = max(self.l, right+left)
        return max(left,right)
```

## 间隔遍历

[337. House Robber III (Medium)](https://leetcode.com/problems/house-robber-iii/description/)

```html
     3
    / \
   2   3
    \   \
     3   1
Maximum amount of money the thief can rob = 3 + 3 + 1 = 7.
```
```python
class Solution(object):
    def __init__(self):
        self.map = {}
    def rob(self, root):
        """
        :type root: TreeNode
        :rtype: int
        """
        if root==None:
            return 0
        if root in self.map:
            return self.map[root]
        val = 0
        if root.left:
            val += self.rob(root.left.left)
            val += self.rob(root.left.right)
        if root.right:
            val += self.rob(root.right.left)
            val += self.rob(root.right.right)
        self.map[root] = max(root.val+val, self.rob(root.left) + self.rob(root.right))
        
        return self.map[root]
```

## 找出二叉树中第二小的节点

[671. Second Minimum Node In a Binary Tree (Easy)](https://leetcode.com/problems/second-minimum-node-in-a-binary-tree/description/)

```html
Input:
   2
  / \
 2   5
    / \
    5  7

Output: 5
```

一个节点要么具有 0 个或 2 个子节点，如果有子节点，那么根节点是最小的节点。

```python
class Solution:
    def findSecondMinimumValue(self, root: TreeNode) -> int:
        if root==None:
            return -1
        self.min = [float('inf'),float('inf')]
        self.helper(root)
        if self.min[1]==float('inf'):
            return -1
        return self.min[1]
        
    def helper(self,root):
        if root==None:
            return
        if root.left and root.right:
            v = min(root.left.val, root.right.val)
            self.insert(v)
        else:
            self.insert(root.val)
        self.helper(root.right)
        self.helper(root.left)
            
    def insert(self, v):
        if v in self.min:
            return
        elif v < self.min[0]:
            t = self.min[0]
            self.min[0] = v
            self.min[1] = t
        elif v < self.min[1]:
            self.min[1] = v
```

# 层次遍历

使用 BFS 进行层次遍历。不需要使用两个队列来分别存储当前层的节点和下一层的节点，因为在开始遍历一层的节点时，当前队列中的节点数就是当前层的节点数，只要控制遍历这么多节点数，就能保证这次遍历的都是当前层的节点。

## 一棵树每层节点的平均数

[637. Average of Levels in Binary Tree (Easy)](https://leetcode.com/problems/average-of-levels-in-binary-tree/description/)

```python
class Solution:
    def averageOfLevels(self, root: TreeNode) -> List[float]:
        if root==None:
            return []
        ret = []
        queue = [root]
        while len(queue)>0:
            summ = 0
            cnt = len(queue)
            for i in range(cnt):
                node = queue.pop(0)
                if node.left:
                    queue.append(node.left)
                if node.right:
                    queue.append(node.right)
                summ += node.val
            ret.append(summ/cnt)
            
        return ret
```

## 得到左下角的节点

[513. Find Bottom Left Tree Value (Easy)](https://leetcode.com/problems/find-bottom-left-tree-value/description/)

```html
Input:

        1
       / \
      2   3
     /   / \
    4   5   6
       /
      7

Output:
7
```

```python
# 两种思路，（1）从左到右层次遍历，返回最后一层第一个；（2）每层从右到左，返回最后一个
# 
class Solution:
    def findBottomLeftValue(self, root: TreeNode) -> int:
        if root==None:
            return
        queue = [root]
        while len(queue):
            node = queue.pop(0)
            if node.right:
                queue.append(node.right)
            if node.left:
                queue.append(node.left)
                
        return node.val
```

# 前中后序遍历

```html
    1
   / \
  2   3
 / \   \
4   5   6
```

- 层次遍历顺序：[1 2 3 4 5 6]
- 前序遍历顺序：[1 2 4 5 3 6]
- 中序遍历顺序：[4 2 5 1 3 6]
- 后序遍历顺序：[4 5 2 6 3 1]

层次遍历使用 BFS 实现，利用的就是 BFS 一层一层遍历的特性；而前序、中序、后序遍历利用了 DFS 实现。

前序、中序、后序遍只是在对节点访问的顺序有一点不同，其它都相同。

① 前序

```java
void dfs(TreeNode root) {
    visit(root);
    dfs(root.left);
    dfs(root.right);
}
```

② 中序

```java
void dfs(TreeNode root) {
    dfs(root.left);
    visit(root);
    dfs(root.right);
}
```

③ 后序

```java
void dfs(TreeNode root) {
    dfs(root.left);
    dfs(root.right);
    visit(root);
}
```

## 非递归实现二叉树的前序遍历

[144. Binary Tree Preorder Traversal (Medium)](https://leetcode.com/problems/binary-tree-preorder-traversal/description/)

```python
class Solution:
    def preorderTraversal(self, root):
        stack = []
        ret = []
        while root or len(stack)>0:
            if root:
                ret.append(root.val)
                stack.append(root.right)
                stack.append(root.left)
            root = stack.pop()
        return ret
```
## 非递归实现二叉树的中序遍历

[94. Binary Tree Inorder Traversal (Medium)](https://leetcode.com/problems/binary-tree-inorder-traversal/description/)

```python
class Solution:
   def inorderTraversal(self, root):
        stack, res = [], []
        node = root
        while node or len(stack)>0:
            while node:
                stack.append(node)
                node = node.left
            node = stack.pop()
            res.append(node.val)
            node = node.right
        return res
```
## 非递归实现二叉树的后序遍历

[145. Binary Tree Postorder Traversal (Medium)](https://leetcode.com/problems/binary-tree-postorder-traversal/description/)

```python
def postorderTraversal(self, root):
     res, stack = [], [(root, False)]
     while stack:
         node, visited = stack.pop()
         if node:
             if visited:
                 # add to result if visited
                 res.append(node.val)
             else:
                 # post-order
                 stack.append((node, True))
                 stack.append((node.right, False))
                 stack.append((node.left, False))
     return res
```

# BST

二叉查找树（BST）：根节点大于等于左子树所有节点，小于等于右子树所有节点。

二叉查找树中序遍历有序。

## 修剪二叉查找树 :warning:

[669. Trim a Binary Search Tree (Easy)](https://leetcode.com/problems/trim-a-binary-search-tree/description/)

```html
Input:

    3
   / \
  0   4
   \
    2
   /
  1

  L = 1
  R = 3

Output:

      3
     /
   2
  /
 1
```

题目描述：只保留值在 L \~ R 之间的节点

```python
class Solution:
    def trimBST(self, root: TreeNode, L: int, R: int) -> TreeNode:
        if root==None:
            return
        if root.val < L :
            return self.trimBST(root.right, L, R)
        if root.val > R:
            return self.trimBST(root.left, L, R)
        root.left = self.trimBST(root.left, L, R)
        root.right = self.trimBST(root.right, L, R)
        return root
```

## 寻找二叉查找树的第 k 个元素

[230. Kth Smallest Element in a BST (Medium)](https://leetcode.com/problems/kth-smallest-element-in-a-bst/description/)

中序遍历解法：
```python
class Solution:
    def kthSmallest(self, root: TreeNode, k: int) -> int:
        self.cnt = 0
        self.val = -1
        def helper(root):
            if root==None:
                return
            helper(root.left)
            self.cnt += 1
            if self.cnt == k:
                self.val = root.val
                return
            helper(root.right)
        helper(root)
        return self.val
```


## 把二叉查找树每个节点的值都加上比它大的节点的值 :deciduous_tree:

[Convert BST to Greater Tree (Easy)](https://leetcode.com/problems/convert-bst-to-greater-tree/description/)

```html
Input: The root of a Binary Search Tree like this:

              5
            /   \
           2     13

Output: The root of a Greater Tree like this:

             18
            /   \
          20     13
```

中序遍历思想，不过是先遍历右子树， root加上右边的。

```python
class Solution:
    def convertBST(self, root: TreeNode) -> TreeNode:
        self.sum = 0
        self.helper(root)
        return root
        
    def helper(self, root):
        if root==None:
            return
        self.helper(root.right)
        
        tmp = self.sum
        self.sum += root.val
        root.val += tmp
        
        self.helper(root.left)
```

## 二叉查找树的最近公共祖先

[235. Lowest Common Ancestor of a Binary Search Tree (Easy)](https://leetcode.com/problems/lowest-common-ancestor-of-a-binary-search-tree/description/)

```html
        _______6______
      /                \
  ___2__             ___8__
 /      \           /      \
0        4         7        9
        /  \
       3   5

For example, the lowest common ancestor (LCA) of nodes 2 and 8 is 6. Another example is LCA of nodes 2 and 4 is 2, since a node can be a descendant of itself according to the LCA definition.
```

```java
public TreeNode lowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q) {
    if (root.val > p.val && root.val > q.val) 
         return lowestCommonAncestor(root.left, p, q);
    if (root.val < p.val && root.val < q.val) 
         return lowestCommonAncestor(root.right, p, q);
    return root;
}
```

## 二叉树的最近公共祖先

[236. Lowest Common Ancestor of a Binary Tree (Medium) ](https://leetcode.com/problems/lowest-common-ancestor-of-a-binary-tree/description/)

```html
       _______3______
      /              \
  ___5__           ___1__
 /      \         /      \
6        2       0        8
        /  \
       7    4

For example, the lowest common ancestor (LCA) of nodes 5 and 1 is 3. Another example is LCA of nodes 5 and 4 is 5, since a node can be a descendant of itself according to the LCA definition.
```

```python
class Solution(object):
    def lowestCommonAncestor(self, root, p, q):
        """
        :type root: TreeNode
        :type p: TreeNode
        :type q: TreeNode
        :rtype: TreeNode
        """
        if root==None or root==p or root==q:
            return root
        
        left = self.lowestCommonAncestor(root.left, p, q)
        right = self.lowestCommonAncestor(root.right, p , q)
        
        if left == None or right==None:
            return left or right
        else:
            return root
```

## 从有序数组中构造二叉查找树

[108. Convert Sorted Array to Binary Search Tree (Easy)](https://leetcode.com/problems/convert-sorted-array-to-binary-search-tree/description/)

```java
public TreeNode sortedArrayToBST(int[] nums) {
    return toBST(nums, 0, nums.length - 1);
}

private TreeNode toBST(int[] nums, int sIdx, int eIdx){
    if (sIdx > eIdx) return null;
    int mIdx = (sIdx + eIdx) / 2;
    TreeNode root = new TreeNode(nums[mIdx]);
    root.left =  toBST(nums, sIdx, mIdx - 1);
    root.right = toBST(nums, mIdx + 1, eIdx);
    return root;
}
```

## 根据有序链表构造平衡的二叉查找树

[109. Convert Sorted List to Binary Search Tree (Medium)](https://leetcode.com/problems/convert-sorted-list-to-binary-search-tree/description/)

```html
Given the sorted linked list: [-10,-3,0,5,9],

One possible answer is: [0,-3,9,-10,null,5], which represents the following height balanced BST:

      0
     / \
   -3   9
   /   /
 -10  5
```

```python
class Solution:
    def sortedListToBST(self, head):
        if head == None:
            return None
        if head.next == None:
            return TreeNode(head.val)
        premid = self.pre_mid(head) 
        mid = premid.next
        midnext = mid.next
        premid.next = None
        t = TreeNode(mid.val)
        t.left = self.sortedListToBST(head)
        t.right = self.sortedListToBST(midnext)
        return t
    def pre_mid(self, head):
        pre = head
        slow = head
        fast = head.next
        while fast!=None and fast.next!=None:
            pre = slow
            slow = slow.next
            fast = fast.next.next
        return pre
```

## 在二叉查找树中寻找两个节点，使它们的和为一个给定值

[653. Two Sum IV - Input is a BST (Easy)](https://leetcode.com/problems/two-sum-iv-input-is-a-bst/description/)

```html
Input:

    5
   / \
  3   6
 / \   \
2   4   7

Target = 9

Output: True
```
应该注意到，这一题不能用分别在左右子树两部分来处理这种思想，因为两个待求的节点可能分别在左右子树中。

```python
class Solution:
    def findTarget(self, root: TreeNode, k: int) -> bool:
        traversed = []
        flag = False
        def inorder(node):
            nonlocal flag
            if node==None:
                return None
            if k-node.val in traversed:
                flag = True
                return
            traversed.append(node.val)
            inorder(node.left)
            inorder(node.right)

        inorder(root)
        # print(flag)
        if flag==True:
            return True
        # print(traversed)
        return False
```

## 在二叉查找树中查找两个节点之差的最小绝对值

[530. Minimum Absolute Difference in BST (Easy)](https://leetcode.com/problems/minimum-absolute-difference-in-bst/description/)

```html
Input:

   1
    \
     3
    /
   2

Output:

1
```

利用二叉查找树的中序遍历为有序的性质，计算中序遍历中临近的两个节点之差的绝对值，取最小值。

```python
class Solution:
    def getMinimumDifference(self, root: TreeNode) -> int:
        pre = None
        mindiff = float("inf")
        def inorder(node):
            nonlocal pre, mindiff
            if node == None:
                return
            
            inorder(node.left)
            
            if pre==None:
                pre = node.val
            else:
                mindiff = min(mindiff, node.val-pre)
                pre = node.val
            # print(pre, mindiff)
            inorder(node.right)
        inorder(root)
        return mindiff
```

## 寻找二叉查找树中出现次数最多的值

[501. Find Mode in Binary Search Tree (Easy)](https://leetcode.com/problems/find-mode-in-binary-search-tree/description/)

```html
   1
    \
     2
    /
   2

return [2].
```

答案可能不止一个，也就是有多个值出现的次数一样多。

```python
class Solution:
    def findMode(self, root: TreeNode) -> List[int]:
        max_cnt = 0
        cur_cnt = 0
        pre_val = None
        ret = []
        
        def inorder(root):
            nonlocal max_cnt, cur_cnt, pre_val, ret
            if root==None:
                return
            inorder(root.left)
            if root.val==pre_val:
                cur_cnt += 1
            else:
                cur_cnt = 1
            if cur_cnt > max_cnt:
                ret = [root.val]
                max_cnt = cur_cnt
            elif cur_cnt==max_cnt:
                ret.append(root.val)
            # print(cur_cnt, ret)
            pre_val = root.val
            
            inorder(root.right)
        
        inorder(root)
        return ret
```

# Trie

<div align="center"> <img src="https://gitee.com/CyC2018/CS-Notes/raw/master/docs/pics/5c638d59-d4ae-4ba4-ad44-80bdc30f38dd.jpg"/> </div><br>

Trie，又称前缀树或字典树，用于判断字符串是否存在或者是否具有某种字符串前缀。

## 实现一个 Trie

[208. Implement Trie (Prefix Tree) (Medium)](https://leetcode.com/problems/implement-trie-prefix-tree/description/)

```python
class Trie:
    def __init__(self):
        """
        Initialize your data structure here.
        """
        self.trie = {}
    def insert(self, word: str) -> None:
        """
        Inserts a word into the trie.
        """
        t = self.trie
        for c in word:
            if c not in t:
                t[c] = {}
            t = t[c]
        t['#'] = {}
    def search(self, word: str) -> bool:
        """
        Returns if the word is in the trie.
        """
        t = self.trie
        for c in word:
            if c in t:
                t = t[c]
            else:
                return False
        if '#' in t:
            return True
        else:
            return False
        
    def startsWith(self, prefix: str) -> bool:
        """
        Returns if there is any word in the trie that starts with the given prefix.
        """
        t = self.trie
        for c in prefix:
            if c in t:
                t = t[c]
            else:
                return False
        return True
```

## 实现一个 Trie，用来求前缀和

[677. Map Sum Pairs (Medium)](https://leetcode.com/problems/map-sum-pairs/description/)

```html
Input: insert("apple", 3), Output: Null
Input: sum("ap"), Output: 3
Input: insert("app", 2), Output: Null
Input: sum("ap"), Output: 5
```
```python
class MapSum:
    def __init__(self):
        """
        Initialize your data structure here.
        """
        self.trie = {}
        self.s = 0
    def insert(self, key, val) :
        t = self.trie
        for c in key:
            if c not in t:
                t[c] = {}
            t = t[c]
        t['#'] = val
        # print(self.trie)

    def sum(self, prefix):
        t = self.trie
        for c in prefix:
            if c not in t:
                return 0
            t = t[c]
        # print(t)
        self.s = 0
        self.dfs(t)
        return self.s
    
    def dfs(self, t):
        for key in t.keys():
            # print(key)
            if '#' == key:
                self.s += t['#']
            else:
                self.dfs(t[key])
```
