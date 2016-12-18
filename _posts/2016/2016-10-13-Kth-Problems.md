---
layout: post
title: Kth Problems
categories:
- Algorithm
tags:
- Heap
- priority Queue
---

# Kth Problems

套路：找第K个的问题，最常用的做法就是用优先队列来实现，根据题意用最大堆或者最小堆把时间复杂度优化到 O(nlogk).

### Merge k Sorted Lists

~~~cpp

//Time O(nlogk)
//Space O(n)
//provides greater
struct Cmp {
  bool operator() (ListNode* n1, ListNode* n2) {
    return n1 -> val > n2 -> val;
  }
};

class Solution {
public:
  ListNode* mergeKLists(vector<ListNode*>& lists) {
    //min_heap needs a greater comparator
    //Method 1: redefine functor
    //priority_queue<ListNode*, vector<ListNode*>, Cmp> min_heap;
    //Method 2: Lambda
    auto cmp = [](ListNode* n1, ListNode* n2) {return n1 -> val > n2 -> val;};
    priority_queue<ListNode*, vector<ListNode*>, decltype(cmp)> min_heap(cmp);
    //maintain the min_heap of size k instead of all nodes
    // klogn => nlogk
    for (int i = 0; i < lists.size(); i++) {
      if (lists[i]) {
        min_heap.emplace(lists[i]);
      }
    }

    ListNode* dummy = new ListNode(0);
    ListNode* cur = dummy;
    while (!min_heap.empty()) {
      ListNode* temp = min_heap.top();
      cur -> next = temp;
      min_heap.pop();
      if (temp -> next) {
        min_heap.emplace(temp -> next);
      }
      cur = cur -> next;
    }
    return dummy -> next;
  }
};
~~~

### Kth Smallest Number In Sorted Matrix

~~~cpp

class Cell {
public:
    int row;
    int col;
    int value;
    Cell(int _row, int _column, int _value) {
        row = _row;
        col = _column;
        value = _value;
    }

    bool operator < (const Cell & c) const {
        return value <= c.value;
    }

    bool operator > (const Cell & c) const {
        return value > c.value;
    }
};

int kthSmallest(vector<vector<int>> m, int k) {
    priority_queue<Cell, vector<Cell>, greater<Cell>> min_heap;
    min_heap.emplace(Cell(0, 0, m[0][0]));
    size_t num_row = m.size();
    size_t num_col = m[0].size();
    vector<vector<bool>> visited(num_row, vector<bool>(num_col, false));
    visited[0][0] = true;
    for (int i = 0; i < k - 1; i++) {
        Cell c = min_heap.top();
        min_heap.pop();
        if (c.row + 1 < num_row && visited[c.row + 1][c.col] == false) {
                min_heap.emplace(Cell(c.row + 1, c.col, m[c.row + 1][c.col]));
                visited[c.row + 1][c.col] = true;
        }

        if (c.col + 1 < num_col && visited[c.row][c.col + 1] == false) {
                min_heap.emplace(Cell(c.row, c.col + 1, m[c.row][c.col + 1]));
                visited[c.row][c.col + 1] = true;
        }
    }
    return min_heap.top().value;
}
~~~

### Kth Smallest Sum In Two Sorted Arrays

~~~cpp
class Cell {
public:
  int i;
  int j;
  int sum;
  Cell(int _i, int _j, int _sum) {
    i = _i;
    j = _j;
    sum = _sum;
  }

  bool operator < (const Cell & c) const {
    return sum <= c.sum;
  }

  bool operator > (const Cell & c) const {
    return sum > c.sum;
  }
};

class Solution {
 public:
  int kthSum(vector<int> a, vector<int> b, int k) {
    // Write your solution here
    priority_queue<Cell, vector<Cell>, greater<Cell>> min_heap;
    vector<vector<bool>> visited(a.size(), vector<bool>(b.size(), false));
    visited[0][0] = true;
    min_heap.emplace(Cell(0, 0, a[0] + b[0]));
    for (int i = 0; i < k - 1; i++) {
      Cell cur = min_heap.top();
      min_heap.pop();
      if (cur.i + 1 < a.size() && !visited[cur.i + 1][cur.j]) {
        int sum = a[cur.i + 1] + b[cur.j];
        min_heap.emplace(Cell(cur.i + 1, cur.j, sum));
        visited[cur.i + 1][cur.j] = true;
      }

      if (cur.j + 1 < b.size() && !visited[cur.i][cur.j + 1]) {
        int sum = a[cur.i] + b[cur.j + 1];
        min_heap.emplace(Cell(cur.i, cur.j + 1, sum));
        visited[cur.i][cur.j + 1] = true;
      }
    }
    return min_heap.top().sum;
  }
};
~~~

