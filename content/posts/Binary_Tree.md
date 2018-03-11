---
layout: post
title: Binary Tree
categories:
- Algorithm
tags:
- Binary Tree
date : "2015-08-05T00:00:00"
---

## Definition of Binary Tree
1. Full Binary Tree
  全部都填满的树。

2. Complete Binary Tree
  除了最后一行其他都填满，最后一行的最后一个之前（左边）全部是满的。

3. Balanced Binary Tree
  左右子树的高度最多差1. Height of the tree: $$O(log(n))$$

4. Binary Search Tree
  将二叉树按inorder方式遍历，是递增的。
## Three different ways to traverse a binary Tree

- DFS: 前序（pre-order, NLR）
- DFS: 中序（in-order, LNR）
- DFS: 后序（post-order, LRN）
- BFS: 层序（level-order）

### Traverse Binary Tree Example
[A:B, C]
[B:D]
[D:None]
[C:E,F]
[E:G,H]
[G:None]
[H:None]
[F:I]
[I:None]


NLR: A B D C E G H F I

LNR: D B A G E H C F I

LRN: D B G H E I F C A


## Template For DFS Traverse Using Recursion(NLR and LNR)

~~~cpp
void helper(TreeNode* node, vector<int> & rst){
    if (node != nullptr){
//      rst.emplace_back(root -> val);  If it is PreOder
        helper(node -> left, rst);
//      rst.emplace_back(root -> val);  If it is InOrder
        helper(node -> right, rst);
//      rst.emplace_back(root -> val);  If it is PostOrder
    }
}
vector<int> DFS_Traversal(TreeNode* root) {
    // write your code here
    vector<int> rst;
    helper(root, rst);
    return rst;
}
~~~

## Template For DFS Traverse Using None-Recursion(NLR and LNR)

~~~cpp
vector<int> DFS_Traversal(TreeNode *root) {
    // write your code here
    vector<int> rst;
    if (root == nullptr) return rst;
    stack<TreeNode*> s;

    while (!s.empty() || root != nullptr){
        if (root != nullptr){
//          rst.emplace_back(root -> val);  If Pre-Order
            s.push(root);
            root = root -> left;
        }
        else{
            root = s.top();
            s.pop();
//          rst.emplace_back(root -> val);  If In-Order
            root = root -> right;
        }            
    }
return rst;
}
~~~

## Template For DFS Traverse Using None-Recursion(LRN)
~~~py
def DFS_Stack_LRN(root):
  s = []
  pre = None
  while root or s:
    if root:
      s.append(root)
      root = root.left
    elif s[-1].right != pre:
      root = s[-1].right
      pre = None
    else:
      pre = s.pop()
      print(pre.val)
~~~


## Template For BFS Traverse Using 1 Queue(Best)

~~~cpp
vector<vector<int>> levelOrder(TreeNode *root) {
    // write your code here
    vector<vector<int>> rst;
    if (root == nullptr) return rst;

    queue<TreeNode*> nodes;
    nodes.push(root);

    while(!nodes.empty()){
        vector<int> level;
        int size = nodes.size();
        for (int i = 0; i < size; i++){
            TreeNode* node = nodes.front();
            nodes.pop();
            level.emplace_back(node -> val);
            if (node -> left != nullptr) nodes.push(node -> left);
            if (node -> right != nullptr) nodes.push(node -> right);    
        }
        rst.emplace_back(level);
    }
return rst;    
}
~~~

## ?Morris Traverse?

## FAQ
### Difference of DFS and BFS
DFS using stacks, and BFS using queues if Non-Recursion
### Difference of Recursion and Non-Recursion
Recursion is dangerous when memory resource is limited: stack may overflow;
However Non-Recursion method occupies more space
