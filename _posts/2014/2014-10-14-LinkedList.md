---
layout: post
title: LinkedList by java
categories:
- Data Structure
tags:
- LinkedList
---

##LinkedList Introduction

- Only use as much space as you need at a time.
- Can insert and delete from middle without shifting left or right by one. 
- However no random access based on location. 

##Linked list implementations

- do not traverse a linked list, e.g, add(i, object), remove(i), set(i, object)


```java
LinkedList<Integer> list = new LinkedList<Integer>()
```

- Put the elements at the end, takes $O(1)$.

```java
list.add(10);
list.add(20);
list.add(30);
```

- LinkedList uses a tail pointer.
- Operations that access the beginning or end are efficient:

```java
list.addFirst()
list.getFirst()
list.getLast()
list.removeFirst()
list.removeLast()
```
##ListIterator

```
ListIterator<String> iter = list.listIterator();
```

- next() will move the next space between two elements, and return the element before the space. 
- hasNext() to make sure there exist next element after the current space

```
ListIterator<Integer> iter = list.listIterator();
while(iter.hasNext()){
	int current = iter.next();
	current += 10;
	}
}
```
Modifying values in element can be achieved by iter.set() to update.

```
while(iter.hasNext()){
	int current = iter.next();
	iter.set(current+10);
	}
}
```

Example of adding element : duplicate 

```
listItertator<String> iter = list.listIterator();
while (iter.hasNext()){
	String current = iter.next();
	iter.add(name);
}
```

Example of removing element:

```
listIterator<Stirng> iter = list.listIterator();
while (iter.hasNext()){
	String current = iter.next();
	if (current < threshold){
		remove();
	}
}
```



