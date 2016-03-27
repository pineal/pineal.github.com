---
layout: post
title: Synchronization on Multithreads
categories:
- Operating System
tags:
- C
- POSIX
- OS
- threads
---
##Threads

###Example: remote login function with or without threads

**Life Without Threads**

~~~cpp
void rlogind(int r_in, int r_out, int l_in, int l_out) {
	fd_set in = 0, out;
	int want_l_write = 0, want_r_write = 0;
	int want_l_read = 1, want_r_read = 1;
	int eof = 0, tsize, fsize, wret; char fbuf[BSIZE], tbuf[BSIZE];
	fcntl(r_in, F_SETFL, O_NONBLOCK);
	fcntl(r_out, F_SETFL, O_NONBLOCK);
	fcntl(l_in, F_SETFL, O_NONBLOCK);
	fcntl(l_out, F_SETFL, O_NONBLOCK);
	while(!eof) {
		FD_ZERO(&in);
		FD_ZERO(&out);
		if (want_l_read)
			FD_SET(l_in, &in);
		if (want_r_read)
			FD_SET(r_in, &in);
		if (want_l_write)
			FD_SET(l_out, &out);
		if (want_r_write)
			FD_SET(r_out, &out);
		select(MAXFD, &in, &out, 0, 0);
		if (FD_ISSET(l_in, &in)) {
			if ((tsize = read(l_in, tbuf, BSIZE)) > 0) {
				want_l_read = 0;
				want_r_write = 1;
			}
			else
				eof = 1;
		}
		if (FD_ISSET(r_in, &in)) {
			if ((fsize = read(r_in, fbuf, BSIZE)) > 0) {
				want_r_read = 0;
				want_l_write = 1;
			}
			else
				eof = 1;
		}
		if (FD_ISSET(l_out, &out)) {
			if ((wret = write(l_out, fbuf, fsize)) == fsize) {
				want_r_read = 1;
				want_l_write = 0;
			}
			else if (wret >= 0)
				tsize -= wret;
			else
				eof = 1;
		}
		if (FD_ISSET(r_out, &out)) {
			if ((wret = write(r_out, tbuf, tsize)) == tsize) {
				want_l_read = 1;
				want_r_write = 0;
			}
			else if (wret >= 0)
				tsize -= wret;
			else
				eof = 1;
		}
	}
}
~~~

**Life with threads**

~~~cpp
incoming(int r_in, int l_out) {
	int eof = 0;
	char buf[BSIZE];
	int size;
	while (!eof) {
		size = read(r_in, buf, BSIZE);
		if (size <= 0)
			eof = 1;
		if (write(l_out, buf, size) <= 0)
			eof = 1;
	}
}
~~~

~~~cpp
outgoing(int l_in, int r_out) {
	int eof = 0;
	char buf[BSIZE];
	int size;
	while (!eof) {
		size = read(l_in, buf, BSIZE);
		if (size <= 0)
			eof = 1;
		if (write(r_out, buf, size) <= 0)
			eof = 1;
	}
}~~~

##Mutex

###Definition

A new data type called mutex standing for Mutual exclusion is defined in POSIX. Mutex is used for:

- Code locking: only one thread is executing a particular piece of code at once.
- Data locking: only one thread is accessing a particular data structure at once.

###Example

The the problem of $x = x + 1$ can be solved by mutexes:

~~~cpp
pthread_mutex_t m = PTHREAD_MUTEX_INITIALIZER; // shared by both threadsint x; // dittopthread_mutex_lock(&m);x = x+1;pthread_mutex_unlock(&m);
~~~

~~~cpp
void proc1( ) {
	pthread_mutex_lock(&m1);
	/* use object 1 */
	pthread_mutex_lock(&m2);
	/* use objects 1 and 2 */
	pthread_mutex_unlock(&m2);
	pthread_mutex_unlock(&m1);
	}
~~~


~~~cpp
void proc2( ) {
	pthread_mutex_lock(&m2);
	/* use object 2 */
	pthread_mutex_lock(&m1);
	/* use objects 1 and 2 */
	pthread_mutex_unlock(&m1);
	pthread_mutex_unlock(&m2);}
~~~


##Deadlock

###Occur conditions
Generally the conditions below are **necessary but not sufficient** for deadlock:

- It must be possible for a thread to hold item it has removed from various container while waiting for items to become available in other containers.
- A thread cannot to yield the items it is holding
- Each container has a finite capacity
- A circular wait can exist. We say that thread A is waiting on thread B if B is holding items from containers from which A is waiting for items. Consider all threads Z for which there is some sequence of threads B, C, D, . . . , Y, such that A is waiting on B, B is waiting on C, C is waiting on D, . . . , and Y is waiting on Z. The set of such threads Z is known as the transitive closure of the relation waiting on. If A is contained in this transitive closure, then we say that a circular wait exists.

When threads can wait on only one of them at a time (while holding locks on any number of them), these conditions are **necessary and also sufficient**.

##Semaphores

We use the following notation to describe the semantics of P:

~~~
when (semaphore > 0)
[semaphore = semaphore – 1;]
~~~

The V operation is simpler: a thread atomically adds one to the value of the semaphore. We write this as:

~~~[semaphore = semaphore + 1;]
~~~

###Mutexes implementation

~~~cpp
semaphore S = 1;
void OneAtATime( ) {       P(S);       ...       /* code executed mutually exclusively */       ...       V(S);}
~~~

###Reader-Writers Problem

~~~cpp
void reader( ) {	when (writers == 0)		[readers++; ]		// read		[readers--;]}~~~

~~~cpp
reader(){
pthread_mutex_lock(&m);
while (!(writers == 0))
	pthread_cond_wait(
		&readersQ, &m);
readers++;
pthread_mutex_unlock(&m);
/* read */
pthread_mutex_lock(&m);
if (--readers == 0)
	pthread_cond_signal(&writersQ);
	pthread_mutex_unlock(&m);
}
~~~

~~~cpp
void writer( ) {when ((writers == 0) &&}   writers++;]// write[writers--;]}~~~