### Kth Smallest With Only 3, 5, 7 As Factors
~~~cpp
  long kth(int k) {
    // Write your solution here.
    priority_queue<long, vector<long>, greater<long>> min_heap;
    min_heap.emplace(105);
    set<long> visited;
    visited.emplace(105);
    for (int i = 0; i < k - 1; i++) {
      long cur = min_heap.top();
      min_heap.pop();
      if (visited.find(cur * 3) == visited.end()) {
        min_heap.emplace(cur * 3);
        visited.emplace(cur * 3);
      }

      if (visited.find(cur * 5) == visited.end()) {
        min_heap.emplace(cur * 5);
        visited.emplace(cur * 5);
      }      

      if (visited.find(cur * 7) == visited.end()) {
        min_heap.emplace(cur * 7);
        visited.emplace(cur * 7);
      }      
    }

    return min_heap.top();
  }

~~~

### Kth Closest Point

~~~cpp
class Point {
public:
    int x;
    int y;
    int z;
    double dis;
    Point (int _x, int _y, int _z, double _dis) {
        x = _x;
        y = _y;
        z = _z;
        dis = _dis;
    }

    bool operator < (const Point & p1) const {
        return dis <= p1.dis;
    }

    bool operator > (const Point & p1) const {
        return dis > p1.dis;
    }

};

class Solution {
 public:
    vector<int> closest(vector<int> a, vector<int> b, vector<int> c, int k) {
        priority_queue<Point, vector<Point>, greater<Point>> min_heap;
        set<vector<int>> visited;
        double d = sqrt(a[0] * a[0] + b[0] * b[0] + c[0] * c[0] + 0.0);
        Point* start = new Point(0,0,0,d);
        min_heap.emplace(*start);
        visited.emplace(vector<int>{0,0,0});
        for (int i = 0; i < k - 1; i++) {
            Point p = min_heap.top();
            min_heap.pop();
            if (p.x + 1 < a.size()) {
                double d = sqrt(a[p.x + 1] * a[p.x + 1] + b[p.y] * b[p.y] + c[p.z] * c[p.z] + 0.0);
                Point* temp = new Point(p.x + 1,p.y,p.z,d);
                if (visited.find({p.x + 1,p.y,p.z}) == visited.end()) {
                    min_heap.emplace(*temp);
                    vector<int> v = {p.x + 1, p.y, p.z};
                    visited.emplace(v);
                }
            }
            if (p.y + 1 < b.size()) {
                double d = sqrt(a[p.x] * a[p.x] + b[p.y + 1] * b[p.y + 1] + c[p.z] * c[p.z] + 0.0);
                Point* temp = new Point(p.x,p.y + 1,p.z,d);
                if (visited.find({p.x,p.y + 1,p.z}) == visited.end()) {
                    min_heap.emplace(*temp);
                    vector<int> v = {p.x,p.y + 1,p.z};
                    visited.emplace(v);
                }
            }

            if (p.z + 1 < c.size()) {
                double d = sqrt(a[p.x] * a[p.x] + b[p.y] * b[p.y] + c[p.z + 1] * c[p.z + 1] + 0.0);
                Point* temp = new Point(p.x,p.y,p.z + 1,d);
                if (visited.find({p.x,p.y,p.z + 1}) == visited.end()) {
                    min_heap.emplace(*temp);
                    vector<int> v = {p.x,p.y,p.z + 1};
                    visited.emplace(v);
                }
            }      
        }

        Point rst = min_heap.top();
        return {a[rst.x], b[rst.y], c[rst.z]};
    }
};
~~~

visited尽量用bool数组表示，二维三维都可。不要存放node类的class，地址不一样。
