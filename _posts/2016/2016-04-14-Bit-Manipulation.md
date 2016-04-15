---
layout: post
title: Bit Manipulation
categories:
- Algorithm
tags:
- Algorithm
- Bit Manipulation
---

＃ 位操作小结


## Count 1 in Binary

数的二进制表示中有多少位1. 有两种方法。

1. 每次把各位的数和 1 做 & 运算，然后计数，右移进行下一位。

```cpp
class Solution {
public:
    /**
     * @param num: an integer
     * @return: an integer, the number of ones in num
     */
    int countOnes(int num) {
        // write your code here
        int counter = 0;
        for (int i = 0; i < sizeof(int)*8; i++) {
            counter += num & 1;
            num >>= 1;
        }
        return counter;
    }
};
```

2. 把 num 和 num - 1 做 & 运算， 直到num为0，有多少次运算就有多少个1. 因为每次“&”都会去掉num最右边的1.


```cpp
class Solution {
public:
    /**
     * @param num: an integer
     * @return: an integer, the number of ones in num
     */
    int countOnes(int num) {
        // write your code here
        int counter = 0;
        while (num) {
            num &= num - 1;
            counter++;
        }
        return counter;
    }
};
```

这个用法判断是不是2的整数次幂很容易，因为2的整数次幂必然只有1个bit的1。

```cpp
bool checkPowerOf2(int n) {
    // write your code here
    return n > 0 && ((n & (n - 1)) == 0);
}
```


## Flip Bits

用到了xor的性质。


```cpp
class Solution {
public:
    /**
     *@param a, b: Two integer
     *return: An integer
     */
    int bitSwapRequired(int a, int b) {
        // write your code here
        int counter = 0;
        int c = a^b;
        for (int i = 0; i < 32; i++) {
            counter += c & 1;
            c >>= 1;
        }
        return counter;
    }
};
```

## A + B problem

这题。

```cpp
class Solution {
    /*
     * param a: The first integer
     * param b: The second integer
     * return: The sum of a and b
     */
    public int aplusb(int a, int b) {
        // 主要利用异或运算来完成
        // 异或运算有一个别名叫做：不进位加法
        // 那么a ^ b就是a和b相加之后，该进位的地方不进位的结果
        // 然后下面考虑哪些地方要进位，自然是a和b里都是1的地方
        // a & b就是a和b里都是1的那些位置，a & b << 1 就是进位
        // 之后的结果。所以：a + b = (a ^ b) + (a & b << 1)
        // 令a' = a ^ b, b' = (a & b) << 1
        // 可以知道，这个过程是在模拟加法的运算过程，进位不可能
        // 一直持续，所以b最终会变为0。因此重复做上述操作就可以
        // 求得a + b的值。
        while (b != 0) {
            int _a = a ^ b;
            int _b = (a & b) << 1;
            a = _a;
            b = _b;
        }
        return a;
    }
};
```


## Update Bits

Given two 32-bit numbers, N and M, and two bit positions, i and j. Write a method to set all bits between i and j in N equal to M (e g , M becomes a substring of N located at i and starting at j)
You can assume that the bits j through i have enough space to fit all of M. That is, if M=10011， you can assume that there are at least 5 bits between j and i. You would not, for example, have j=3 and i=2, because M could not fully fit between bit 3 and bit 2.

把一个数的 ［j， i］ 之间的bits 用另一个数去填充。

```cpp
class Solution {
public:
    /**
     *@param n, m: Two integer
     *@param i, j: Two bit positions
     *return: An integer
     */
    int updateBits(int n, int m, int i, int j) {
        // write your code here
        int right_part = n & ((1 << i) - 1);
        // The behavior of right shift >= 32 is undefined in C++.
        int left_part = j >= 31 ? 0 : (n >> (j + 1)) << (j + 1);
        return left_part | (m << i) | right_part;
    }
};
```

先用一个 mask 把 i 右边的数给取出来。
左边的数要这么取：先把bits右移 j + 1位，然后再左移 j + 1位，这样右边的数就都清空了。
最后一步把 m 左移后再把左边部分和右边部分用 ｜ 粘起来。
这种思想还可以用来做高地位互换等。把前一半的数右移，把后一半的数左移，然后 ｜ 起来。

## swap two variables

如何不用第三个临时变量来交换两个数。

```cpp
void swap(int &a, int &b){
	if (a != b){
		  a ^= b;
      b ^= a;
      a ^= b;
	}
}
```

## abs

```cpp
//取符号位
	int a = -100;
	int i = a >> 31;
	//i = 0 正数
	if(i == 0){
		printf("%d\n",a);
	}
	//i = 1 负数
	else{
		printf("%d\n",~a + 1);
	}
```
