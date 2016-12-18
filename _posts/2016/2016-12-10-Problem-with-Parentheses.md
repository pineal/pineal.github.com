---
layout: post
title: Problems with Parentheses
categories:
- Algorithm
tags:
- stack
- DFS
---

# 括号问题

对括号的处理，最经典的做法是对左右括号计数，因为一定会成对。也可以用栈的性质，来配对。

### 生成所有的括号

~~~cpp
class Solution {
public:
    vector<string> generateParenthesis(int n) {
        vector<string> rst;
        string cur;
        dfs(cur, rst, 0, 0, n);
        return rst;
    }

    void dfs(string & cur, vector<string> & rst, int l, int r, int n) {
        if (l + r == 2 * n) {
            rst.emplace_back(cur);
            return;
        }

        if (l < n) {
            cur += '(';
            dfs(cur, rst, l + 1, r, n);
            cur.pop_back();
        }

        if (r < l) {
            cur += ')';
            dfs(cur, rst, l, r + 1, n);
            cur.pop_back();
        }
    }
};
~~~


### 验证合法的括号

~~~cpp
class Solution {
public:
    bool isValid(string s) {
        stack<char> st;
        for (auto c : s)
            if (!st.empty() &&
                ((c == ')' && st.top() == '(') ||
                 (c == '}' && st.top() == '{') ||
                 (c == ']' && st.top() == '['))
               ) {
                st.pop();    
               }
            else {
                st.push(c);
            }
        return st.empty();
    }
};
~~~
