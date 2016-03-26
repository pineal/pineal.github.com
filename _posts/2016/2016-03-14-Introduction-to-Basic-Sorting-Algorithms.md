---
layout: post
title: Introduction to Basic Sorting Algorithms
categories:
- Algorithm
tags:
- Sorting
---
假设这里的排序都是升序。

## Insertion Sort:
插入排序。假设一个长度为N的数组A[]，总体过程过程为，从index为1开始到N-1，使得A[0,index]是一个排好序的数组。

具体过程为，把A[index]从index到0逐一比较（这里的顺序一定要从后往前，因为已经是排好序的了），找到第一个比他小的数，然后插到这个数的后面，当然原来位置及以后的数都要往后挪一位。实际代码中是在for循环中加入找到这个小数的条件，每次都往后挪，循环完了才插入。

举个栗子：

| Original | 34  8  64  51  32  21 | Position Moved |
| -- | --  --  --  --  --  -- | -- |
| after i = 1 | 8  34  64  51  32  21 | 1 |
| after i = 2 | 8  34  64  51  32  21 | 0 |
| after i = 3 | 8  34  51  64  32  21 | 1 |
| after i = 4 | 8  32  34  51  64  21 | 3 |
| after i = 5 | 8  21  32  34  51  64 | 4 |

~~~cpp
template<typename T>
void insertion_sort(T arr[], int len){
  for (int i = 1; i < len; i++){
    int temp = arr[i];
      for (int j = i; j > 0 && arr[j-1]>temp; j--){ //实际比较的是j-1
        arr[j] = arr[j-1];  //全部往后移一位腾出位置等插入
      }
    arr[j] = temp;    //插入到腾出来的位置
  }
}
~~~

复杂度分析：


## Selection Sort:

选择排序。假设一个长度为N的数组A[]，总体过程过程为，从index为0开始到N-1，使得A[0,index]是一个排好序的数组。

怎么跟插入排序这么像呢？

是挺像的，但是具体的过程是有区别。这区别就是“插入”和“选择”的区别。

插入排序是每次往前面那些已经排序好的数里“插入”进去，而选择排序则是，每次从这个数后面那些没排好序的数里“选择”到最小的，和这个数交换。

举个栗子：

| Original | 34  8  64  51  32  21 | Position Moved |
| -- | --  --  --  --  --  -- | -- |
| after i = 1 | 8  34  64  51  32  21 | 1 |
| after i = 2 | 8  21  64  51  32  34 | 4 |
| after i = 3 | 8  21  32  51  64  34 | 2 |
| after i = 4 | 8  21  32  34  64  51 | 2 |
| after i = 5 | 8  21  32  34  51  64 | 1 |

~~~cpp
template<typename T>
void selection_sort(T arr[], int len){
  for (int i = 0; i < len - 1; i++){  //len - 1即可，最后一次交换在倒数第一个和倒数第二个之间进行
    int min_index = i;
    for (int j = i + 1; j < len - 1; j++){
      //找到未排序的数组中最小的数的index
      if (arr[j]<arr[min_index]){
          min_index = j;
      }        
    }
    swap(arr[i], arr[min_index]);
  }
}
~~~

复杂度分析：
$O(n^2)$的复杂度。实际上是冒泡排序的一个优化，虽然最坏时间复杂度上是一样的。

## Merge Sort:
归并排序。分治法（divide and conquer）思想入门的算法。Recursively 递归入栈时将字符串分为左右两半，直到无法分割为止。出栈时再把这两半合并起来，在合并的过程中排序。最后所有的栈返回是一个排好序的数组。在用分治法的时候注意一下和纯递归求解的区别。

~~~cpp
//Merge part
  vector<int> merge(vector<int> A, vector<int> B) {
	  vector<int> rst;
	  std::vector<int>::iterator iter1 = A.begin(), iter2 = B.begin();
//this is not elegant, may be && and insert the rest may be better
	  while (iter1 != A.end() || iter2 != B.end()) {
	  	if (iter1 == A.end()) {
		  	rst.insert(rst.end(), iter2, B.end());
		  	break;
		  }
		  if (iter2 == B.end()) {
		  	rst.insert(rst.end(), iter1, A.end());
		  	break;
		  }
		  if (*iter1 < *iter2) {
			  rst.emplace_back(*iter1);
			  iter1++;
		  }
		  else {
			  rst.emplace_back(*iter2);
			  iter2++;
		  }
	  }
	  return rst;
  }

//Key part of Merge sort, recursive function
  vector<int> MSort(vector<int> array, int left, int right) {
	  vector<int> rst;
	  if (left == right) {
		  rst.emplace_back(array[left]);
		  return rst;
	  }
	  int mid = left + (right - left) / 2;
	  vector<int> leftArr = MSort(array, left, mid);
	  vector<int> rightArr = MSort(array, mid + 1, right);
	  rst = merge(leftArr, rightArr);
	  return rst;
  }
