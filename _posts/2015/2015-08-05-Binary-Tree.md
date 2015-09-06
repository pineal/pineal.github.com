---
layout: post
title: Binary Tree
categories:
- Algorithm
tags:
- Algorithm
- Binary Tree
---

##Definition of Binary Tree
Full Binary Tree
Complete Binary Tree

##Three different ways to traverse a binary Tree
DFS:
前序（pre-order, NLR）
中序（in-order, LNR）
后序（post-order, LRN）
BFS:
层序（level-order）

###Traverse Binary Tree Example
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


##Template For DFS Traverse Using Recursion
```py
def DFS_Recursive(root):
    if root:
      #if NLR: print(root.val)
      DFS_Recursive(root.left)
      #if LNR: print(root.val)
      DFS_Recursive(root.right)
      #if LRN: print(root.val)
```

##Template For DFS Traverse Using Divide and Conquer
```py
def DFS_DC(root):
  if root is None:
    return

  #Divide
  left = DFS_DC(root.left);
  right = DFS_DC(root.right);

  #Conquer
  #result = Merge from left and right
  return result
```

##Template For DFS Traverse Using None-Recursion(NLR and LNR)

```py
def DFS_Stack(root):
  s = []
  while root or s:
    if root:
      #if NLR: print(root.val) 前序
      s.append(root)
      root = root.left
    else:
      root = s.pop()
      #if LNR: print(root.val) 中序
      root = root.right
```
##Template For DFS Traverse Using None-Recursion(LRN)
```py
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
```

##Template For BFS Traverse Using 2 Queues
##Template For BFS Traverse Using 1 Queue and Dummy Node
##Template For BFS Traverse Using 1 Queue(Best)

```py
from collections import deque

def BFS_oneQueue(root):
  if not root: return
  q = deque([root])
  while q:
    root = q.popleft()
    print(root.val)
    if root.left: q.append(root.left)
    if root.right: q.append(root.right)
```


##FAQ
###Difference of DFS and BFS
###Difference of Recursion and Non-Recursion