//Driver for Merge Sort
  vector<int> mergeSort(vector<int> array) {
    if (array.size() <= 1) return array;
	  vector<int> rst;
	  rst = MSort(array, 0, array.size() - 1);
	  return rst;
  }
~~~

复杂度分析： $$O(nlog(n))$$：画出递归树，一共$$log(n)$$层, 每一层是$$O(n)$$的复杂度.

## Quick Sort:
快速排序。快速排序是一个比较复杂的问题。基本的思想为：

1. 选取一个pivot。
2. 所有比pivot小的数放在pivot的左边，所有比pivot大的数放在pivot的右边。
3. 分割数组：对pivot两边的数组递归重复以上步骤，直到不能分割。

具体有哪些实现呢。
先来看第一种方法。这是一个textbook的解法。

~~~cpp
 int median(vector<int>& arr, int left, int right){
   // arr[left] <= arr[center] <= arr[right]
   int center = left + (right - left)/2;
   if (arr[left] > arr[center]) swap(&arr[left], &arr[center]);
   if (arr[left] > arr[right])  swap(&arr[left], &arr[right]);
   if (arr[center] > arr[right]) swap(&arr[center], &arr[right]);
   swap(&arr[center], &arr[right - 1]); //hide pivot
   return arr[right - 1];
 }

 void QSort(vector<int> & arr, int left, int right) {

//   if (left >= right) return;
   if (left + CutOff <= right){
     int pivot = median(arr, left, right);
     int i = left;
     int j = right - 1;
     for(;;){
      while (arr[++i] < pivot){}
       while (arr[--j] > pivot){}
      if (i < j) swap(&arr[i], &arr[j]);
       else  break;
     }
//   if (left + 1 != right){
       swap(&arr[i], &arr[right - 1]); //restore pivot
//   }  
     QSort(arr, left, i - 1);
     QSort(arr, i + 1, right);
   }
   else{
     insertion_Sort(arr, left, right - left + 1);
   }
 }

 vector<int> quickSort(vector<int> array) {
   if (array.size() <= 1) return array;
   QSort(array, 0, array.size() - 1);
   return array;
 }
~~~

另一个解法。基本思想是一样的。每次都要保持所有比pivot小的数放在pivot的左边，所有比pivot大的数放在pivot的右边这个条件。
实现的过程为，选取两块挡板，分别从数组的头和尾往中间靠拢，直到挡板相遇。在每一次的循环中，保证第一块挡板左边的数都小于pivot，第二块挡板右边的数都大于pivot。

事实上根据上面这个general rule来分，两个挡板指向的两个数只有四种情况：

1. a[i] < pivot, a[j] > pivot

2. a[i] < pivot, a[j] < pivot

3. a[i] > pivot, a[j] > pivot

4. a[i] >= pivot, a[j] <= pivot

那么对应刚才的rule该做着么呢？

1. 满足条件：移动挡板i++， j--

2. 满足前半个条件，移动挡板i++，继续检查条件

3. 满足后半个条件，移动挡板j--，继续检查条件

4. 不满足任何条件，但是一旦交换两个挡板上的数，即可让条件满足


来看下代码。


~~~cpp
  void QuickSort(vector<int> & array, int left, int right){
    if (left > right) return;
    int pivot_index = (left + right)/2;
    int pivot =  array[pivot_index];
    int left_bound = left;
    int right_bound = right - 1;
    //hide the pivot in the rightmost
    std::swap(array[pivot_index], array[right]);
    //three regions:
    //1. [0, leftbound - 1] : all elements smaller than pivot should be here
    //2. [leftbound, rightbound]: to be discovered, scan the element in a[leftbound], and move leftbound every step
    //3. [rightbound + 1, array.size() - 1] all elements bigger than pivot should be here
    while (left_bound <= right_bound) {
      //check two
      if (array[left_bound] < pivot) {
      // obey all three rules, move leftbound
        ++left_bound;
      }
      else if (array[right_bound] > pivot) {
        --right_bound;
      }
      else {
        //array[left_bound] > pivot && array[right_bound < pivot]
        std::swap(array[left_bound++], array[right_bound--]);      
      }
    }
    //restore the pivot to the original position
    std::swap(array[left_bound], array[right]);
    //partition and recursion
    QuickSort(array, left, left_bound - 1);
    QuickSort(array, left_bound + 1, right);
  }

  vector<int> quickSort(vector<int> array) {
    if (array.size() <= 1) return array;
    QuickSort(array, 0, array.size() - 1);
    return array;
  }
~~~

这样的解法可以归结为一种类型。比如一堆数中只有两种，三种四种数，那么就可以对应个数的挡板将数分割成相应区域，每次检查条件是否满足。Eg: Rainbow Sort。这样的做法复杂度只需要$O(n)$?
